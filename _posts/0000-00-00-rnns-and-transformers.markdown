---
layout: post
title:  "RNNs and Transformers"
date: "2024-09-17 20:00:00 +0700"
last_modified_at: "2024-09-17 20:00:00 +0700"
categories: [Study, Deep_Learning]
---

![Image not found](/assets/img/rnns-and-transformers/ink_1.png)
![Image not found](/assets/img/rnns-and-transformers/ink_2.png)

# III. Introduction to Transformers

## III.1. Intro

> Multihead attention is a technique for allowing the model to focus on different part of the input sequence at different levels of abtraction, allowing it to capture more complex relationships between the words.

## III.2. Transformers in Detail

### III.2.1. Transformer Architecture

![Image not found](/assets/img/rnns-and-transformers/transformer_architecture.jpeg)

EncoderLayer(x) = LayerNorm(x + SelfAttention(x) + FeedForward(x))