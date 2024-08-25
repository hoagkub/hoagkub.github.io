---
layout: post
title:  "Optimizing Performance"
date: "2024-08-25 21:00:00 +0700"
last_modified_at: "2024-08-25 21:00:00 +0700"
categories: [Study, Deep_Learning]
---

CHAPER 3. CNN IN DEPTH 

IMAGE AUGMENTATION is  very common method to:
+ Avoid overfitting
+ Increase robustness of the network
+ Introduce rotational, translational, scale invariance as well as insensitiveness to color changes
+ Avoid shortcut learning

NOTES:
+ Train data should have randomness, whereas validation/test data shouldn't have.
+ The typically image augmentation in validation/test data is "RESIZE ---> CENTERCROP ---> TOTENSOR ---> NORMALIZE"
+ The resize and crop should be the same between train/test/validation data for better performance
+ The normalization also should be the same between train/test/validation

AUTOAUGMENT TRANSFORM can be used in Pytorch by call following function:
+ T.RandAugment(num_ops, magnitude). In which:
++ numops : how many transformation should be applied to each image
++ magnitude: the strength of these transformation

***

BATCH NORMALIZATION is used to standardize the Activations. Formula as seen as below:
+ x <--- [(x - µ_x) / σ_x ] * ϒ + β

TYPICAL USAGE OF BATCHNORM IN NETWORK ARCHITECTURE DESIGN
+ BatchNorm2D immediately after conv layer
+ BatchNorm1D immediately after Linear layer
+ Example:
--->Input
---> Conv1 ---> BatchNorm2D1 ---> MaxPool2D1 ---> ReLU1 ---> Dropout  
---> Conv2 ---> BatchNorm2D2 ---> MaxPool2D2 ---> ReLU2 ---> Dropout 
---> ...
---> Flatten 
---> Linear ---> BatchNorm1D ---> ReLU ---> Dropout 
---> ...
---> Output

***

IMPORTANT TERMS IN OPTIMIZING PERFORMANCE

+ Parameter
++ Internal to the model
++ May vary during training
++ Examples: Weights and biases of a network

+ Hyperparameter
++ External to the model
++ Fixed during training
++ Examples: Learning rate, number of layers, activation layers

+ Experiment
++ A specific training run with a fixed set of hyperparameters
++ Practitioners typically perform many experiments varying the hyperparameters. Each experiment produces one or more metrics that can be used to select the best-performing set of hyperparameters (see the next section).

STRATEGIES FOR OPTIMIZING HYPERPARAMETERS

+ Grid search
++ Divide the parameter space in a regular grid
++ Execute one experiment for each point in the grid
++ Simple, but wasteful

+ Random search
++ Divide the parameter space in a random grid
++ Execute one experiment for each point in the grid
++ Much more efficient sampling of the hyperparameter space with respect to grid search

+ Bayesian Optimization
++ Algorithm for searching the hyperparameter space using a Gaussian Process model
++ Efficiently samples the hyperparameter space using minimal experiments

MOST IMPORTANT HYPERPARAMETERS

"Optimizing hyperparameters can be confusing at the beginning, so we provide you with some rules of thumb about the actions that typically matter the most. They are described in order of importance below. These are not strict rules, but should help you get started:"

+ Design parameters: When you are designing an architecture from scratch, the number of hidden layers, as well as the layers parameters (number of filters, width and so on) are going to be important.
+ Learning rate: Once the architecture is fixed, this is typically the most important parameter to optimize. The next video will focus on this.
+ Batch size: This is typically the most influential hyperparameter after the learning rate. A good starting point, especially if you are using BatchNorm, is to use the maximum batch size that fits in the GPU you are using. Then you vary that value and see if that improves the performances.
+ Regularization: Once you optimized the learning rate and batch size, you can focus on the regularization, especially if you are seeing signs of overfitting or underfitting.
+ Optimizers: Finally, you can also fiddle with the other parameters of the optimizers. Depending on the optimizers, these vary. Refer to the documentation and the relevant papers linked there to discover what these parameters are.

OPTIMIZING LEARNING RATE

using learning rate scheduler in pytorch

TO IMPROVE THE PERFORMANCE OF A CNN

+ Image augmentation
+ Hyperparameters tunning
+ Learning rate scheduler

THE ROLE OF THE BATCHNORM LAYER IN MODERN CNNS

+ Standardize the activations of a layer to prevent covariate shift (i.e., shifts in the distribution of the activations)
+ Allow for faster training and larger learning rates
+ Makes the layers more independent from each other, making the work of the optimizer much easier

RANK THE HYPERPARAMETERS IN ORDER OF IMPORTANCE

+ 1. Architecture Design: Number of hidden layers, layer parameters (number of filters, width, ...), BatchNorm, ...
+ 2. Learning rate: Typically the most importance hyperparameters for a given architecture
+ 3. Batch size: When using BatchNorm, typically a good value is the largest value that fits in the GPU
+ 4. Regularization: Weight decay, Dropout, ...
+ 5. Optimizer and its parameters: SGD, Adam, RMSProp, ...

WEIGHT INITIALIZATION

Pytorch library offers the best choice of weight initialization

TRANSFER LEARNING

LESSON OBJECTIVES
+ Understand key CNN architecture
+ + AlexNet
+ + VGG
+ + Skip connection and ResNet
+ + Channel attention - EfficientNet
+ + Transformers in Computer Vision
+ Apply multiple ways of adapting pre-trained network using transfer learning
+ + Using models in TorchVision and TIMM
+ + Train only the head
+ + Fine-tune the entire network
+ + Other situations
+ Fine-tune a pre-trained network on a new dataset

INNOVATIVE CNN ARCHITECTURE

AlexNet ---> VGG ---> ResNet

RESNET IN DEPTH

ResNet introduce a fundemental innovation: "skip connection"

INPUT SIZE AND GAP - GLOBAL AVERAGE POOLING LAYER

+ The conv and pool layers can handle any input size
+ The Dense layers only can handle specific input size
+ The Global Average Pooling is a Average Pooling with window size equivalent to input size. It'll use to replace Flatten layers.

ATTENTION LAYERS

+ Make the network focus on the importance information
+ Channel Attention:
+ + Squeeze and Excitation - SE block
+ + Transformer - This will be explain details on the it own lesson
+ State of the art (SOTA) Computer vision models

TRANSFER LEARNING
https://learn.udacity.com/paid-courses/cd0281/lessons/ls0479/concepts/6f3c6870-3291-4b19-8a74-db2b79458691?_gl=1*5fphi2*_gcl_au*NzI4NjM2NzAuMTcyMDk0Mjk3MQ..*_ga*MTIxNTIzMTc0Ni4xNzIwOTQyOTg0*_ga_CF22GKVCFK*MTcyNDAzODMxMy4xNi4xLjE3MjQwMzgzMTUuNTguMC4w&lesson_tab=lesson
+ Finetune just the head - Small dataset, similiar
+ Finetune the head, the finetune the rest - Large dataset, mostly dissimiliar, somewhat similiar
+ Train from scratch - Large dataset, very dissimiliar
+ Collect more dataset - Small dataset, very dissimiliar

VISUALIZING CNNS 
+

AUTOENCODER - used to pinpoints which inputs that are difficult/easier to recognize. In other ways, it used to pinpoints the anomalies.

Input images ---> Encoder (similiar to Backbone - extract features, called Embedding)
---> Flatten ---> Decoder (similiar to Head - reconstruct the images from Embeddings)

(1) Encoder = MLP, Decoder = MLP --- which is called MLPs based Autoencoder
(2) Encoder = Conv+Pool, Decoder=Transpose --- which is called CNNs based Autoencoder



