---
title : "PortForwarding"
categories:
  - Linux
---
Linux에서 포트포워딩을 해 봅시다.


- 우선작업

    ~~~
    > vi /ect/sysctl.conf

    net.ipv4.ip_forward=1

    > sysctl -p /etc/sysctl.conf
    ~~~

- 그냥 다 초기화

    ~~~
    > iptables -F
    > iptables -t nat -F
    > iptables -X
    ~~~

- 패킷의 출발지를 나가는 장치의 IP로 바꾼다.(선택사항)

    ~~~
    > iptables -t nat -A POSTROUTING -j MASQUERADE
    ~~~

- 등록

    ~~~
    > iptables -t nat -A PREROUTING -p tcp \
    --dport 1022 -j DNAT --to-destination 111.222.333.444:22
    ~~~

- 목록 보기

    ~~~
    > sudo iptables -t nat -L --line-numbers
    (== sudo iptables --table nat --list --line-numbers)
    ~~~

- 삭제

    ~~~
    - 번호를 통한 삭제
    > iptables -D PREROUTING 13 -t nat
    ~~~

- 저장 (안 하면 재부팅시 낭패)

    ~~~
    sudo netfilter-persistent save
    sudo netfilter-persistent reload
    ~~~
