---
title : "YOLO-Mark"
categories:
  - blogging
---

YOLO-Mark를 사용해 봅시다.



- git clone https://github.com/AlexeyAB/Yolo_mark.git

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/clone.png">
  <figcaption></figcaption>
</figure>


- cmake .

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/cmake.png">
  <figcaption></figcaption>
</figure>

- make

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/make_err.png">
  <figcaption>오류가 안 나면 서운할 뻔 했다..</figcaption>
</figure>



- main.cpp의 내용을 수정해 주어야 한다.

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/main_0.png">
  <figcaption>CV_CAP_PROP_FPS -> CAP_PROP_FPS</figcaption>
</figure>

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/main_1.png">
  <figcaption>CV_FILLED -> FILLED</figcaption>
</figure>

- make

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/cmake.png">
  <figcaption>기분이 좋다.</figcaption>
</figure>

- ./linux_mark.sh

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/permission.png">
  <figcaption>기분이 안 좋다.</figcaption>
</figure>

- sudo chmod 755 ./linux_mark.sh

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/chmod.png">
  <figcaption></figcaption>
</figure>

- ./linux_mark.sh

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/exec.png">
  <figcaption>GOOD!</figcaption>
</figure>

- 아래와 같은 화면이 실행됩니다.

<figure>
  <img src="/assets/images/2018-10-13-YOLO-Mark/run.png">
  <figcaption></figcaption>
</figure>



기타 사용법은 아래의 블로그를 참고하시기 바랍니다.

<http://pgmrlsh.tistory.com/6?category=766787>