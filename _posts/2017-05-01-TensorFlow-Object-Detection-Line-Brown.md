---
layout: post
title: TensorFlow Object Detection. Where is Brown's head?
image: "/images/posts/feature_images/line-object-detection.png"
author: ben
---

My fun mini weekend project on Object Recognition. I’m very impressed on the evolution of Computer Vision. The computer started recognizing the objects by using ~200 images for the training data. The project was performed using an NVIDIA Tesla K80 and training for ~12 hours on Google Cloud Compute Engine.

![Brown Object Detection](/images/posts/content_images/line4.png)

### How do I train the algorithm?
##### **Step 1:** manually label region (object/element) in each image (heavy lifiting work!).
Labels will store (x,y) coordinates of boundary box in the picture with class/type of the box. 
##### **Step 2:** Feed labeled image to object detection algorithm and select pre-trained deep learning model.
I've chose "Faster R-CNN" model becasue it recieved a good score of mAP (mean Average Precision). However, it takes longer time to process images than other models.
##### **Step 3:** Monitor training process closely through an algorithm dashboard.
##### **Step 4:** Validate model by applying models on images that are in test set.

![Brown Object Detection](/images/posts/content_images/line3.png)

**Check out GitHub** [https://github.com/Lanbig/custom-object-detection](https://github.com/Lanbig/custom-object-detection)