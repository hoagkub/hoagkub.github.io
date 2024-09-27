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

LTSMs have some limitations:

- Limited attention span
- Computation efficiency
- Handling multiple requests

Transformers overcome all these limitation of LSTMs by using self-attention and parallel processing

### III.2.1. Transformer Architecture

At a high level, Transformer architecture consists of an encoder and a decoder

- Encoder take in a sequence input tokens and produces a sequence of hidden presentations
- Decoder take in the encoder's output and generates a sequence of output tokens

> The self-attention mechanism works by computing attention weights between each input token and all other input tokens and using these weights to compute a weighted sum of the input token embeddings. The attention weights are computed using a softmax function applied to the dot product of a query vector, a key vector, and a scaling factor. The query vector is derived from the previous layer's hidden representation, while the key and value vectors are derived from the input embeddings. The resulting weighted sum is fed into a multi-layer perceptron (MLP) to produce the next layer's hidden representation.

![Image not found](/assets/img/rnns-and-transformers/transformer_architecture.jpeg)

> EncoderLayer(x) = LayerNorm(x + SelfAttention(x) + FeedForward(x))

### III.2.2. Key, Value and Query

![Image not found](/assets/img/rnns-and-transformers/transformer_self_attention.png)

- Key: You can think of the key vectors as a set of reference points the model uses to decide which parts of the input sequence are important.
- Value: The value vectors are the actual information that the model associates with each key vector.
- Query: Query vectors are used to determine how much attention to give to each key-value pair.