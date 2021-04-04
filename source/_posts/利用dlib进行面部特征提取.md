---
title: 利用dlib进行面部特征提取
date: 2018-07-11 19:08:30
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


### 实现步骤
- 加载官方提供模型，面部检测器
```python
detector = dlib.get_frontal_face_detector()
```

- 加载[面部标记模型](http://dlib.net/files/shape_predictor_5_face_landmarks.dat.bz2)，将面对齐到标准姿势。与脸、眼睛、眉毛、嘴角等对齐
```python
sp = dlib.shape_predictor('shape_predictor_5_face_landmarks.dat')
```
- 最后加载用于将面部特征转换为128D矢量模型[(下载)](http://dlib.net/files/dlib_face_recognition_resnet_model_v1.dat.bz2)
```python
facerec = dlib.face_recognition_model_v1('dlib_face_recognition_resnet_model_v1.dat.bz2')
```
### 具体实现
```python

class extract_128d_feature(object):
    """
        返回单张图像的128D特征
    """
    def __init__(self, dlib_shape_5_face_dat_path, dlib_face_rec_model_dat_path):
        
        # detector to find the faces
        self.detector = dlib.get_frontal_face_detector()

        # shape predictor to find the face landmarks
        if not os.path.exists(dlib_shape_5_face_dat_path):
            print(dlib_shape_5_face_dat_path + "文件不存在！")
            exit()
            return None
        self.predictor = dlib.shape_predictor(dlib_shape_5_face_dat_path)

        if not os.path.exists(dlib_face_rec_model_dat_path):
            print(dlib_face_rec_model_dat_path + "文件不存在！")
            exit()
            return None
        # face recognition model, the object maps human faces into 128D vectors
        self.facerec = dlib.face_recognition_model_v1(dlib_face_rec_model_dat_path)

    def detector(self):
        return self.detector


    def __call__(self, img):
        img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
        dets = self.detector(img_gray, 1)

        if(len(dets)!=0):
            shape = self.predictor(img_gray, dets[0])
            face_descriptor = self.facerec.compute_face_descriptor(img_gray, shape)
        else:
            face_descriptor = 0
            return False, None

        return True, face_descriptor

```

最终可以得到128维向量数据，其中同一人的图像彼此接近，并且来自不同人的图像相距很远。 因此，您可以通过将面映射到128D空间然后检查它们的欧几里德距离是否很小来执行面部识别。

通常，如果两个面部描述符向量之间的欧几里德距离小于0.6，则它们来自同一个人，否则它们来自不同的人。

---
<small><font color= "gray">本文[Quenwaz](http://quenwaz.github.io)版权所有，转载请说明出处</font></small>