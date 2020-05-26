---
title: What is a Convolutional Neural Network(CNN)?
date: 2020-05-20 01:00:44
excerpt: Convolutional neural networks have already been unknowingly influencing your life, their application range from searching for an image on google to detecting deforestation from satellite images
thumbnail: /images/thumbnails/what-is-a-CNN.jpeg
tags: [CNN,AI, Beginner]
categories: [AI, Computer Vision]
---


Convolutional neural networks have already been unknowingly influencing your life, their applications range from facial recognition to detecting deforestation from satellite images.

In this article, I'm going to be showing you how powerful of a tool Convolutional Neural Networks(CNNs) are, and how they learn.

In abstract terms, A CNN is a function which has an image as its input and outputs what it thinks the image is.

![](/images/What-is-a-CNN/f(sherlock)-cropped.jpg)


Inside this function, there are values called parameters which we get to choose the value of; our aim is to try and choose these values, these parameters such that our function correctly predicts the breed as much as possible. For this example say we have 1000 images of 5 different breeds of dog, evenly split. Our model will then be used to predict what breed of dog each image is.

What our model is actually outputting is 5 values, these values are each associated to a fixed breed; each value is the probability that the image is that breed from the CNNs predict. This is illustrated in the image below.

![](/images/What-is-a-CNN/Softmax-cropped.jpg)

Here our model has predicted with a certainty  of 80% that the image is a Cockapoo, and with a 10% certainty that it's a Cavachon. Our model will then pick the highest probability as it's prediction.

On the other hand, we can have an example where our model is very uncertain,

![](/images/What-is-a-CNN/Softmax-bad-cropped.jpg)

Here our model has predicted with a certainty of 50% that the image is a Cockapoo, but has also predicted with a 45% certainty that it's a Cavachon. 

We can see that there is alot more ambiguity in the latter example, however, as we currently see it both examples would give identical answers. 

Our aim here onwards will be to try and get the correct prediction's value as close to 1 as possible and all the other values as close to 0 as possible.

## How they learn

Firstly, I'll need to introduce the term "cost function" from the deep learning literature. 

A cost function is a way of measuring how well we are doing at getting the probabilities in the diagrams above correct. It measures how close the correct probability is to 1 and how close the other probabilities are to 0. The value produced by the cost function is called the loss.

The smaller the value of the cost function the better, and conversely the larger the value of the cost function the worse we are doing.

So in order to make our function very good at predicting dog breeds, we need to minimize the value of our cost function.

![](/images/What-is-a-CNN/gradient-descent.gif)
*Gradient descent visualised: 120 mins*

Here we have a diagram, the vertical axis is the value given by our cost function and the others are the values we get to pick, called parameters. Our aim is to minimise the value given by our cost function, on this graph that corresponds to finding the values of w0 and w1 which give the global minimum for the loss.

Given that we start at a random point on the graph how can we find our way down to the global minimum?

Imagine you are standing on the slope of a valley, and you are trying to find your way to the bottom of the valley, but are blindfolded. What you can do is take a step in the downwards direction, We find the slope, also called gradient and take a step in the downwards direction

So that's all we're doing here, we take a point on the graph and then take a step in the downward direction, then keep repeating this process until we're at the minimum point.

This is called gradient descent.

So here we have 2 parameters w0 & w1, and we've found the value for these which mean our function will correctly predict the breed of dog as much as possible.

![](/images/What-is-a-CNN/f(sherlock)-cropped.jpg)

When our program is learning,  all it's doing exactly what we've described, the only difference is we have 2 parameters here, and in a real Convolutional Neural network you could easily have over 1 million, but it's doing the exact same thing.

---

## How A CNNs is different to a normal NN

The difference is that have something called kernels, which can also be called feature detectors because that's exactly what they do.

A kernel is a sqaure matrix which is scanned across the entire image, extracting features at every position it is in. In the image below the yellow 3x3 square is the kernel which is moved across the 5x5 pixel image.

![](/images/What-is-a-CNN/kernel-gif.gif)
[*Source*](https://stats.stackexchange.com/questions/295397/what-is-the-difference-between-conv1d-and-conv2d
)

At each position the kernel is performing elementwise multiplication, this is where the each value in the kernel is multiplied by the pixel value it is ontop in the image, and then these values are summed up. This is shown in the image below, The website this image has come from has an interactive kernel, which I would recommened you have a look at.

![](/images/What-is-a-CNN/setosa.png)
[*setosa.io*](https://setosa.io/ev/image-kernels/)

For the remainder of these article I'm going to be going into more depth about kernels and what they can show us.

In the image below we can see an example of a simple kernel which can be used to detect the edges of an image.

![](/images/What-is-a-CNN/kernel-wiki.png)

So how do we choose the values within these kernels??

We can look at the problem in this way, we have parameters(values inside the kernels) which we need to choose the value of in a way which maximises the number of dogs we correctly predict the breed of(minimise the value of our loss function).

This is the exact same as the process I described in the "How they learn section", If it isn't making sense, have a quick re-read of that now you know what kernels are.

**In real CNNs:**

Instead of having just one feature detector in the first layer, we might have 500 feature detectors. 

This means there are 500 different filters looking for different features in the image.

The beauty of it is that using gradient descent we don't need to specify the features the program needs to look for, it automatically learns the best ones for our problem( almost :p).

## What the kernels can tell us about the learning:




[*Source*](https://en.wikipedia.org/wiki/Kernel_(image_processing))

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