---
title: "darknetpy"
categories:
  - blogging
toc: true
---

원본 이미지에서 학습된 이미지를 바운딩 박스해준다.

<link>https://pypi.org/project/darknetpy/</link>



pip install darknetpy



**설치 옵션**

- CUDA 

   GPU=1 pip install darknetpy

- cuDNN 

   CUDNN=1 pip install darknetpy

- OpenCV 

   OPENCV=1 pip install darknetpy

- multi-core CPU 

   OPENMP=1 pip install darknetpy

**python file**

```
from darknetpy.detector import Detector

detector = Detector('<absolute-path-to>/darknet',
                    '<absolute-path-to>/darknet/cfg/coco.data',
                    '<absolute-path-to>/darknet/cfg/yolov3.cfg',
                    '<absolute-path-to>/darknet/yolov3.weights')

results = detector.detect('<absolute-path-to>/darknet/data/dog.jpg')

print(results)
```

darknet과 weights 파일을 다운로드 하여야 한다.

```
git clone https://github.com/pjreddie/darknet
```

```
wget https://pjreddie.com/media/files/yolov3.weights
```



**결과**

```
[{'right': 194, 'bottom': 353, 'top': 264, 'class': 'dog', 'prob': 0.8198755383491516, 'left': 71}]
```
