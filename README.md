# Домашнее задание к занятию "6.4. PostgreSQL"

## Задача 1

~~~
docker pull postgres:13

docker volume create postgres

docker run --rm --name postgres -e POSTGRES_PASSWORD=postgres -ti -d -p 5432:5432 -v vol1:/var/lib/postgresql/data postgres:13

docker exec -it postgres bash
~~~

~~~
вывод списка БД
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges

-----------+----------+----------+------------+------------+--------------------
---
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres        
  +
           |          |          |            |            | postgres=CTc/postgr
es
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres        
  +
           |          |          |            |            | postgres=CTc/postgr
es
(3 rows)
~~~

~~~
Подключение в БД
postgres=# \c postgres
You are now connected to database "postgres" as user "postgres".
~~~

~~~
Вывод списка таблиц
postgres=# \dt
Did not find any relations.

в таблицах пусто , исползовал оп параметр S - для системных объектов
postgres=# \dtS
                    List of relations
   Schema   |          Name           | Type  |  Owner   
------------+-------------------------+-------+----------
 pg_catalog | pg_aggregate            | table | postgres
 pg_catalog | pg_am                   | table | postgres
 pg_catalog | pg_amop                 | table | postgres
 pg_catalog | pg_amproc               | table | postgres
 pg_catalog | pg_attrdef              | table | postgres
 pg_catalog | pg_attribute            | table | postgres
 pg_catalog | pg_auth_members         | table | postgres

~~~

~~~
вывода описания содержимого таблиц
postgres=# \dS+ pg_index
                                      Table "pg_catalog.pg_index"
     Column     |     Type     | Collation | Nullable | Default | Storage  | Stats target | Desc
ription
----------------+--------------+-----------+----------+---------+----------+--------------+-----
--------
 indexrelid     | oid          |           | not null |         | plain    |              |
 indrelid       | oid          |           | not null |         | plain    |              |
 indnatts       | smallint     |           | not null |         | plain    |              |
 indnkeyatts    | smallint     |           | not null |         | plain    |              |
 indisunique    | boolean      |           | not null |         | plain    |              |
 indisprimary   | boolean      |           | not null |         | plain    |              |
 indisexclusion | boolean      |           | not null |         | plain    |              |
 indimmediate   | boolean      |           | not null |         | plain    |              |
 indisclustered | boolean      |           | not null |         | plain    |              |
 indisvalid     | boolean      |           | not null |         | plain    |              |
~~~

~~~
Выход из psql

postgres=# \q
~~~

## Задача 2

~~~
postgres=# CREATE DATABASE test_database
postgres-# ;
CREATE DATABASE

root@8a4d48647ab6:/var/lib/postgresql# psql -U postgres -f ./test_dump.sql test_database

postgres=# \c test_database

test_database=# ANALYZE VERBOSE public.orders;
INFO:  analyzing "public.orders"
INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
ANALYZE
test_database=#


test_database=# select avg_width from pg_stats where tablename='orders';
 avg_width
-----------
         4
        16
         4
(3 rows)


~~~


## Задача 3

~~~
test_database=# alter table orders rename to orders_simple;
ALTER TABLE

test_database=# create table orders (id integer, title varchar(80), price integer) partition by range(price);
CREATE TABLE

test_database=# create table orders_less499 partition of orders for values from (0) to (499);
CREATE TABLE

test_database=# create table orders_more499 partition of orders for values from (499) to (999999999);
CREATE TABLE

test_database=# insert into orders (id, title, price) select * from orders_simple;
INSERT 0 8

При изначальном проектировании таблиц можно было сделать ее секционированной, тогда не пришлось бы переименовывать исходную таблицу и переносить данные в новую.

~~~

## Задача 4

~~~
root@8a4d48647ab6:/var/lib/postgresql# pg_dump -U postgres -d test_database >test_database_dump.sql

можно добавить индекс
CREATE INDEX ON orders ((lower(title)));
~~~
