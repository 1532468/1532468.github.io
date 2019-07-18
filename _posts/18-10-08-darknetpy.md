---
title: "darknetpy"
categories:
  - blogging
---

원본 이미지에서 학습된 이미지를 바운딩 박스해준다.

<https://pypi.org/project/darknetpy/>



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

- darknet과 weights 파일을 다운로드 하여야 한다.

   ```
   git clone https://github.com/pjreddie/darknet
   ```

   ```
   wget https://pjreddie.com/media/files/yolov3.weights
   ```



   위의 설치 옵션을 Makefile에서 수정할 수도 있습니다. 수정하시면 반드시 make를 해야합니다.

   <figure>
     <img src="/assets/images/2018-10-08-darknetpy/makefile.png">
     <figcaption></figcaption>
   </figure>

**python file**

```python
from darknetpy.detector import Detector

detector = Detector('<absolute-path-to>/darknet',
                    '<absolute-path-to>/darknet/cfg/coco.data',
                    '<absolute-path-to>/darknet/cfg/yolov3.cfg',
                    '<absolute-path-to>/darknet/yolov3.weights')

results = detector.detect('<absolute-path-to>/darknet/data/dog.jpg')

print(results)
```



**결과**

```
[{'right': 194, 'bottom': 353, 'top': 264, 'class': 'dog', 'prob': 0.8198755383491516, 'left': 71}]
```



- nvidia-smi

  GPU를 사용하고 있는지 확인할 수 있습니다. 

  <figure>
    <img src="/assets/images/2018-10-08-darknetpy/nvidia-smi.png">
    <figcaption></figcaption>
  </figure>

