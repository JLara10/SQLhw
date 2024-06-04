# SQLhw part 1 

create table table1 (ID INT, Value VARCHAR(10));
Query OK, 0 rows affected (0.03 sec)

mysql> Insert Into table1 (ID, Value)
    -> SELECT 1, 'FIRST'
    -> UNION ALL
    -> SELECT 2, 'Second'
    -> UNION ALL
    -> SELECT 3, 'Third'
    -> UNION ALL 
    -> SELECT 4, 'Fourth'
    -> UNION ALL
    -> SELECT 5, 'Fifth';
Query OK, 5 rows affected (0.00 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> create table table2 (ID INT, Value VARCHAR(10));
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO table2 (ID, Value)
    -> SELECT 1, 'FIRST'
    -> UNION ALL
    -> SELECT 2, 'Second'
    -> UNION ALL
    -> SELECT 3, 'Third'
    -> UNION ALL
    -> SELECT 6, 'Sixth'
    -> UNION ALL
    -> SELECT 7, 'Seventh'
    -> UNION ALL
    -> SELECT 8, 'Eighth';
Query OK, 6 rows affected (0.00 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> select * from table1;
+------+--------+
| ID   | Value  |
+------+--------+
|    1 | FIRST  |
|    2 | Second |
|    3 | Third  |
|    4 | Fourth |
|    5 | Fifth  |
+------+--------+
5 rows in set (0.00 sec)

mysql> select table table2;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'table table2' at line 1
mysql> select * from table2;
+------+---------+
| ID   | Value   |
+------+---------+
|    1 | FIRST   |
|    2 | Second  |
|    3 | Third   |
|    6 | Sixth   |
|    7 | Seventh |
|    8 | Eighth  |
+------+---------+
6 rows in set (0.00 sec)

mysql> select t1.*,t2.* 
    -> from Table1 t1
    -> INNER JOIN table2 t2 ON t1.ID = t2.ID;
+------+--------+------+--------+
| ID   | Value  | ID   | Value  |
+------+--------+------+--------+
|    1 | FIRST  |    1 | FIRST  |
|    2 | Second |    2 | Second |
|    3 | Third  |    3 | Third  |
+------+--------+------+--------+
3 rows in set (0.00 sec)

mysql> select t1.*,t2.*
    -> from Table1 t1
    -> LEFT OUTER JOIN table2 t2 ON t1.ID = t2.ID;
+------+--------+------+--------+
| ID   | Value  | ID   | Value  |
+------+--------+------+--------+
|    1 | FIRST  |    1 | FIRST  |
|    2 | Second |    2 | Second |
|    3 | Third  |    3 | Third  |
|    4 | Fourth | NULL | NULL   |
|    5 | Fifth  | NULL | NULL   |
+------+--------+------+--------+
5 rows in set (0.00 sec)

mysql> select t1*,t2.*
    -> from Table1 t1
    -> RIGHT OUTER JOIN table2 t2 ON t1.ID = t2.ID;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ',t2.*
from Table1 t1
RIGHT OUTER JOIN table2 t2 ON t1.ID = t2.ID' at line 1
mysql> SELECT t1.ID AS T1ID, t1.Value AS T1Value,
    -> t2.ID T2ID, t2.Value AS t2Value
    -> FROM Table1 t1
    -> RIGHT JOIN Table2 t2 ON t1.ID = t2.ID;
+------+---------+------+---------+
| T1ID | T1Value | T2ID | t2Value |
+------+---------+------+---------+
|    1 | FIRST   |    1 | FIRST   |
|    2 | Second  |    2 | Second  |
|    3 | Third   |    3 | Third   |
| NULL | NULL    |    6 | Sixth   |
| NULL | NULL    |    7 | Seventh |
| NULL | NULL    |    8 | Eighth  |
+------+---------+------+---------+
6 rows in set (0.00 sec)

mysql> select t1*,t2.*
    -> from Table1 t1
    -> RIGHT JOIN table2 t2 ON t1.ID = t2.ID;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ',t2.*
from Table1 t1
RIGHT JOIN table2 t2 ON t1.ID = t2.ID' at line 1
mysql> select t1.ID AS T1ID, t1.Value AS T1Value, t2.ID T2ID, t2.Value AS T2Value
    -> FROM Table1 t1
    -> LEFT JOIN table2 t2 ON t1.ID = t2.ID 
    -> UNION 
    -> select t1.ID AS T1ID, t1.Value AS T1Value, t2.ID T2ID, t2.Value AS T2Value
    -> FROM Table1 t1
    -> RIGHT OUTER JOIN table2 t2 ON t1.ID = t2.ID; 
+------+---------+------+---------+
| T1ID | T1Value | T2ID | T2Value |
+------+---------+------+---------+
|    1 | FIRST   |    1 | FIRST   |
|    2 | Second  |    2 | Second  |
|    3 | Third   |    3 | Third   |
|    4 | Fourth  | NULL | NULL    |
|    5 | Fifth   | NULL | NULL    |
| NULL | NULL    |    6 | Sixth   |
| NULL | NULL    |    7 | Seventh |
| NULL | NULL    |    8 | Eighth  |
+------+---------+------+---------+
8 rows in set (0.01 sec)

mysql> select t1*,t2.*
    -> FROM table1 t1
    -> CROSS JOIN table2 t2;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ',t2.*
FROM table1 t1
CROSS JOIN table2 t2' at line 1
mysql> select t1.ID AS T1ID, t1.Value AS T1Value, t2.ID T2ID, t2.Value AS T2Value
    -> FROM table1 t1
    -> CROSS JOIN table2 t2;
+------+---------+------+---------+
| T1ID | T1Value | T2ID | T2Value |
+------+---------+------+---------+
|    5 | Fifth   |    1 | FIRST   |
|    4 | Fourth  |    1 | FIRST   |
|    3 | Third   |    1 | FIRST   |
|    2 | Second  |    1 | FIRST   |
|    1 | FIRST   |    1 | FIRST   |
|    5 | Fifth   |    2 | Second  |
|    4 | Fourth  |    2 | Second  |
|    3 | Third   |    2 | Second  |
|    2 | Second  |    2 | Second  |
|    1 | FIRST   |    2 | Second  |
|    5 | Fifth   |    3 | Third   |
|    4 | Fourth  |    3 | Third   |
|    3 | Third   |    3 | Third   |
|    2 | Second  |    3 | Third   |
|    1 | FIRST   |    3 | Third   |
|    5 | Fifth   |    6 | Sixth   |
|    4 | Fourth  |    6 | Sixth   |
|    3 | Third   |    6 | Sixth   |
|    2 | Second  |    6 | Sixth   |
|    1 | FIRST   |    6 | Sixth   |
|    5 | Fifth   |    7 | Seventh |
|    4 | Fourth  |    7 | Seventh |
|    3 | Third   |    7 | Seventh |
|    2 | Second  |    7 | Seventh |
|    1 | FIRST   |    7 | Seventh |
|    5 | Fifth   |    8 | Eighth  |
|    4 | Fourth  |    8 | Eighth  |
|    3 | Third   |    8 | Eighth  |
|    2 | Second  |    8 | Eighth  |
|    1 | FIRST   |    8 | Eighth  |
+------+---------+------+---------+
30 rows in set (0.00 sec)

mysql> select t1.ID AS T1ID, t1.Value AS T1Value, t2.ID T2ID, t2.Value AS T2Value
    -> FROM Table1 t1 
    -> NATURAL JOIN table2 t2;
+------+---------+------+---------+
| T1ID | T1Value | T2ID | T2Value |
+------+---------+------+---------+
|    1 | FIRST   |    1 | FIRST   |
|    2 | Second  |    2 | Second  |
|    3 | Third   |    3 | Third   |
+------+---------+------+---------+
3 rows in set (0.01 sec)

mysql> select t1.ID AS T1ID, t1.Value AS T1Value, t2.ID T2ID, t2.Value AS T2Value FROM Table1 t1  NATURAL LEFT JOIN table2 t2;
+------+---------+------+---------+
| T1ID | T1Value | T2ID | T2Value |
+------+---------+------+---------+
|    1 | FIRST   |    1 | FIRST   |
|    2 | Second  |    2 | Second  |
|    3 | Third   |    3 | Third   |
|    4 | Fourth  | NULL | NULL    |
|    5 | Fifth   | NULL | NULL    |
+------+---------+------+---------+
5 rows in set (0.00 sec)

mysql> select t1.ID T1ID, t1.Value AS T1Value 
    -> From table1 t1 
    -> UNION ALL 
    -> select t2.ID AS T2ID, t2.Value AS T2Value
    -> FROM Table2 t2;
+------+---------+
| T1ID | T1Value |
+------+---------+
|    1 | FIRST   |
|    2 | Second  |
|    3 | Third   |
|    4 | Fourth  |
|    5 | Fifth   |
|    1 | FIRST   |
|    2 | Second  |
|    3 | Third   |
|    6 | Sixth   |
|    7 | Seventh |
|    8 | Eighth  |
+------+---------+
11 rows in set (0.20 sec)

mysql> select t1.ID AS T1ID, t1.Value AS T1Value, t2.ID T2ID, t2.Value AS T2Value
    -> FROM Table1 t1
    -> LEFT JOIN Table2 t2 ON t1.ID = t2.ID
    -> UNION 
    -> SELECT t1.ID AS T1ID, t1.Value AS T1Value, t2.ID T2ID, t2.Value AS T2Value
    -> FROM Table1 t1 
    -> RIGHT OUTER JOIN Table2 t2 ON t1.ID = t2.ID;
+------+---------+------+---------+
| T1ID | T1Value | T2ID | T2Value |
+------+---------+------+---------+
|    1 | FIRST   |    1 | FIRST   |
|    2 | Second  |    2 | Second  |
|    3 | Third   |    3 | Third   |
|    4 | Fourth  | NULL | NULL    |
|    5 | Fifth   | NULL | NULL    |
| NULL | NULL    |    6 | Sixth   |
| NULL | NULL    |    7 | Seventh |
| NULL | NULL    |    8 | Eighth  |
+------+---------+------+---------+
8 rows in set (0.10 sec)

mysql> use sakila;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> select cust.customer_id, cust.first_name, cust.last_name
    -> from customer cust
    -> WHERE cust.customer_id IN
    -> (
    -> select ren.customer_id
    -> FROM rental ren
    -> WHERE ren.inventory_id IN 
    -> (
    -> select inv.inventory_id
    -> FROM inventory inv 
    -> WHERE inv.film_id IN 
    -> (
    -> select fl.film_id 
    -> from film fl
    -> where fl.film_id IN
    -> (
    -> select fc.film_id
    -> from film_category fc
    -> where fc.category_id IN
    -> (
    -> select cat.category_id
    -> FROM category cat
    -> where cat.name = 'Action'
    -> )))))
    -> Order BY cust.customer_id, cust.first_name, cust.last_name;
Empty set (0.02 sec)

mysql> desc sakila;
ERROR 1146 (42S02): Table 'sakila.sakila' doesn't exist
mysql> desc customer;
+-------------+-------------------+------+-----+-------------------+-----------------------------------------------+
| Field       | Type              | Null | Key | Default           | Extra                                         |
+-------------+-------------------+------+-----+-------------------+-----------------------------------------------+
| customer_id | smallint unsigned | NO   | PRI | NULL              | auto_increment                                |
| store_id    | tinyint unsigned  | NO   | MUL | NULL              |                                               |
| first_name  | varchar(45)       | NO   |     | NULL              |                                               |
| last_name   | varchar(45)       | NO   | MUL | NULL              |                                               |
| email       | varchar(50)       | YES  |     | NULL              |                                               |
| address_id  | smallint unsigned | NO   | MUL | NULL              |                                               |
| active      | tinyint(1)        | NO   |     | 1                 |                                               |
| create_date | datetime          | NO   |     | NULL              |                                               |
| last_update | timestamp         | YES  |     | CURRENT_TIMESTAMP | DEFAULT_GENERATED on update CURRENT_TIMESTAMP |
+-------------+-------------------+------+-----+-------------------+-----------------------------------------------+
9 rows in set (0.04 sec)

mysql> select * from customer;
Empty set (0.00 sec)

mysql> mysql> select payment_id, cust.first_name, cust.last_name, amount
    -> from payment pt
    -> INNNER JOIN customer cust ON cust.customer_id = pt.customer_id
    -> where amount > 
    -> (select AVG(amount)
    -> from payment pt1
    -> where pt1.customer_id = pt.customer_id)
    -> ORDER BY cust.customer_id;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'INNNER JOIN customer cust ON cust.customer_id = pt.customer_id
where amount > 
(' at line 3
mysql> show tables;
+----------------------------+
| Tables_in_sakila           |
+----------------------------+
| actor                      |
| actor_info                 |
| address                    |
| category                   |
| city                       |
| country                    |
| customer                   |
| customer_list              |
| film                       |
| film_actor                 |
| film_category              |
| film_list                  |
| film_text                  |
| inventory                  |
| language                   |
| nicer_but_slower_film_list |
| payment                    |
| rental                     |
| sales_by_film_category     |
| sales_by_store             |
| staff                      |
| staff_list                 |
| store                      |
+----------------------------+
23 rows in set (0.04 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| practice           |
| products           |
| sakila             |
| sys                |
| world              |
+--------------------+
8 rows in set (0.01 sec)

mysql> use practice;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+--------------------+
| Tables_in_practice |
+--------------------+
| table1             |
| table2             |
+--------------------+
2 rows in set (0.00 sec)

mysql> SELECT t1.ID AS T1ID, t1.Value AS T1Value
    -> FROM Table1 t1
    -> WHERE t1.ID NOT IN (SELECT t2.ID FROM Table2 t2);
+------+---------+
| T1ID | T1Value |
+------+---------+
|    4 | Fourth  |
|    5 | Fifth   |
+------+---------+
2 rows in set (0.04 sec)

mysql> SELECT t1.ID AS T1ID, t1.Value AS T1Value, t2.Value
    -> FROM Table1 t1
    -> LEFT OUTER JOIN Table2 t2 on t1.ID = t2.ID
    -> WHERE t2.Value IS NULL;
+------+---------+-------+
| T1ID | T1Value | Value |
+------+---------+-------+
|    4 | Fourth  | NULL  |
|    5 | Fifth   | NULL  |
+------+---------+-------+
2 rows in set (0.01 sec)





