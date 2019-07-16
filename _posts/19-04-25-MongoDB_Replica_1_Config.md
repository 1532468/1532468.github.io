---
title : "[MongoDB Replica]-1 Config"
categories:
  - MongoDB
---
config server 설정

<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/logo.jpg">
  <figcaption></figcaption>
</figure>

<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/cluster.PNG">
  <figcaption></figcaption>
</figure>

1. config

MongoDB version 4.0.8


- server #2

  ~~~
  mongod \
  --fork \
  --configsvr \
  --replSet config --bind_ip 192.168.0.2 --port 10000 \
  --dbpath /data/mongodb/configdb \
  --logpath /data/mongodb/logs/config.log
  ~~~

- server #3

  ~~~
  mongod \
  --fork \
  --configsvr \
  --replSet config --bind_ip 192.168.0.3 --port 10000 \
  --dbpath /data/mongodb/configdb \
  --logpath /data/mongodb/logs/config.log
  ~~~

- server #4

  ~~~
  mongod \
  --fork \
  --configsvr \
  --replSet config --bind_ip 192.168.0.4 --port 10000 \
  --dbpath /data/mongodb/configdb \
  --logpath /data/mongodb/logs/config.log
  ~~~


- server #1

  ~~~
  mongo --host 192.168.0.1 --port 10000
  ~~~

  ~~~
  var config = {

      _id : "config", members : [ 
          {_id : 0, host : '192.168.0.2:10000'},
          {_id : 1, host : '192.168.0.3:10000'},
          {_id : 2, host : '192.168.0.4:10000'}  
      ]
  }

  rs.initiate(config)
  ~~~
