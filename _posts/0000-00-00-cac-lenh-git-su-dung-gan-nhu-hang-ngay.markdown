---
layout: post
title:  "Các lệnh Git mình sử dụng gần như hàng ngày và best-practice flow cho từng use-case"
date: "2022-03-10 19:00:00 +0700"
last_modified_at: "2022-03-10 19:00:00 +0700"
categories: Uncategorized
---

Hồi mới đi làm, hàng ngày mình phải gõ đi gõ lại rất nhiều lệnh Git. Việc này đã khiến mình cảm thấy mất thời gian và chán nản. Vì vậy, mình đã nghĩ "Vậy sao không nghĩ tới việc tổng hợp các lệnh Git hay dùng và viết chúng thành một script nhỏ để sử dụng hàng ngày?"

# I. Setup local repository

Đây là những lệnh được gõ đầu tiên khi mình muốn làm việc với Git.
- `git init` - dùng để tạo local repository rỗng
- `git remote add origin <url>` - dùng để chỉ định remote repository URL

Hãy thử gõ các lệnh sau vào terminal nhé. <a href="https://github.com/hoagkub/Game_TheCat" title="https://github.com/hoagkub/Game_TheCat" target="_blank">https://github.com/hoagkub/Game_TheCat</a> là 1 project mà mình đã làm lúc còn đi học.

> <pre style="white-space: pre-wrap; word-break: keep-all;">
<b>⚠️ Note</b>
<i>Các dòng bắt đầu bằng dấu nhắc dollar ($) là lệnh, những dòng không có dấu nhắc là output của lệnh.</i>

    $ cd Game_TheCat
    $ git init
    Initialized empty Git repository in D:/Documents/SAU_DAI_HOC/Game_TheCat/.git/
    $ git remote add origin https://github.com/hoagkub/Game_TheCat

Các lệnh setup này thường chỉ được gõ một lần khi mình muốn init local repository nên sẽ không cần tạo script. Các lệnh về sau mới là những lệnh mà mình đã gõ đi gõ lại hàng ngày và nhận thấy rằng có thể gộp chúng lại thành một script nhỏ.

# II. Clean-checkout branch

Sau khi đã init và set remote repository URL, thì việc tiếp theo là checkout một branch từ remote repo (vì từ repository lặp lại khá nhiều nên mình sẽ viết tắt thành repo nhé)

Thông thường, việc này chỉ đơn giản là dùng lệnh:

    $ git checkout <branch-name>

Nhưng thực tế mình thấy khi dùng lệnh này sẽ có các trường hợp xảy ra: local repo dọn dẹp chưa sạch các file, branch chưa được fetch về `origin` ở dưới local, local repo mặc dù đã gọi checkout rồi nhưng vẫn chưa chuyển hẳn qua branch. 

Vì vậy để khắc phục những vấn đề này sẽ có các lệnh sau và mình đã tổng hợp chúng lại thành một file tên là `git-clean-checkout.bat`, đường dẫn tới folder chứa file này mình cũng cho nó vô environment variable `PATH` luôn.

## 1. Xóa tất cả các file/sub-folder chưa được đánh dấu tracking trong local.

    $ git clean -fd

## 2. Fetch toàn bộ branch tồn tại trên remote về local `origin`.

    $ git fetch origin
    remote: Enumerating objects: 14, done.
    remote: Counting objects: 100% (14/14), done.
    remote: Compressing objects: 100% (12/12), done.
    remote: Total 14 (delta 1), reused 14 (delta 1), pack-reused 0
    Unpacking objects: 100% (14/14), 12.14 KiB | 28.00 KiB/s, done.
    From https://github.com/hoagkub/Game_TheCat
    * [new branch]      main       -> origin/main

## 3. Fetch branch tên `main` trên remote về local `origin`.
   
    $ git fetch origin main
    From https://github.com/hoagkub/Game_TheCat
    * branch            main       -> FETCH_HEAD

Sau lệnh này, bạn hãy để ý `FETCH_HEAD` trong output.
## 4. Checkout branch tên `main`.
   
    $ git checkout main
    Switched to a new branch 'main'
    Branch 'main' set up to track remote branch 'main' from 'origin'.

## 5. Thêm các lệnh reset để chuyển hẳn qua `main`.

    $ git reset --hard FETCH_HEAD
    HEAD is now at e9583a3 Add Readme.md
    $ git reset --hard origin/main
    HEAD is now at e9583a3 Add Readme.md
    $ git clean -fd

Và đây là nội dung file `git-clean-checkout.bat`:

    @echo off
    setlocal EnableDelayedExpansion
    
    set dstBranch=%1
    
    echo. Checkouting '%dstBranch%' ...
    
    :: Update working directory to clean version of dstBranch
    echo. git clean -fd; git fetch origin; git fetch origin %dstBranch%; git checkout %dstBranch%; git reset --hard FETCH_HEAD; git reset --hard origin/%dstBranch%; git clean -fd
    git clean -fd
    git fetch origin
    git fetch origin %dstBranch%
    git checkout %dstBranch%
    git reset --hard FETCH_HEAD
    git reset --hard origin/%dstBranch%
    git clean -fd
    echo(

Ví dụ, file này mình để ở `D:/GitTools` thì mình sẽ add đường dẫn này vào enviroment.

| <a href="/assets/img/cac-lenh-git/GitTools.png" target="_blank">![Image not found.](/assets/img/cac-lenh-git/GitTools.png)</a> |
|:--:|
| File git-clean-checkout.bat được để trong D:/GitTools. |

| <a href="/assets/img/cac-lenh-git/AddGitTools.png" target="_blank">![Image not found](/assets/img/cac-lenh-git/AddGitTools.png)</a> |
|:--:|
| D:/GitTools được thêm vào Environment. Để xem hình to hơn, bạn click vào hình nhé.|

Sau này, mình chỉ việc gọi lệnh sau để checkout branch và sẽ tránh được các lỗi mình đã nói ở trên.

      $ git-clean-checkout main
       Checkouting 'main' ...
       git clean -fd; git fetch origin; git fetch origin main; git checkout main; git reset --hard FETCH_HEAD; git reset --hard origin/main; git clean -fd
      From https://github.com/hoagkub/Game_TheCat
       * branch            main       -> FETCH_HEAD
      Already on 'main'
      Your branch is up to date with 'origin/main'.
      HEAD is now at e9583a3 Add Readme.md
      HEAD is now at e9583a3 Add Readme.md

Bây giờ, mình sẽ tách branch `edit-some-stuff` từ `main` bằng lệnh sau:

    $ git checkout -b edit-some-stuff
    Switched to a new branch 'edit-some-stuff'

Mọi chỉnh sửa của mình đều sẽ add, commit và push lên branch `edit-some-stuff` này.

# III. Add, commit và push

Đây là lệnh dùng để add:

    $ git add .

Đây là lệnh dùng để commit:

    $ git commit -m "update something"

Đây là lệnh dùng để push:

    $ git push origin HEAD

Ba lệnh này mình sẽ gõ lần lượt mỗi khi có chỉnh sửa gì muốn đẩy lên remote repo.

# IV. Rebase

Trong quá trình chỉnh sửa trên branch `edit-some-stuff`, có ai đó chỉnh sửa branch `main` và mình cần đồng bộ chỉnh sửa của họ trên baseline branch (tạm dịch là nhánh gốc) thì sẽ dùng tới lệnh sau - rebase:

    $ git rebase main

Tuy nhiên, như mình nói ở trên, lệnh này nếu chỉ gõ thế thôi thì sẽ có nhiều lỗi xảy ra: `main` trên local chưa được đồng bộ với `main` trên remote, v.v.. Nên mình viết một script tên `git-rebase.bat` như sau:

    set srcBranch=%1
    set dstBranch=%2

    echo. Rebasing '%srcBranch%' onto '%dstBranch%' ...


    :: Update working directory to clean version of srcBranch
    echo. git-clean-checkout %srcBranch%
    Call git-clean-checkout %srcBranch%

    :: Update working directory to clean version of dstBranch
    echo. git-clean-checkout %dstBranch%
    Call git-clean-checkout %dstBranch%

    echo. git checkout %dstBranch%; git pull origin %dstBranch%
    git checkout %dstBranch%
    git pull origin %dstBranch%

    echo. git checkout %srcBranch%; git pull origin %srcBranch%
    git checkout %srcBranch%
    git pull origin %srcBranch%

    echo. Being at branch: %srcBranch%
    echo. git rebase %dstBranch%
    git rebase %dstBranch%

    echo. Please use command: 'git status' to check merge status
    echo. Please use command: 'git rebase --continue' to finish merge process
    echo(

Về sau, mỗi khi cần rebase branch `edit-some-stuff` về `main` thì mình gõ lệnh sau:

    $ git-rebase edit-some-stuff main

Lệnh này sẽ đồng bộ các chỉnh sửa ở `main` về `edit-some-stuff`. Mình dùng tiếp lệnh sau để push force lên branch `edit-some-stuff`:

    $ git push origin HEAD -f

Bài này mình nghĩ cũng dài rồi, nên mình tạm dừng ở đây. Mình sẽ viết tiếp ở bài sau nhé.