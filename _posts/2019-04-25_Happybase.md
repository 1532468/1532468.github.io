---
title : "Python Happybase"
categories:
  - blogging
---
<figure>
  <img src="/assets/images/2019-04-25-Happybase/hbase.png">
  <figcaption></figcaption>
</figure>

## python에서 hbase를 해보자!

pip install happybase


happybase를 이용할 것이다.

happybase는 Thrift를 이용한다.
```
hbase-daemon.sh start thrift
```
기본적으로 9090 포트가 열린다. 의심스러우면..
```
hbase-daemon.sh start thrift -p 9090
```


```
import happybase
connection = happybase.Connection('192.168.0.25', 9090)
print(connection.tables())

table = connection.table('cf_test')
table.put(b'1', {b'cf1:name': msg.value})


rows
scan

```
https://happybase.readthedocs.io/en/latest/

```
import happybase
import time
from time import sleep

BUCKET_NUM = 9
SOURCE = 'TST_WS'

def Salt(id) :
    salt = (int)(id % BUCKET_NUM)
    return str(salt) + '_' + str(id) + '_' + SOURCE

from_hbase = happybase.Connection('133.186.168.10', 9090) # Pure HBase
to_hbase = happybase.Connection('121.134.221.132', 9090) # Dev HBase

from_table = from_hbase.table('H_MWS_COLT_EVT')
to_table = to_hbase.table('MWS_COLT_EVT')

id = 0
cnt = 0
start = time.time()

while cnt < 10000 :
    sleep(1)
    for key, data in from_table.rows([bytes(Salt(id), 'utf-8')]):
        for k in data.keys() :
            to_table.put(key, {k:data[k]})
            cnt = cnt + 1
        id = id + 1

print("last ID is " + str(id))
print("Runtime: %0.2f Minutes" % ((time.time() - start_vect)/60))
        
        
#for key, data in from_table.scan(row_start=bytes(Salt(id), 'utf-8')):
#    print(key, data)
#    for k in data.keys() :
#        print(id)
#        to_table.put(key, {k:data[k]})
```