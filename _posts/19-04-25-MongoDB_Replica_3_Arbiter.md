---
title : "[MongoDB Replica]-3 Arbiter"
categories:
  - MongoDB
---
arbiter 설정

<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/logo.jpg">
  <figcaption></figcaption>
</figure>

<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/cluster.PNG">
  <figcaption></figcaption>
</figure>

primary 선출을 위한 '선관위' 같은 느낌이다.


3. arbiter

- server #1
  
  각각의 Replica Set에 대하여 Arbiter를 설정해 준다.
  ~~~
  mongod \
  --fork \
  --replSet RS_1 \
  --bind_ip 192.168.0.1 --port 30001 \
  --dbpath /data/mongodb/arbiter/shard01 \
  --logpath /data/mongodb/logs/arbiter_1.log
  ~~~

  ~~~
  mongod \
  --fork \
  --replSet RS_2 \
  --bind_ip 192.168.0.1 --port 30002 \
  --dbpath /data/mongodb/arbiter/shard02 \
  --logpath /data/mongodb/logs/arbiter_2.log
  ~~~

  ~~~
  mongod \
  --fork \
  --replSet RS_3 \
  --bind_ip 192.168.0.1 --port 30003 \
  --dbpath /data/mongodb/arbiter/shard03 \
  --logpath /data/mongodb/logs/arbiter_3.log
  ~~~

- server #2

  ~~~
  mongo --host 192.168.2 --port 20001
  ~~~

  ~~~
  > rs.addArb("192.168.0.1:30001")
  ~~~

- server #3

  ~~~
  mongo --host 192.168.3 --port 20002
  ~~~

  ~~~
  > rs.addArb("192.168.0.1:30002")
  ~~~

- server #4

  ~~~
  mongo --host 192.168.4 --port 20003
  ~~~

  ~~~
  > rs.addArb("192.168.0.1:30003")
  ~~~