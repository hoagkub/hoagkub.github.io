---
layout: post
title:  "Các lệnh Git tôi sử dụng gần như hàng ngày và best-practice flow cho từng use-case"
date: "2023-03-11 19:00:00 +0700"
categories: Uncategorized
---

Hồi mới đi làm, hàng ngày tôi phải gõ đi gõ lại rất nhiều lệnh Git. Việc này đã
khiến tôi cảm thấy mất thời gian và chán nản. Vì vậy, tôi đã nghĩ "Vậy sao không
nghĩ tới việc tổng hợp các lệnh Git hay dùng và viết chúng thành một script nhỏ
để sử dụng hàng ngày?"

- [Setup một local repository](#setup-một-local-repository)
- [Clean-checkout 1 branch](#clean-checkout-1-branch)

# Setup một local repository

1. cd đến thư mục cần kéo file từ git về.
   > <pre>
   $ cd Game_TheCat

2. Tạo một Git repository rỗng trên local.
   > <pre>
   $ git init
   Initialized empty Git repository in D:/Documents/SAU_DAI_HOC/Game_TheCat/.git/

3. Thêm 1 remote tên `origin` cho cho repository tại URL:
   https://github.com/hoagkub/Game_TheCat.
   > <pre>
   $ git remote add origin https://github.com/hoagkub/Game_TheCat


# Clean-checkout 1 branch

1. Xóa tất cả các file/sub-folder không được đánh dấu tracking trong local.
   > <pre>
   $ git clean -fd

2. Fetch toàn bộ branch tồn tại trong remote tên `origin` mà chúng ta đã thiết lập ở trên.

   > <pre>
   $ git fetch origin
   remote: Enumerating objects: 14, done.
   remote: Counting objects: 100% (14/14), done.
   remote: Compressing objects: 100% (12/12), done.
   remote: Total 14 (delta 1), reused 14 (delta 1), pack-reused 0
   Unpacking objects: 100% (14/14), 12.14 KiB | 28.00 KiB/s, done.
   From https://github.com/hoagkub/Game_TheCat
    &ast; [new branch]      main       -> origin/main

3. Fetch branch tên `main` trong remote `origin`.
   > <pre>
   $ git fetch origin main
   From https://github.com/hoagkub/Game_TheCat
    &ast; branch            main       -> FETCH_HEAD

   Sau lệnh này, bạn hãy để ý `FETCH_HEAD` trong output.
4. Update các file/folder ở local đúng với version trên Git remote URL.
   > <pre>
   $ git checkout main
   Switched to a new branch 'main'
   Branch 'main' set up to track remote branch 'main' from 'origin'.

5. Revert tất cả các thay đổi trên local một lần nữa cho chắc.
   > <pre>
   $ git reset --hard FETCH_HEAD
   HEAD is now at e9583a3 Add Readme.md
   $ git reset --hard origin/main
   HEAD is now at e9583a3 Add Readme.md
   $ git clean -fd


