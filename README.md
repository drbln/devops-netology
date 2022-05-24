# Домашнее задание к занятию "6.5. Elasticsearch"
## Задача 1

### Dockerfile манифеста
~~~
FROM centos:7
LABEL HomeworkElastic 6.5

ENV PATH=/usr/lib:/usr/lib/jvm/jre-11/bin:$PATH

RUN yum update -y
RUN yum install java-11-openjdk -y
RUN yum install wget -y
RUN yum install perl-Digest-SHA -y

RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.2.0-linux-x86_64.tar.gz
RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.2.0-linux-x86_64.tar.gz.sha512
RUN shasum -a 512 -c elasticsearch-8.2.0-linux-x86_64.tar.gz.sha512
RUN tar -xzf elasticsearch-8.2.0-linux-x86_64.tar.gz

ADD elasticsearch.yml /elasticsearch-8.2.0/config/
ENV JAVA_HOME=/elasticsearch-8.2.0/jdk/
ENV ES_HOME=/elasticsearch-8.2.0
RUN groupadd elasticsearch \
    && useradd -g elasticsearch elasticsearch

RUN mkdir /var/lib/logs \
    && chown elasticsearch:elasticsearch /var/lib/logs \
    && mkdir /var/lib/data \
    && chown elasticsearch:elasticsearch /var/lib/data \
    && chown -R elasticsearch:elasticsearch /elasticsearch-8.2.0/
RUN mkdir /elasticsearch-8.2.0/snapshots &&\
    chown elasticsearch:elasticsearch /elasticsearch-8.2.0/snapshots
RUN export JAVA_HOME=/elasticsearch-8.2.0/jdk/

USER elasticsearch
CMD ["/usr/sbin/init"]
CMD ["/elasticsearch-8.2.0/bin/elasticsearch"]
~~~

### Ответ elasticsearch на запрос пути / в json виде

~~~
rudnev@Mac-mini ~ % curl -X GET 127.0.0.1:9200
{
  "name" : "69fdca54ffcf",
  "cluster_name" : "netology_test",
  "cluster_uuid" : "5ZVv6wKrRgC4-68aUs4khA",
  "version" : {
    "number" : "8.2.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "b174af62e8dd9f4ac4d25875e9381ffe2b9282c5",
    "build_date" : "2022-04-20T10:35:10.180408517Z",
    "build_snapshot" : false,
    "lucene_version" : "9.1.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}

~~~

### Ссылка на образ в репозитории dockerhub

~~~
docker push drbln/elk:tagname
https://hub.docker.com/repository/docker/drbln/elk
~~~


## Задача 2

~~~
rudnev@Mac-mini ~ % curl -X PUT localhost:9200/ind-1 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"ind-1"}%               rrurururrudrurudrurururrururrrrrrururrrrrrrrrrrrrurudnrudnrudrudrudrudrurudrrrururudrudrurudrururudnev@Mac-mini ~ % curl -X PUT localhost:9200/ind-2 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 2,  "number_of_replicas": 1 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"ind-2"}%                                                                                                               rudnev@Mac-mini ~ % curl -X PUT localhost:9200/ind-3 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 4,  "number_of_replicas": 2 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"ind-3"}%
~~~

~~~
rudnev@Mac-mini ~ %curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   ind-1 CsLSDO8nRmOtU1euAWV8lQ   1   0          0            0       225b           225b
yellow open   ind-2 zq_oOAxERc-7F2aiDueWuw   2   1          0            0       450b           450b
yellow open   ind-3 AVi5uktaQFyBH3qK2bsJmA   4   2          0            0       225b           225b
~~~

~~~
rudnev@Mac-mini ~ % curl -X GET 'http://localhost:9200/_cluster/health/ind-1?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 1,
  "active_shards" : 1,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
~~~

~~~
rudnev@Mac-mini ~ % curl -X GET 'http://localhost:9200/_cluster/health/ind-2?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 2,
  "active_shards" : 2,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 2,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 47.368421052631575
}
~~~

~~~
rudnev@Mac-mini ~ % curl -X GET 'http://localhost:9200/_cluster/health/ind-3?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 4,
  "active_shards" : 4,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 8,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 47.368421052631575
}
~~~

~~~
curl -XGET localhost:9200/_cluster/health/?pretty=true
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 7,
  "active_shards" : 7,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 10,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 41.176470588235
~~~

~~~
curl -X DELETE 'http://localhost:9200/ind-1?pretty'
{
  "acknowledged" : true
}

curl -X DELETE 'http://localhost:9200/ind-2?pretty'
{
  "acknowledged" : true
}

curl -X DELETE 'http://localhost:9200/ind-3?pretty'
{
  "acknowledged" : true
}

curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index uuid pri rep docs.count docs.deleted store.size pri.store.size
~~~

индексы в статусе Yellow потому что у них указано число реплик, а по факту нет других серверов для репликации

## Задача 3

В elasticsearch.yml была добавлена директива path.repo: /usr/share/elasticsearch/snapshots, сам запрос на регистрацию snapshot repo:
~~~
curl -XPOST localhost:9200/_snapshot/netology_backup?pretty -H 'Content-Type: application/json' -d'{"type": "fs", "settings": { "location":"/usr/share/elasticsearch/snapshots" }}'
{
  "acknowledged" : true
}
~~~


~~~
curl -X PUT localhost:9200/test -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"test"}1
~~~

~~~
{
  "test" : {
    "aliases" : { },
    "mappings" : { },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "test",
        "creation_date" : "1615033116650",
        "number_of_replicas" : "0",
        "uuid" : "P1duHjRTQBacJyWPGeNwUQ",
        "version" : {
          "created" : "7110199"
        }
      }
    }
  }
}
~~~

~~~
curl -X PUT localhost:9200/_snapshot/netology_backup/elasticsearch?wait_for_completion=true
~~~

~~~
[elasticsearch@69fdca54ffcf snapshots]$ ls -la
total 92
drwxr-xr-x 2 elasticsearch elasticsearch  4096 May  24 10:53 .
drwxr-xr-x 1 elasticsearch elasticsearch  4096 May  24 10:53 ..
-rw-r--r-- 1 elasticsearch elasticsearch   692 May  24 10:53 index-1
-rw-r--r-- 1 elasticsearch elasticsearch     8 May  24 10:53 index.latest
drwxr-xr-x 3 elasticsearch elasticsearch  4096 May  24 10:53 indices
-rw-r--r-- 1 elasticsearch elasticsearch 30931 May  24 10:53 meta-qnkfqjUhTNiyJLt99z-N5Q.dat
-rw-r--r-- 1 elasticsearch elasticsearch 30931 May  24 10:53 meta-wixOT9zMS_WYXlGfNw7nsQ.dat
-rw-r--r-- 1 elasticsearch elasticsearch   267 May  24 10:53 snap-qnkfqjUhTNiyJLt99z-N5Q.dat
-rw-r--r-- 1 elasticsearch elasticsearch   269 May  24 10:53 snap-wixOT9zMS_WYXlGfNw7nsQ.dat
~~~


~~~
curl -X DELETE 'http://localhost:9200/test?pretty'
{
  "acknowledged" : true
}

curl -X PUT localhost:9200/test-2?pretty -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "test-2"
}

curl -X POST localhost:9200/_snapshot/netology_backup/elasticsearch/_restore?pretty -H 'Content-Type: application/json' -d'{"include_global_state":true}'
{
  "accepted" : true
}

curl -X GET http://localhost:9200/_cat/indices?v
health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test-2 qwS4ntA_RRig2_gBq_w2cg   1   0          0            0       208b           208b
green  open   test   HzFsM8gwT1WPFxzDLl0Uew   1   0          0            0       208b           208b
~~~
