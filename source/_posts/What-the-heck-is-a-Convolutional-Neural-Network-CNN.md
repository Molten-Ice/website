---
title: What is a Convolutional Neural Network(CNN)?
date: 2020-05-20 01:00:44
excerpt: Convolutional neural networks have already been unknowingly influencing your life, their application range from searching for an image on google to detecting deforestation from satellite images
thumbnail: /images/thumbnails/what-is-a-CNN.jpeg
tags: [CNN,AI, Beginner]
categories: [AI, Computer Vision]
---


Convolutional neural networks have already been unknowingly influencing your life, their applications range from facial recognition to detecting deforestation from satellite images.

In this article, I am going to show you how powerful of a tool Convolutional Neural Networks are, and how they learn.

In abstract terms, A CNN is a function which has an image as its input and outputs what it thinks the image is.

![](/images/What-is-a-CNN/f(sherlock)-cropped.jpg)


Inside this function, there are values called parameters which we get to choose the value of; our aim is to try and choose these values, these parameters such that our function correctly predicts the breed as much as possible. For this example say we have 1000 images of 5 different breeds of dog, evenly split. Our model will then be used to predict what breed of dog each image is.

What our model is outputting is 10 values, these values are the probability of what breed it thinks the input image is,


![](/images/What-is-a-CNN/Softmax-cropped.jpg)

Here our model has predicted with a certainty  of 80% that the image is a Cockapoo, and with a 10% certainty that it's a Cavachon. Our model will then pick the highest value as it's prediction.

On the other hand, we can have an example where our model is very uncertain,

![](/images/What-is-a-CNN/Softmax-bad-cropped.jpg)

Here our model has predicted with a certainty of 50% that the image is a Cockapoo, but has also predicted with a 45% certainty that it's a Cavachon. 

There is alot more ambiguity in the latter example, however, as we currently see it both examples would give identical answers. 

Our aim here onwards will be to try and get the correct prediction's value as close to 1 as possible and all the other values as close to 0 as possible.

## How they learn

Firstly, I'll need to introduce the term "cost function" from the deep learning literature. 
A cost function is a way of measuring how well we are doing at getting the probabilities in the diagrams above correct. It measures how close the correct probability is to 1 and how close the other probabilities are to 0. The value produced by the cost function is called the loss.

The smaller the value of the cost function the better, and conversely the larger the value of the cost function the worse we are doing.

So in order to make our function very good at predicting dog breeds, we need to minimize the value of our cost function.

![](/images/What-is-a-CNN/gradient-descent.gif)
*Gradient descent visualised: 120 mins*

Here we have a diagram, the vertical axis is the value given by our cost function and the others are the values we get to pick, called parameters. Our aim is to minimise the value given by our cost function, on this gram that corresponds to finding the values of w0 and w1 which give the global minimum for the loss.

Given that we start at a random point on the graph how can we find our way down to the global minimum?

Imagine you are standing on the slope of a valley, and you are trying to find your way to the bottom of the valley, but are blindfolded. What you can do is take a step in the downwards direction, We find the slope, also called gradient and take a step in the downwards direction

So that's all we're doing here, we take a point on the graph and then take a step in the downward direction, then keep repeating this process until we're at the minimum point.

This also has a fancy name called gradient descent.

So here we have 2 parameters w0 & w1, and we've found the value for these which mean our function will correctly predict the breed of dog as much as possible.

![](/images/What-is-a-CNN/f(sherlock)-cropped.jpg)

When our program is learning,  all it's doing exactly what we've described, the only difference is we have 2 parameters here, and in a real Convolutional Neural network you could easily have over 1 million, but it's doing the exact same thing.
---

## How A CNNs different to a normal NN

The difference is that have something called kernels, which can also be called feature detectors because that's exactly what they do.

Our feature detector is scanned across the entire image
*insert animation *

Before we were talking about how the model learns and all it's doing is choosing the best parameters, the best values inside the model.

But where do these values actually fit in with the model? They're actually the values in this feature detector!!

explain elementwise multiplication with diagram
*give vertical edge example*


Instead of having just one feature detector in the first layer, we might have 500 feature detectors. 
This means there are 500 different filters looking for different features in the image.

The beauty of it is that we don't need to specify the features the program needs to look for.

## What the kernels can tell us about the learning:


![](/images/What-is-a-CNN/layer7.jpeg)
![](/images/What-is-a-CNN/layer14.jpeg)
![](/images/What-is-a-CNN/layer20.jpeg)
![](/images/What-is-a-CNN/layer30.jpeg)
![](/images/What-is-a-CNN/layer40.jpeg)


*layer 20; layer 30*
*Layer 40*

Image classification is the task of taking an input image, in the form of its raw pixels and outputting a class which it thinks the image belongs to.

The core building block of a convolutional neural network is a convolutional layer. Several of these are stacked one after each other. The main aim of convolutions is to extract features such as edges and corners from the input, as you go deeper into then network the complexity of these features increases




---