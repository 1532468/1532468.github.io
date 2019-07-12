---
title : "PortForwarding_Windows"
categories:
  - Linux
---

cmd창을 관리자 권한으로 실행

- 등록

    ~~~
    > netsh interface portproxy add v4tov4 listenport=1022 listenaddress=0.0.0.0 connectport=22 connectaddress=111.222.333.444
    ~~~

- 목록 보기

    ~~~
    netsh interface portproxy show v4tov4
    ~~~

- 삭제

    ~~~
    netsh interface portproxy delete v4tov4 listenport=3331 listenaddress=0.0.0.0
    ~~~