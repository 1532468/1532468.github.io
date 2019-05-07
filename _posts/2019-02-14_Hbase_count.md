---
title : "Hbase count"
categories:
  - Eco System
---


Hbase에서 count를 할 때, 옵션을 줄 수 있다.

- INTERVAL 지정한 개수마다 화면에 출력한다.
- CACHE 크기가 클 수록 속도가 빠르다.


<!-- hbase > count 'table_name', INTERVAL => 1000000, CACHE => 1000
~~~
Current count: 1000000, row: row_num                                                
Current count: 2000000, row: row_num
Current count: 3000000, row: row_num 
Current count: 4000000, row: row_num                                                                        
Current count: 21000000, row: row_num
Current count: 22000000, row: row_num
Current count: 23000000, row: row_num
Current count: 24000000, row: row_num                                                                         
24807513 row(s) in 697.7710 seconds

=> 24807513
~~~

hbase > count 'table_name', INTERVAL => 1000000, CACHE => 10000
~~~
24807513 row(s) in 574.9870 seconds

=> 24807513
~~~

hbase > count 'table_name', INTERVAL => 1000000, CACHE => 100000
~~~
24807513 row(s) in 572.9220 seconds

=> 24807513
~~~

과유불급 -->