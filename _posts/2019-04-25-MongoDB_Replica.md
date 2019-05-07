---
title : "MongoDB Replica"
categories:
  - MongoDB
---
<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/logo.jpg">
  <figcaption></figcaption>
</figure>

<figure>
  <img src="/assets/images/2019-04-25-MongoDB_Replica/cluster.PNG">
  <figcaption></figcaption>
</figure>

1. config

server #2
~~~
sudo mongod --configsvr --replSet config --bind_ip 192.168.0.2 --port 10000 --fork \
--dbpath /data/mongodb/configdb --logpath /data/mongodb/logs/config.log
~~~


server #3
~~~
sudo mongod --configsvr --replSet config --bind_ip 192.168.0.3 --port 10000 --fork \
--dbpath /data/mongodb/configdb --logpath /data/mongodb/logs/config.log
~~~

server #4
~~~
sudo mongod --configsvr --replSet config --bind_ip 192.168.0.4 --port 10000 --fork \
--dbpath /data/mongodb/configdb --logpath /data/mongodb/logs/config.log
~~~


server #2
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

2. replica set \
뭔가 많아 보이지만 간단합니다.\
replica set 이름과 port만 바꿉니다.

replica set #1

server #2

~~~
sudo mongod --replSet RS_1 --shardsvr --bind_ip 192.168.0.2 --port 20001 --fork \
--dbpath /data/mongodb/replica/replica_1 --logpath /data/mongodb/logs/replica_1.log
~~~

server #3
~~~
sudo mongod --replSet RS_1 --shardsvr --bind_ip 192.168.0.3 --port 20001 --fork \
--dbpath /data/mongodb/replica/replica_1 --logpath /data/mongodb/logs/replica_1.log
~~~

server #4
~~~
sudo mongod --replSet RS_1 --shardsvr --bind_ip 192.168.0.4 --port 20001 --fork \
--dbpath /data/mongodb/replica/replica_1 --logpath /data/mongodb/logs/replica_1.log
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
sudo mongod --replSet RS_2 --shardsvr --bind_ip 192.168.0.2 --port 20002 --fork \
--dbpath /data/mongodb/replica/replica_1 --logpath /data/mongodb/logs/replica_2.log
~~~

server #3
~~~
sudo mongod --replSet RS_2 --shardsvr --bind_ip 192.168.0.3 --port 20002 --fork \
--dbpath /data/mongodb/replica/replica_1 --logpath /data/mongodb/logs/replica_2.log
~~~

server #4
~~~
sudo mongod --replSet RS_2 --shardsvr --bind_ip 192.168.0.4 --port 20002 --fork \
--dbpath /data/mongodb/replica/replica_1 --logpath /data/mongodb/logs/replica_2.log
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
sudo mongod --replSet RS_2 --shardsvr --bind_ip 192.168.0.2 --port 20003 --fork \
--dbpath /data/mongodb/replica/replica_1 --logpath /data/mongodb/logs/replica_2.log
~~~

server #3
~~~
sudo mongod --replSet RS_2 --shardsvr --bind_ip 192.168.0.3 --port 20003 --fork \
--dbpath /data/mongodb/replica/replica_1 --logpath /data/mongodb/logs/replica_2.log
~~~

server #4
~~~
sudo mongod --replSet RS_2 --shardsvr --bind_ip 192.168.0.4 --port 20003 --fork \
--dbpath /data/mongodb/replica/replica_1 --logpath /data/mongodb/logs/replica_2.log
~~~

server #4
~~~
mongo --host 192.168.0.3 --port 20003
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

3. arbiter

~~~
sudo mongod --replSet RS_1 --bind_ip 192.168.0.1 --port 30001 --fork \
--dbpath /data/mongodb/arbiter/shard01 --logpath /data/mongodb/logs/arbiter_1.log
~~~
~~~
sudo mongod --replSet RS_2 --bind_ip 192.168.0.1 --port 30002 --fork \
--dbpath /data/mongodb/arbiter/shard02 --logpath /data/mongodb/logs/arbiter_2.log
~~~
~~~
sudo mongod --replSet RS_3 --bind_ip 192.168.0.1 --port 30003 --fork \
--dbpath /data/mongodb/arbiter/shard03 --logpath /data/mongodb/logs/arbiter_3.log
~~~

server #2
~~~
mongo --host 192.168.2 --port 20001
~~~
~~~
rs.addArb("192.168.0.1:30001")
~~~

server #3
~~~
mongo --host 192.168.3 --port 20002
~~~
~~~
rs.addArb("192.168.0.1:30002")
~~~

server #4
~~~
mongo --host 192.168.4 --port 20003
~~~
~~~
rs.addArb("192.168.0.1:30003")
~~~

4. mongos

server #1
~~~
sudo mongos --bind_ip 192.168.0.1 --port 50000 --fork \
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