---
title : "MongoDB Replica Shard & Replica"
categories:
  - MongoDB
---
shard와 replica 설정

<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/logo.jpg">
  <figcaption></figcaption>
</figure>

<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/cluster.PNG">
  <figcaption></figcaption>
</figure>

2. replica set \
뭔가 많아 보이지만 간단합니다.\
replica set 이름과 port만 바꿔주면 됩니다.

- replica set #1

  server #2

  ~~~
  mongod \
  --fork \
  --replSet RS_1 \
  --shardsvr --bind_ip 192.168.0.2 --port 20001 \
  --dbpath /data/mongodb/replica/replica_1 \
  --logpath /data/mongodb/logs/replica_1.log
  ~~~

  server #3

  ~~~
  mongod \
  --fork \
  --replSet RS_1 \
  --shardsvr --bind_ip 192.168.0.3 --port 20001 \
  --dbpath /data/mongodb/replica/replica_1 \
  --logpath /data/mongodb/logs/replica_1.log
  ~~~

  server #4

  ~~~
  mongod \
  --fork \
  --replSet RS_1 \
  --shardsvr --bind_ip 192.168.0.4 --port 20001 \
  --dbpath /data/mongodb/replica/replica_1 \
  --logpath /data/mongodb/logs/replica_1.log
  ~~~

  server #2

  ~~~
  mongo --host 192.168.0.2 --port 20001
  ~~~

  ~~~
  var config={_id:'RS_1', 
  members:[
  {_id:0, host:'192.168.0.2:20001'}, 
  {_id:1, host:'192.168.0.3:20001'}, 
  {_id:2, host:'192.168.0.4:20001'}] 
  };

  rs.initiate(config)
  ~~~

- replica set #2


  server #2

  ~~~
  mongod \
  --fork \
  --replSet RS_2 \
  --shardsvr --bind_ip 192.168.0.2 --port 20002
  --dbpath /data/mongodb/replica/replica_2 \
  --logpath /data/mongodb/logs/replica_2.log
  ~~~

  server #3

  ~~~
  mongod \
  --fork \
  --replSet RS_2 \
  --shardsvr --bind_ip 192.168.0.3 --port 20002 \
  --dbpath /data/mongodb/replica/replica_2 \
  --logpath /data/mongodb/logs/replica_2.log
  ~~~

  server #4

  ~~~
  mongod \
  --fork \
  --replSet RS_2 \
  --shardsvr --bind_ip 192.168.0.4 --port 20002 \
  --dbpath /data/mongodb/replica/replica_2 \
  --logpath /data/mongodb/logs/replica_2.log
  ~~~

  server #3

  ~~~
  mongo --host 192.168.0.3 --port 20002
  ~~~

  ~~~
  var config={_id:'RS_2', 
  members:[
  {_id:0, host:'192.168.0.2:20002'}, 
  {_id:1, host:'192.168.0.3:20002'}, 
  {_id:2, host:'192.168.0.4:20002'}] 
  };

  rs.initiate(config)
  ~~~

- replica set #3


  server #2

  ~~~
  mongod \
  --fork \
  --replSet RS_3 
  --shardsvr --bind_ip 192.168.0.2 --port 20003 \
  --dbpath /data/mongodb/replica/replica_3 \
  --logpath /data/mongodb/logs/replica_3.log
  ~~~

  server #3

  ~~~
  mongod \
  --fork \
  --replSet RS_3 \
  --shardsvr --bind_ip 192.168.0.3 --port 20003 \
  --dbpath /data/mongodb/replica/replica_3 \
  --logpath /data/mongodb/logs/replica_3.log
  ~~~

  server #4

  ~~~
  mongod \
  --fork \
  --replSet RS_3 \
  --shardsvr --bind_ip 192.168.0.4 --port 20003 \
  --dbpath /data/mongodb/replica/replica_3 \
  --logpath /data/mongodb/logs/replica_3.log
  ~~~

  server #4

  ~~~
  mongo --host 192.168.0.4 --port 20003
  ~~~

  ~~~
  var config={_id:'RS_3', 
  members:[
  {_id:0, host:'192.168.0.2:20003'}, 
  {_id:1, host:'192.168.0.3:20003'}, 
  {_id:2, host:'192.168.0.4:20003'}] 
  };

  rs.initiate(config)
  ~~~