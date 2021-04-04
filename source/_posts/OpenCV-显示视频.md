---
title: OpenCV 显示视频
date: 2018-07-02 10:40:57
categories: 编码
tags: OpenCV
---
### 环境依赖
- python 3.6.1   
- cv2
- numpy

### 从摄像头中读取
```python
# created by Quenwaz
# 15/06/2018 17:05:45 
import cv2
import numpy as np

cap = cv2.VideoCapture(0)
while(True):
    # 读取摄像头数据流中的一帧
    ret, frame = cap.read()
    # 显示
    cv2.imshow("capture", frame)
    # 按键q退出
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
cap.release()
cv2.destroyAllWindows() 
```
若需知道视频最大分辨率，及帧数
```python
# 读取摄像头最大分辨率
cap.get(cv2.CAP_PROP_FRAME_WIDTH)
cap.get(cv2.CAP_PROP_FRAME_HEIGHT)
# 获取视频帧数
fps =cap.get(cv2.CAP_PROP_FPS) 
```

### 从MP4文件中读取
```python
# created by Quenwaz
# 15/06/2018 17:05:45 
import cv2
import numpy as np

cap = cv2.VideoCapture("D://test.mp4")
while(True):
    # 读取摄像头数据流中的一帧
    ret, frame = cap.read()
    # 显示
    cv2.imshow("capture", frame)
    # 按键q退出
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
cap.release()
cv2.destroyAllWindows() 
```

---
<small><font color= "gray">本文[Quenwaz](http://quenwaz.github.io)版权所有，转载请说明出处</font></small>