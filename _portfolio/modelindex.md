---
layout: post
title: Classifying Columbia University landmarks using a neural network
img: "assets/img/portfolio/tf_classifier.jpg"
date: June 2020
tags: [Tensorflow, Keras, Classification, Convolutional Networks]
jsarr:
- model_js/index.js
---

# TensorFlow.js: Using a Keras model in the browser to classify 3 popular Columbia Landmarks

Description: This simple website deploys a classifier built with Keras and Tensorflow. Using transfer learning, the classifier is based partly on a well known CNN model, [VGG16](https://neurohive.io/en/popular-networks/vgg16/), to learn how to classify accurately any of these 3 popular Columbia University landmarks.
The model predicts the probabilites of any image to be classified as one of the 3 landmarks. Have fun!


This project is based on [this website](https://github.com/tensorflow/tfjs-examples/tree/master/mobilenet).

<div class="tfjs-example-container">
  <section>
    <p class='section-head'>Status</p>
    <div id="status"></div>
  </section>
  <section>
    <p class='section-head'>Model Output</p>
    <div id="file-container" style="display: none">
      Upload an image of any of the 3 landmarks:  <input type="file" id="files" name="files[]" multiple />
    </div>
    <div id="predictions"></div>
    <img id="cat" src="/assets/img/portfolio/tf_classifier.jpg" width=256 height=256 />
  </section>
</div>