---
title : "RaspberryPi mircophone"
categories:
  - blogging
---

라즈베리파이에서 마이크를 사용하기 위해 몇 가지 설정이 필요합니다.



**확인사항**

- lsusb

<figure>
  <img src="/assets/images/2018-10-12-mircophone/lsusb.png">
  <figcaption>usb 포트에 연결된 기기 목록 확인</figcaption>
</figure>

- arecord -l

<figure>
  <img src="/assets/images/2018-10-12-mircophone/arecord.png">
  <figcaption>오디오 녹음장치 확인</figcaption>
</figure>



**수정사항**

- sudo vi /usr/share/alsa/alsa.conf

  defaults.ctl.card 0

  defaults.pcm.card 0

  를

  defaults.ctl.card 1

  defaults.pcm.card 1

<figure>
  <img src="/assets/images/2018-10-12-mircophone/alsa.png">
  <figcaption>alsa.conf</figcaption>
</figure>

