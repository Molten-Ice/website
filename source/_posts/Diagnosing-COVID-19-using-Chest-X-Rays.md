---
title: Diagnosing COVID-19 using Chest X-Rays
date: 2020-04-15 23:32:18
excerpt: Utilising learning rate discrimination & progressive resizing
thumbnail: /images/thumbnails/covid-x-rays.png
tags: CNN, fastai
categories: [AI, Computer Vision]
---

In this article, I will be discussing how a Resnet architecture can be used to classify a dataset containing chest X-rays of patients who have COVID-19, pneumonia or are healthy.

The spread of coronavirus throughout the world has happened at an unprecedented rate, shutting down entire countries and crippling economies. Production of testing equipment is often very limited and the global community would undoubtedly benefit from having alternative ways to diagnosing COVID-19.

Currently, there is a very limited amount of data about Coronavirus cases, the main source I will be using for this article is [Synthetaic](https://covidresearch.ai/datasets/dataset?id=2) which has collated data from [COVID-NET](https://arxiv.org/abs/2003.09871) and [Image Data Collection](https://arxiv.org/abs/2003.11597). Due to the rapid pace which new COVID-19 data is being collected and the pace at which the Coronavirus literature is moving, I will be regularly updating my model and this article as more information becomes available.

Using standard Coronavirus testing methods the estimated false negative rate is around 10%, the aim for this project is to help provide a system for distinguishing between pneumonia and COVID-19 with a high rate of success.

*Note: A link to my GitHub containing the code is at the bottom*


---

## Model

For this task, we will be using a Convolutional Neural Network(CNN) with the Resnet50 architecture, due to time constraints and a lack of data we will be using pre-trained weights gained from training this model on the ImageNet dataset. The data we will be using belongs to 3 classes: Uninfected, Infected or Pneumonia.

I will be using the fastai library which is a deep learning framework built using PyTorch.

Firstly, we create a DataBunch using default transformations and size of 224.

``` python
src = ImageList.from_folder(path)
.split_by_rand_pct(0.2).label_from_folder()
tfms = get_transforms() 
data = src.transform(tfms=get_transforms, size=224, resize_method=ResizeMethod.SQUISH).databunch(bs=bs, num_workers=4).normalize()
```

![](/images/COVID-19-X-rays/training-set-images.png)
*Images from the training set*

The Images we will be processing look like the images above, these images have already had transformations applied to them. Some of the transformations include:

* flipping the image horizontally
* rotating the image by up to 10 degrees
* Zooming into the image
* Random lighting and contrast changes
* Applying a random warp

These transforms are essential as they help the model to generalise to images not contained with the training set, as well as helping to prevent the model overfitting.

``` python
learn = cnn_learner(data, models.resnet50, metrics=error_rate)
```


Next, we create a cnn_learner, this is inbuilt into fastai to allow easy implementation of convolutional neural networks. We also specify our model architecture to be resnet50, by default fast.ai will use transfer learning. Transfer learning is where we train the weights using another dataset and then we reuse those weights are starting values for our model whilst training on our dataset.

Generally, the earlier layers in a CNN are used for detecting simple features such as edges, these features are very common are likely to be encountered in almost any image you come across. The image below is a visualization of an early layer within a CNN, this gives us a good intuition of why transfer learning is effective; most of the weights can be heavily reused throughout the network, allowing us to converge to an answer quickly with less data.

![](/images/COVID-19-X-rays/CNN-layer.png)
*Visualisation of a filter in an early layer of a CNN*


## Training: Stage 1

For this model, we will go through several rounds of training. Throughout the training process, we will be using several regularization techniques: weight decay, momentum and batch normalization.

Firstly, we freeze the weights of the model and will only allow a few layers at the end of the model to be adjusted. This gives a very good accuracy of over 91% on the training set in just 5 epochs!

![](/images/COVID-19-X-rays/training1.png)
*Training the CNN with a learning rate of 0.002*

Next, we will unfreeze the model and train all the layers for 5 epochs again. We will be using discriminative learning rates, this is where the learning rate will be lower in earlier layers, the reason for this is earlier layers have features which will not need to be changed very much (The pre-trained values from ImageNet are already very good values for this part of the network). Here the earlier layers will use a learning rate of 0.000001 whilst the later layers will use a learning rate of 0.0004. This gives us an even better accuracy of 93%.

![](/images/COVID-19-X-rays/training2.png)
*Training the CNN with discriminative learning rates*

## Training: Stage 2

Next, we will use another amazing tool called progressive resizing. Originally we were using images of size 224 to feed into our model, here we will feed in images of size 448. This allows our model to learn details about the training set and thus improve its performance. We also repeat the process of first training the model with the weights frozen and then unfreezing the weights, training in total for another 10 epochs.

![](/images/COVID-19-X-rays/training1.png)

We can see our final model had an accuracy of over 95% on the training set!!

---

## Evaluating on the test set

![](/images/COVID-19-X-rays/confusion-matrix.png)
*Confusion Matrix*

Our model has a weighted f1-score of 0.974 an accuracy rate of 97.4% on the test set. Furthermore correctly classified all the cases of Coronavirus; due to the lack of data, it is hard to draw any solid conclusions, however, our model managed to distinguished 10 COVID-19 x-rays from over 600 pneumonia x-rays.

On the 22nd of March, Linda Wang and Alexander Wong published a [paper](https://arxiv.org/abs/2003.09871) detailing COVID-Net, which was described as,

*"A Tailored Deep Convolutional Neural Network Design for Detection of COVID-19 Cases from Chest Radiography Images"*


![](/images/COVID-19-X-rays/covid-net.png)
*Predictions from COVID-Net*

The picture above shows the predictions from COVID-Net, we can see that COVID-Net correctly classified 8/10 COVID-19 x-rays whereas our model correctly classified 10/10 x-rays; it is worth that due to the incredibly small sample size this is not conclusive evidence that our model outperforms COVID-Net.

It is also worth noting our test set contained a much larger sample of patients who are healthy or have pneumonia whilst still containing the same amount of COVID-19 patients

AI has already undoubtedly proven itself as an invaluable tool to help improve the healthcare industry, however, unproven models such as this one should not be used for any medical diagnostics.

*Fun fact: All the training for this model cost me under $2 using Microsoft azure!*

Link to the source code in my [GitHub](https://github.com/Molten-Ice/Covid19-Chest-X-Ray)
