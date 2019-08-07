---
title : "Python ElasticSearch"
categories:
  - Python
---
MongoDB - Python - ElasticSearch

'logstash' 라는 Elastic Stack에 포함되어 있는 것이 있지만.. 난 python이다.<br>
plugin이 ruby로 되어있고..상황에 맞게 pipeline의 성격을 내가 구현하지 못할 듯 하다.^^

### 닥치고 code

~~~python
import pymongo

import pandas as pd

from elasticsearch import Elasticsearch
from elasticsearch import helpers

class My_MongoDB() :
    def __init__(self) :
        self.uri = "mongodb://192.168.0.100"
        self.port = 27017
        self.client = pymongo.MongoClient(self.uri, self.port)

    def Get_Data(self, db, collection) :
        return self.client[db][collection].find({}).limit(10)
            
    def __del__(self) :
        self.client.close()

class My_Elasticsearch() :
    def __init__(self) :
        self.es = Elasticsearch(hosts="192.168.0.200")
        
    def Search(self, _index, _body) :
        return self.es.search(index=_index, body={_body})
    
    def Insert(self, _index, _type, _data) :
        helpers.bulk(self.es, _data, index=_index, doc_type=_type)
        # es.index(index="_index, doc_type=_type, body=_data)

def main() :
    mongodb = My_MongoDB()
    mongo_data = pd.DataFrame(mongodb.Get_Data("MONGO_DB", "DATA"))

    es = My_Elasticsearch()
    es.Insert("index", "type", mongo_data.to_dict('records'))
~~~
