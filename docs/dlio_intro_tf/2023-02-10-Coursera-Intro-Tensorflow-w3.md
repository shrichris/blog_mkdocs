---
layout: post
title:  "Coursera - Introduction to Tensorflow - Week 3"
date:   2023-02-09 
categories: notes
learning_platform: [coursera, deeplearning.ai]
specialization: DeepLearning.AI TensorFlow Developer Professional Certificate
course: Introduction to Tensorflow
tags: [Coursera, Intro_Tensorflow] 
---
# Notes from Week 3 of auditing the course

# Why CNNs?

Introduces Convolutional Neural Networks (CNNs) as an improvement over the naive implementation of week 2. 

Pooling is way of compressing images e.g. using a 2*2 grid to pick out the pixel with the highest value to quarter the size of the image.
Convolutions are essentially filters that can be passed over input data to emphasise features e.g. using fliters to highlight vertical or horizontal lines in an image.

```python
model = keras.Sequential([
    # Adding convolution and pooling layers
    # (28,28,1) - The 1 is to indicate the single byte depth for the grascale images in FMNIST.
    # The 64 filters in the convolution layer are not random. They are a set of known good filters. See Course 4 of deep learning specialization to understand the details of convolution and pooling layers
    keras.layers.Conv2D(64, (3,3), activation='relu', input_shape=(28,28,1))
    keras.layers.MaxPooling2d(2,2)
    keras.layers.Conv2D(64, (3,3), activation='relu')
    keras.layers.MaxPooling2d(2,2)
    # End of convolution and pooling layers. Input content size has been greatly reduced.
    keras.layers.Flatten(),
    keras.layers.Dense(128, activation=tf.nn.relu),
    keras.layers.Dense(10,activation=tf.nn.softmax)
])
```
## Output shape changes at each layer due to the effects of convolution and pooling

model.summary can be used to inspect the layers of the model, including the output shapes of each layer

* Inspecting the model above will show that the output shape of the first convolution layer is (26,26,64) 
  * This is due to the effect of the (3,3) convolution filter -  the filter cannot be applied on the edges of the image, along a 1 pixel margin. This reduces the margin by 2 pixels on each axis 
  * For a (5,5) filter, the output shape will be 4 pixels less on x and y axes.
* The first 2dMax pooling halves the output along each axis and the output shape is (13,13,64)
* The second convolution layer changes the output shape to (11,11,64), due to the effect of the (3,3) filter
* The second convolution layer now reduces the output layer to (5,5,64)
  * N.B For odd dimensions, max pooling will round down i.e. (11,11,64) -> (5,5,64)
* The end result of convolution and pooling layers is that the 28*28 images are reduced to 5*5 images
* Remember that there are 64 convolutions. Meaning that the input to the flatten layer is 1600 pixels (rather than 784)

## Number of paramters

The lecture did not walk through the number of parameters at each layer.  

Revisit this aspect.

## Jupyter Notebooks

* [Notebook - Week 3 - Lab 1 - Using convolutions with FMNIST for computer vision](https://github.com/https-deeplearning-ai/tensorflow-1-public/blob/main/C1/W3/ungraded_labs/C1_W3_Lab_1_improving_accuracy_using_convolutions.ipynb)
* [Notebook - Week 3 - Lab 2 -  Exploring convolutions](https://github.com/https-deeplearning-ai/tensorflow-1-public/blob/main/C1/W3/ungraded_labs/C1_W3_Lab_2_exploring_convolutions.ipynb)

## Links

* [Tensorflow documentation](https://www.tensorflow.org/api_docs/python/tf/)
* [Conv2D in TF](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv2D)
* [Pooling in TF](https://www.tensorflow.org/api_docs/python/tf/keras/layers/MaxPool2D)
* [Convolution and Pooling details in the Deep Learning Specialization](https://www.youtube.com/playlist?list=PLkDaE6sCZn6Gl29AoE31iwdVwSG-KnDzF)
* [Lode's Computer Graphics Tutorial](https://lodev.org/cgtutor/filtering.html)

## To Do

* Review the walkthrough lecture "Improving the Fashion classifier with convolutions"
