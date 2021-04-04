---
title: 利用dlib进行人脸检测
date: 2018-07-05 12:21:04
categories: 编码
tags: 
    - 人脸检测
    - OpenCV
    - Dlib
---

### 环境依赖
- python 3.6.1
- opencv
- dlib
- numpy

### DLIB 介绍
Dlib是一个包含机器学习算法的C++开源工具包。Dlib可以帮助您创建很多复杂的机器学习方面的软件来帮助解决实际问题。目前Dlib已经被广泛的用在行业和学术领域,包括机器人,嵌入式设备,移动电话和大型高性能计算环境。

Dlib是开源的、免费的;官网和git地址:
- [官网](http://dlib.net/)
- [github](https://github.com/davisking/dlib) 

Dlib的主要特点:
1. 文档齐全

    不像很多其他的开源库一样,Dlib为每一个类和函数提供了完整的文档说明。同时,还提供了debug模式;打开debug模式后,用户可以调试代码,查看变量和对象的值,快速定位错误点。另外,Dlib还提供了大量的实例。

2. 高质量的可移植代码

    Dlib不依赖第三方库,无须安装和配置,这部分可参照(官网左侧树形目录的how to compile的介绍)。Dlib可用在window、Mac OS、Linux系统上。

3. 提供大量的机器学习 / 图像处理算法

    - 深度学习

    - 基于SVM的分类和递归算法

    - 针对大规模分类和递归的降维方法

    - 相关向量机(relevance vector machine);是与支持向量机相同的函数形式稀疏概率模型,对未知函数进行预测或分类。其训练是在贝叶斯框架下进行的,与SVM相比,不需要估计正则化参数,其核函数也不需要满足Mercer条件,需要更少的相关向量,训练时间长,测试时间短。

    - 聚类: linear or kernel k-means, Chinese Whispers, and Newman clustering. Radial Basis Function Networks

    - 多层感知机

### 人脸检测（CNN）
示例显示如何使用dlib运行基于CNN的面部检测器。加载预训练模型并使用它来查找图像中的面。该CNN模型比基于HOG的模型准确得多，但需要更多的计算能力运行，并且意味着在GPU上执行以获得合理的速度。
```python
#!/usr/bin/python
# The contents of this file are in the public domain. See LICENSE_FOR_EXAMPLE_PROGRAMS.txt
#
#   This example shows how to run a CNN based face detector using dlib.  The
#   example loads a pretrained model and uses it to find faces in images.  The
#   CNN model is much more accurate than the HOG based model shown in the
#   face_detector.py example, but takes much more computational power to
#   run, and is meant to be executed on a GPU to attain reasonable speed.
#
#   You can download the pre-trained model from:
#       http://dlib.net/files/mmod_human_face_detector.dat.bz2
#
#   The examples/faces folder contains some jpg images of people.  You can run
#   this program on them and see the detections by executing the
#   following command:
#       ./cnn_face_detector.py mmod_human_face_detector.dat ../examples/faces/*.jpg
#
#
# COMPILING/INSTALLING THE DLIB PYTHON INTERFACE
#   You can install dlib using the command:
#       pip install dlib
#
#   Alternatively, if you want to compile dlib yourself then go into the dlib
#   root folder and run:
#       python setup.py install
#
#   Compiling dlib should work on any operating system so long as you have
#   CMake installed.  On Ubuntu, this can be done easily by running the
#   command:
#       sudo apt-get install cmake
#
#   Also note that this example requires Numpy which can be installed
#   via the command:
#       pip install numpy

import sys
import dlib
import os

# if len(sys.argv) < 3:
#     print(
#         "Call this program like this:\n"
#         "   ./cnn_face_detector.py mmod_human_face_detector.dat ../examples/faces/*.jpg\n"
#         "You can get the mmod_human_face_detector.dat file from:\n"
#         "    http://dlib.net/files/mmod_human_face_detector.dat.bz2")
#     exit()
# 官方例程都是从命令行加载，其提供的训练模型
# 需要先行下载
cnn_face_detector = dlib.cnn_face_detection_model_v1("mmod_human_face_detector.dat")
win = dlib.image_window()

# 此处改成自己的图片目录，图片中需包含人脸
# 该例程会进行人脸框选
cur_path = os.getcwd()
image_path = cur_path + os.sep + 'faces'
print(image_path)
dir_pics = os.listdir(image_path)
for f in dir_pics:
    print("Processing file: {}".format(f))
    path = image_path + os.sep + f
    print(path)
    img = dlib.load_rgb_image(path)
    # The 1 in the second argument indicates that we should upsample the image
    # 1 time.  This will make everything bigger and allow us to detect more
    # faces.
    dets = cnn_face_detector(img, 1)
    '''
    This detector returns a mmod_rectangles object. This object contains a list of mmod_rectangle objects.
    These objects can be accessed by simply iterating over the mmod_rectangles object
    The mmod_rectangle object has two member variables, a dlib.rectangle object, and a confidence score.
    
    It is also possible to pass a list of images to the detector.
        - like this: dets = cnn_face_detector([image list], upsample_num, batch_size = 128)

    In this case it will return a mmod_rectangless object.
    This object behaves just like a list of lists and can be iterated over.
    '''
    print("Number of faces detected: {}".format(len(dets)))
    for i, d in enumerate(dets):
        print("Detection {}: Left: {} Top: {} Right: {} Bottom: {} Confidence: {}".format(
            i, d.rect.left(), d.rect.top(), d.rect.right(), d.rect.bottom(), d.confidence))

    rects = dlib.rectangles()
    rects.extend([d.rect for d in dets])

    win.clear_overlay()
    win.set_image(img)
    win.add_overlay(rects)
    dlib.hit_enter_to_continue()
```
{% asset_img cnn_face_detector.jpg cnn_face_detector %}


### 人脸检测（HOG）
利用OpenCV读取摄像头数据帧，官方例程利用load_rgb_image加载图片文件，其数据帧就是numpy数组。然后直接dlib提供的detector方法进行采取面部68个地标进行人脸检测。
```python
import cv2
import dlib
 
cap=cv2.VideoCapture(0)
# 可在官网下载已经训练好的68特征人脸数据 
# http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2 下载数据包
predictor_path = "shape_predictor_68_face_landmarks.dat"
predictor = dlib.shape_predictor(predictor_path)
detector = dlib.get_frontal_face_detector()
 
while True:
    _,frame=cap.read()
    # Ask the detector to find the bounding boxes of each face. The 1 in the  
    # second argument indicates that we should upsample the image 1 time. This  
    # will make everything bigger and allow us to detect more faces. 
    dets = detector(frame, 1)
    if len(dets) != 0:
        # Get the landmarks/parts for the face in box d.
        shape = predictor(frame, dets[0])
        print(shape.parts())
        for p in shape.parts():
            cv2.circle(frame, (p.x, p.y), 3, (0,0,0), -1)

    cv2.imshow('video',frame)
    if cv2.waitKey(1)&0xFF==27:
        break
cap.release()
cv2.destroyAllWindows()
```

---
<small><font color= "gray">本文[Quenwaz](http://quenwaz.github.io)版权所有，转载请说明出处</font></small>