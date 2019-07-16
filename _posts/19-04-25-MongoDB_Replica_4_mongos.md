---
title : "MongoDB Replica"
categories:
  - MongoDB
---
mongos 설정

<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/logo.jpg">
  <figcaption></figcaption>
</figure>

<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/cluster.PNG">
  <figcaption></figcaption>
</figure>

4. mongos

- server #1

  ~~~
  mongos \
  --fork \
  --bind_ip 192.168.0.1 --port 50000 \
  --configdb config/192.168.0.2:10000, 192.168.0.3:10000, 192.168.0.4:10000 \
  --logpath /data/mongodb/logs/mongos.log
  ~~~

  ~~~
  mongo --host 192.168.0.1 --port 50000
  ~~~

  ~~~
  use admin
  sh.addShard("RS_1/192.168.0.2:30001,192.168.0.3:30001,192.168.0.4:30001")
  sh.addShard("RS_2/192.168.0.2:30002,192.168.0.3:30002,192.168.0.4:30002")
  sh.addShard("RS_3/192.168.0.2:30003,192.168.0.3:30003,192.168.0.4:30003")
  ~~~

5. shard \

모든 서버 작업은 끝이 났다. 마지막으로 shard key를 설정하면 된다.

    ~~~
    use admin

    db.runCommand({enablesharding:'DB_NAME'});
    db.runCommand({shardcollection:'DB_NAME.TABLE_ONE', key:{ID:'hashed'}}); 
    db.runCommand({shardcollection:'DB_NAME.TABLE_TWO', key:{ID:'1'}});
    db.runCommand({shardcollection:'DB_NAME.TABLE_THREE', key:{ID:1, name:1}}); 
    ~~~