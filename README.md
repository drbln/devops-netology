# Домашнее задание к занятию "6.2. SQL"

## Задача 1

~~~
docker pull postgres:12

C:\Users\o.rudnev>docker volume create vol1
vol1

C:\Users\o.rudnev>docker volume create vol2
vol2

docker run -d -it -e POSTGRES_PASSWORD=postgres -v vol1:/var/lib/postgresql/data -v vol2:/var/lib/postgresql -p 5432:5432 --name postgres postgres:12
9456d559732ef544c2483659a328b2f44299cd6ce33277df0505ed1c1c7690e0

~~~

## Задача 2

~~~
docker exec -it postgres psql -U postgres

postgres=# CREATE ROLE "test-admin-user" SUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN
postgres-# ;
CREATE ROLE
postgres=# CREATE DATABASE test_db
postgres-# ;
CREATE DATABASE

postgres=# \c test_db

test_db=# CREATE TABLE orders (id SERIAL PRIMARY KEY, name TEXT, price INTEGER)
;
CREATE TABLE

test_db=# CREATE TABLE clients
(
id SERIAL PRIMARY KEY,
lastmane TEXT,
county TEXT,
booking INTEGER,
FOREIGN KEY (booking) REFERENCES orders (id)
)
;
CREATE TABLE

test_db=# CREATE ROLE "test-simple-user" NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;
CREATE ROLE

test_db=# GRANT SELECT ON TABLE clients TO "test-simple-user";
GRANT
test_db=# GRANT INSERT ON TABLE clients TO "test-simple-user";
GRANT
test_db=# GRANT UPDATE ON TABLE clients TO "test-simple-user";
GRANT
test_db=# GRANT DELETE ON TABLE clients TO "test-simple-user";
GRANT

test_db=# GRANT SELECT ON TABLE orders TO "test-simple-user";
GRANT
test_db=# GRANT INSERT ON TABLE orders TO "test-simple-user";
GRANT
test_db=# GRANT UPDATE ON TABLE orders TO "test-simple-user";
GRANT
test_db=# GRANT DELETE ON TABLE orders TO "test-simple-user";
GRANT
~~~

~~~
test_db=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 test_db   | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
(4 rows)

~~~

~~~
test_db=# \dt
          List of relations
 Schema |  Name   | Type  |  Owner
--------+---------+-------+----------
 public | clients | table | postgres
 public | orders  | table | postgres
(2 rows)
~~~

~~~
test_db=# \du
                                       List of roles
    Role name     |                         Attributes                         | Member of
------------------+------------------------------------------------------------+-----------
 postgres         | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 test-admin-user  | Superuser, No inheritance                                  | {}
 test-simple-user | No inheritance                                             | {}

~~~

~~~
test_db=# SELECT * FROM information_schema.table_privileges WHERE grantee in ('test-admin-user', 'test-simple-user');
 grantor  |     grantee      | table_catalog | table_schema | table_name | privilege_type | is_grantable | with_hierarchy
----------+------------------+---------------+--------------+------------+----------------+--------------+----------------
 postgres | test-simple-user | test_db       | public       | clients    | INSERT         | NO           | NO
 postgres | test-simple-user | test_db       | public       | clients    | SELECT         | NO           | YES
 postgres | test-simple-user | test_db       | public       | clients    | UPDATE         | NO           | NO
 postgres | test-simple-user | test_db       | public       | clients    | DELETE         | NO           | NO
 postgres | test-simple-user | test_db       | public       | orders     | INSERT         | NO           | NO
 postgres | test-simple-user | test_db       | public       | orders     | SELECT         | NO           | YES
 postgres | test-simple-user | test_db       | public       | orders     | UPDATE         | NO           | NO
 postgres | test-simple-user | test_db       | public       | orders     | DELETE         | NO           | NO
(8 rows)
~~~

## Задача 3

~~~
test_db=# INSERT INTO orders VALUES (1, 'Шоколад', 10), (2, 'Принтер', 3000), (3, 'Книга', 500), (4, 'Монитор', 7000), (5, 'Гитара', 4000);
INSERT 0 5

test_db=# INSERT INTO clients VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович','Canada'), (3, 'Иоганн Себастьян Бах','Japan'),
(4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');
INSERT 0 5
~~~

~~~
test_db=# SELECT COUNT(*) FROM orders;
 count
-------
     5
(1 row)

test_db=# SELECT COUNT(*) FROM clients;
 count
-------
     5
(1 row)
~~~

## Задача 4

~~~
test_db=# UPDATE clients SET booking = 3 WHERE id = 1;
UPDATE 1
test_db=# UPDATE clients SET booking = 4 WHERE id = 2;
UPDATE 1
test_db=# UPDATE clients SET booking = 5 WHERE id = 3;
UPDATE 1

test_db=# SELECT * FROM clients WHERE booking is not null;
 id |       lastmane       | county | booking
----+----------------------+--------+---------
  1 | Иванов Иван Иванович | USA    |       3
  2 | Петров Петр Петрович | Canada |       4
  3 | Иоганн Себастьян Бах | Japan  |       5
(3 rows)
~~~

## Задача 5

~~~
test_db=# EXPLAIN SELECT * FROM clients WHERE booking is not null;
                        QUERY PLAN
-----------------------------------------------------------
 Seq Scan on clients  (cost=0.00..18.10 rows=806 width=72)
   Filter: (booking IS NOT NULL)
(2 rows)

Показывает стоимость(нагрузку на исполнение) запроса , и фильтрацию по полю Booking для выборки.
~~~

## Задача 6

~~~
postgres@15300572a54d:~$ pg_dump test_db > backup_test_db

docker run -d -it -e POSTGRES_PASSWORD=postgres -v vol1:/var/lib/postgresql/data -v vol2:/var/lib/postgresql -p 5432:5432 --name postgres2 postgres:12

C:\Users\o.rudnev>docker exec -it postgres2 su - postgres

postgres=# CREATE DATABASE test_db;
CREATE DATABASE
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 test_db   | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
(4 rows)

postgres=# exit
postgres@1f370e3169ef:~$ psql -f backup_test_db test_db

postgres@1f370e3169ef:~$ psql
psql (12.10 (Debian 12.10-1.pgdg110+1))
Type "help" for help.

postgres=# \c test_db

test_db=# SELECT * FROM clients WHERE booking is not null;
 id |                lastmane                | county | booking
----+----------------------------------------+--------+---------
  1 | Иванов Иван Иванович | USA    |       3
  2 | Петров Петр Петрович | Canada |       4
  3 | Иоганн Себастьян Бах | Japan  |       5
(3 rows)
~~~
