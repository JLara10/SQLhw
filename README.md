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



SQL HW part 2

use world;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql>   SELECT count(*)
    -> FROM city
    -> WHERE countrycode = 'USA';
+----------+
| count(*) |
+----------+
|      274 |
+----------+
1 row in set (0.05 sec)

mysql> SELECT population, lifeexpectancy
    -> FROM country
    -> WHERE code = 'ARG';
+------------+----------------+
| population | lifeexpectancy |
+------------+----------------+
|   37032000 |           75.1 |
+------------+----------------+
1 row in set (0.00 sec)

mysql> SELECT * 
    -> FROM city 
    -> WHERE countrycode = 'BRA';
+-----+--------------------------+-------------+---------------------+------------+
| ID  | Name                     | CountryCode | District            | Population |
+-----+--------------------------+-------------+---------------------+------------+
| 206 | São Paulo                | BRA         | São Paulo           |    9968485 |
| 207 | Rio de Janeiro           | BRA         | Rio de Janeiro      |    5598953 |
| 208 | Salvador                 | BRA         | Bahia               |    2302832 |
| 209 | Belo Horizonte           | BRA         | Minas Gerais        |    2139125 |
| 210 | Fortaleza                | BRA         | Ceará               |    2097757 |
| 211 | Brasília                 | BRA         | Distrito Federal    |    1969868 |
| 212 | Curitiba                 | BRA         | Paraná              |    1584232 |
| 213 | Recife                   | BRA         | Pernambuco          |    1378087 |
| 214 | Porto Alegre             | BRA         | Rio Grande do Sul   |    1314032 |
| 215 | Manaus                   | BRA         | Amazonas            |    1255049 |
| 216 | Belém                    | BRA         | Pará                |    1186926 |
| 217 | Guarulhos                | BRA         | São Paulo           |    1095874 |
| 218 | Goiânia                  | BRA         | Goiás               |    1056330 |
| 219 | Campinas                 | BRA         | São Paulo           |     950043 |
| 220 | São Gonçalo              | BRA         | Rio de Janeiro      |     869254 |
| 221 | Nova Iguaçu              | BRA         | Rio de Janeiro      |     862225 |
| 222 | São Luís                 | BRA         | Maranhão            |     837588 |
| 223 | Maceió                   | BRA         | Alagoas             |     786288 |
| 224 | Duque de Caxias          | BRA         | Rio de Janeiro      |     746758 |
| 225 | São Bernardo do Campo    | BRA         | São Paulo           |     723132 |
| 226 | Teresina                 | BRA         | Piauí               |     691942 |
| 227 | Natal                    | BRA         | Rio Grande do Norte |     688955 |
| 228 | Osasco                   | BRA         | São Paulo           |     659604 |
| 229 | Campo Grande             | BRA         | Mato Grosso do Sul  |     649593 |
| 230 | Santo André              | BRA         | São Paulo           |     630073 |
| 231 | João Pessoa              | BRA         | Paraíba             |     584029 |
| 232 | Jaboatão dos Guararapes  | BRA         | Pernambuco          |     558680 |
| 233 | Contagem                 | BRA         | Minas Gerais        |     520801 |
| 234 | São José dos Campos      | BRA         | São Paulo           |     515553 |
| 235 | Uberlândia               | BRA         | Minas Gerais        |     487222 |
| 236 | Feira de Santana         | BRA         | Bahia               |     479992 |
| 237 | Ribeirão Preto           | BRA         | São Paulo           |     473276 |
| 238 | Sorocaba                 | BRA         | São Paulo           |     466823 |
| 239 | Niterói                  | BRA         | Rio de Janeiro      |     459884 |
| 240 | Cuiabá                   | BRA         | Mato Grosso         |     453813 |
| 241 | Juiz de Fora             | BRA         | Minas Gerais        |     450288 |
| 242 | Aracaju                  | BRA         | Sergipe             |     445555 |
| 243 | São João de Meriti       | BRA         | Rio de Janeiro      |     440052 |
| 244 | Londrina                 | BRA         | Paraná              |     432257 |
| 245 | Joinville                | BRA         | Santa Catarina      |     428011 |
| 246 | Belford Roxo             | BRA         | Rio de Janeiro      |     425194 |
| 247 | Santos                   | BRA         | São Paulo           |     408748 |
| 248 | Ananindeua               | BRA         | Pará                |     400940 |
| 249 | Campos dos Goytacazes    | BRA         | Rio de Janeiro      |     398418 |
| 250 | Mauá                     | BRA         | São Paulo           |     375055 |
| 251 | Carapicuíba              | BRA         | São Paulo           |     357552 |
| 252 | Olinda                   | BRA         | Pernambuco          |     354732 |
| 253 | Campina Grande           | BRA         | Paraíba             |     352497 |
| 254 | São José do Rio Preto    | BRA         | São Paulo           |     351944 |
| 255 | Caxias do Sul            | BRA         | Rio Grande do Sul   |     349581 |
| 256 | Moji das Cruzes          | BRA         | São Paulo           |     339194 |
| 257 | Diadema                  | BRA         | São Paulo           |     335078 |
| 258 | Aparecida de Goiânia     | BRA         | Goiás               |     324662 |
| 259 | Piracicaba               | BRA         | São Paulo           |     319104 |
| 260 | Cariacica                | BRA         | Espírito Santo      |     319033 |
| 261 | Vila Velha               | BRA         | Espírito Santo      |     318758 |
| 262 | Pelotas                  | BRA         | Rio Grande do Sul   |     315415 |
| 263 | Bauru                    | BRA         | São Paulo           |     313670 |
| 264 | Porto Velho              | BRA         | Rondônia            |     309750 |
| 265 | Serra                    | BRA         | Espírito Santo      |     302666 |
| 266 | Betim                    | BRA         | Minas Gerais        |     302108 |
| 267 | Jundíaí                  | BRA         | São Paulo           |     296127 |
| 268 | Canoas                   | BRA         | Rio Grande do Sul   |     294125 |
| 269 | Franca                   | BRA         | São Paulo           |     290139 |
| 270 | São Vicente              | BRA         | São Paulo           |     286848 |
| 271 | Maringá                  | BRA         | Paraná              |     286461 |
| 272 | Montes Claros            | BRA         | Minas Gerais        |     286058 |
| 273 | Anápolis                 | BRA         | Goiás               |     282197 |
| 274 | Florianópolis            | BRA         | Santa Catarina      |     281928 |
| 275 | Petrópolis               | BRA         | Rio de Janeiro      |     279183 |
| 276 | Itaquaquecetuba          | BRA         | São Paulo           |     270874 |
| 277 | Vitória                  | BRA         | Espírito Santo      |     270626 |
| 278 | Ponta Grossa             | BRA         | Paraná              |     268013 |
| 279 | Rio Branco               | BRA         | Acre                |     259537 |
| 280 | Foz do Iguaçu            | BRA         | Paraná              |     259425 |
| 281 | Macapá                   | BRA         | Amapá               |     256033 |
| 282 | Ilhéus                   | BRA         | Bahia               |     254970 |
| 283 | Vitória da Conquista     | BRA         | Bahia               |     253587 |
| 284 | Uberaba                  | BRA         | Minas Gerais        |     249225 |
| 285 | Paulista                 | BRA         | Pernambuco          |     248473 |
| 286 | Limeira                  | BRA         | São Paulo           |     245497 |
| 287 | Blumenau                 | BRA         | Santa Catarina      |     244379 |
| 288 | Caruaru                  | BRA         | Pernambuco          |     244247 |
| 289 | Santarém                 | BRA         | Pará                |     241771 |
| 290 | Volta Redonda            | BRA         | Rio de Janeiro      |     240315 |
| 291 | Novo Hamburgo            | BRA         | Rio Grande do Sul   |     239940 |
| 292 | Caucaia                  | BRA         | Ceará               |     238738 |
| 293 | Santa Maria              | BRA         | Rio Grande do Sul   |     238473 |
| 294 | Cascavel                 | BRA         | Paraná              |     237510 |
| 295 | Guarujá                  | BRA         | São Paulo           |     237206 |
| 296 | Ribeirão das Neves       | BRA         | Minas Gerais        |     232685 |
| 297 | Governador Valadares     | BRA         | Minas Gerais        |     231724 |
| 298 | Taubaté                  | BRA         | São Paulo           |     229130 |
| 299 | Imperatriz               | BRA         | Maranhão            |     224564 |
| 300 | Gravataí                 | BRA         | Rio Grande do Sul   |     223011 |
| 301 | Embu                     | BRA         | São Paulo           |     222223 |
| 302 | Mossoró                  | BRA         | Rio Grande do Norte |     214901 |
| 303 | Várzea Grande            | BRA         | Mato Grosso         |     214435 |
| 304 | Petrolina                | BRA         | Pernambuco          |     210540 |
| 305 | Barueri                  | BRA         | São Paulo           |     208426 |
| 306 | Viamão                   | BRA         | Rio Grande do Sul   |     207557 |
| 307 | Ipatinga                 | BRA         | Minas Gerais        |     206338 |
| 308 | Juazeiro                 | BRA         | Bahia               |     201073 |
| 309 | Juazeiro do Norte        | BRA         | Ceará               |     199636 |
| 310 | Taboão da Serra          | BRA         | São Paulo           |     197550 |
| 311 | São José dos Pinhais     | BRA         | Paraná              |     196884 |
| 312 | Magé                     | BRA         | Rio de Janeiro      |     196147 |
| 313 | Suzano                   | BRA         | São Paulo           |     195434 |
| 314 | São Leopoldo             | BRA         | Rio Grande do Sul   |     189258 |
| 315 | Marília                  | BRA         | São Paulo           |     188691 |
| 316 | São Carlos               | BRA         | São Paulo           |     187122 |
| 317 | Sumaré                   | BRA         | São Paulo           |     186205 |
| 318 | Presidente Prudente      | BRA         | São Paulo           |     185340 |
| 319 | Divinópolis              | BRA         | Minas Gerais        |     185047 |
| 320 | Sete Lagoas              | BRA         | Minas Gerais        |     182984 |
| 321 | Rio Grande               | BRA         | Rio Grande do Sul   |     182222 |
| 322 | Itabuna                  | BRA         | Bahia               |     182148 |
| 323 | Jequié                   | BRA         | Bahia               |     179128 |
| 324 | Arapiraca                | BRA         | Alagoas             |     178988 |
| 325 | Colombo                  | BRA         | Paraná              |     177764 |
| 326 | Americana                | BRA         | São Paulo           |     177409 |
| 327 | Alvorada                 | BRA         | Rio Grande do Sul   |     175574 |
| 328 | Araraquara               | BRA         | São Paulo           |     174381 |
| 329 | Itaboraí                 | BRA         | Rio de Janeiro      |     173977 |
| 330 | Santa Bárbara d´Oeste    | BRA         | São Paulo           |     171657 |
| 331 | Nova Friburgo            | BRA         | Rio de Janeiro      |     170697 |
| 332 | Jacareí                  | BRA         | São Paulo           |     170356 |
| 333 | Araçatuba                | BRA         | São Paulo           |     169303 |
| 334 | Barra Mansa              | BRA         | Rio de Janeiro      |     168953 |
| 335 | Praia Grande             | BRA         | São Paulo           |     168434 |
| 336 | Marabá                   | BRA         | Pará                |     167795 |
| 337 | Criciúma                 | BRA         | Santa Catarina      |     167661 |
| 338 | Boa Vista                | BRA         | Roraima             |     167185 |
| 339 | Passo Fundo              | BRA         | Rio Grande do Sul   |     166343 |
| 340 | Dourados                 | BRA         | Mato Grosso do Sul  |     164716 |
| 341 | Santa Luzia              | BRA         | Minas Gerais        |     164704 |
| 342 | Rio Claro                | BRA         | São Paulo           |     163551 |
| 343 | Maracanaú                | BRA         | Ceará               |     162022 |
| 344 | Guarapuava               | BRA         | Paraná              |     160510 |
| 345 | Rondonópolis             | BRA         | Mato Grosso         |     155115 |
| 346 | São José                 | BRA         | Santa Catarina      |     155105 |
| 347 | Cachoeiro de Itapemirim  | BRA         | Espírito Santo      |     155024 |
| 348 | Nilópolis                | BRA         | Rio de Janeiro      |     153383 |
| 349 | Itapevi                  | BRA         | São Paulo           |     150664 |
| 350 | Cabo de Santo Agostinho  | BRA         | Pernambuco          |     149964 |
| 351 | Camaçari                 | BRA         | Bahia               |     149146 |
| 352 | Sobral                   | BRA         | Ceará               |     146005 |
| 353 | Itajaí                   | BRA         | Santa Catarina      |     145197 |
| 354 | Chapecó                  | BRA         | Santa Catarina      |     144158 |
| 355 | Cotia                    | BRA         | São Paulo           |     140042 |
| 356 | Lages                    | BRA         | Santa Catarina      |     139570 |
| 357 | Ferraz de Vasconcelos    | BRA         | São Paulo           |     139283 |
| 358 | Indaiatuba               | BRA         | São Paulo           |     135968 |
| 359 | Hortolândia              | BRA         | São Paulo           |     135755 |
| 360 | Caxias                   | BRA         | Maranhão            |     133980 |
| 361 | São Caetano do Sul       | BRA         | São Paulo           |     133321 |
| 362 | Itu                      | BRA         | São Paulo           |     132736 |
| 363 | Nossa Senhora do Socorro | BRA         | Sergipe             |     131351 |
| 364 | Parnaíba                 | BRA         | Piauí               |     129756 |
| 365 | Poços de Caldas          | BRA         | Minas Gerais        |     129683 |
| 366 | Teresópolis              | BRA         | Rio de Janeiro      |     128079 |
| 367 | Barreiras                | BRA         | Bahia               |     127801 |
| 368 | Castanhal                | BRA         | Pará                |     127634 |
| 369 | Alagoinhas               | BRA         | Bahia               |     126820 |
| 370 | Itapecerica da Serra     | BRA         | São Paulo           |     126672 |
| 371 | Uruguaiana               | BRA         | Rio Grande do Sul   |     126305 |
| 372 | Paranaguá                | BRA         | Paraná              |     126076 |
| 373 | Ibirité                  | BRA         | Minas Gerais        |     125982 |
| 374 | Timon                    | BRA         | Maranhão            |     125812 |
| 375 | Luziânia                 | BRA         | Goiás               |     125597 |
| 376 | Macaé                    | BRA         | Rio de Janeiro      |     125597 |
| 377 | Teófilo Otoni            | BRA         | Minas Gerais        |     124489 |
| 378 | Moji-Guaçu               | BRA         | São Paulo           |     123782 |
| 379 | Palmas                   | BRA         | Tocantins           |     121919 |
| 380 | Pindamonhangaba          | BRA         | São Paulo           |     121904 |
| 381 | Francisco Morato         | BRA         | São Paulo           |     121197 |
| 382 | Bagé                     | BRA         | Rio Grande do Sul   |     120793 |
| 383 | Sapucaia do Sul          | BRA         | Rio Grande do Sul   |     120217 |
| 384 | Cabo Frio                | BRA         | Rio de Janeiro      |     119503 |
| 385 | Itapetininga             | BRA         | São Paulo           |     119391 |
| 386 | Patos de Minas           | BRA         | Minas Gerais        |     119262 |
| 387 | Camaragibe               | BRA         | Pernambuco          |     118968 |
| 388 | Bragança Paulista        | BRA         | São Paulo           |     116929 |
| 389 | Queimados                | BRA         | Rio de Janeiro      |     115020 |
| 390 | Araguaína                | BRA         | Tocantins           |     114948 |
| 391 | Garanhuns                | BRA         | Pernambuco          |     114603 |
| 392 | Vitória de Santo Antão   | BRA         | Pernambuco          |     113595 |
| 393 | Santa Rita               | BRA         | Paraíba             |     113135 |
| 394 | Barbacena                | BRA         | Minas Gerais        |     113079 |
| 395 | Abaetetuba               | BRA         | Pará                |     111258 |
| 396 | Jaú                      | BRA         | São Paulo           |     109965 |
| 397 | Lauro de Freitas         | BRA         | Bahia               |     109236 |
| 398 | Franco da Rocha          | BRA         | São Paulo           |     108964 |
| 399 | Teixeira de Freitas      | BRA         | Bahia               |     108441 |
| 400 | Varginha                 | BRA         | Minas Gerais        |     108314 |
| 401 | Ribeirão Pires           | BRA         | São Paulo           |     108121 |
| 402 | Sabará                   | BRA         | Minas Gerais        |     107781 |
| 403 | Catanduva                | BRA         | São Paulo           |     107761 |
| 404 | Rio Verde                | BRA         | Goiás               |     107755 |
| 405 | Botucatu                 | BRA         | São Paulo           |     107663 |
| 406 | Colatina                 | BRA         | Espírito Santo      |     107354 |
| 407 | Santa Cruz do Sul        | BRA         | Rio Grande do Sul   |     106734 |
| 408 | Linhares                 | BRA         | Espírito Santo      |     106278 |
| 409 | Apucarana                | BRA         | Paraná              |     105114 |
| 410 | Barretos                 | BRA         | São Paulo           |     104156 |
| 411 | Guaratinguetá            | BRA         | São Paulo           |     103433 |
| 412 | Cachoeirinha             | BRA         | Rio Grande do Sul   |     103240 |
| 413 | Codó                     | BRA         | Maranhão            |     103153 |
| 414 | Jaraguá do Sul           | BRA         | Santa Catarina      |     102580 |
| 415 | Cubatão                  | BRA         | São Paulo           |     102372 |
| 416 | Itabira                  | BRA         | Minas Gerais        |     102217 |
| 417 | Itaituba                 | BRA         | Pará                |     101320 |
| 418 | Araras                   | BRA         | São Paulo           |     101046 |
| 419 | Resende                  | BRA         | Rio de Janeiro      |     100627 |
| 420 | Atibaia                  | BRA         | São Paulo           |     100356 |
| 421 | Pouso Alegre             | BRA         | Minas Gerais        |     100028 |
| 422 | Toledo                   | BRA         | Paraná              |      99387 |
| 423 | Crato                    | BRA         | Ceará               |      98965 |
| 424 | Passos                   | BRA         | Minas Gerais        |      98570 |
| 425 | Araguari                 | BRA         | Minas Gerais        |      98399 |
| 426 | São José de Ribamar      | BRA         | Maranhão            |      98318 |
| 427 | Pinhais                  | BRA         | Paraná              |      98198 |
| 428 | Sertãozinho              | BRA         | São Paulo           |      98140 |
| 429 | Conselheiro Lafaiete     | BRA         | Minas Gerais        |      97507 |
| 430 | Paulo Afonso             | BRA         | Bahia               |      97291 |
| 431 | Angra dos Reis           | BRA         | Rio de Janeiro      |      96864 |
| 432 | Eunápolis                | BRA         | Bahia               |      96610 |
| 433 | Salto                    | BRA         | São Paulo           |      96348 |
| 434 | Ourinhos                 | BRA         | São Paulo           |      96291 |
| 435 | Parnamirim               | BRA         | Rio Grande do Norte |      96210 |
| 436 | Jacobina                 | BRA         | Bahia               |      96131 |
| 437 | Coronel Fabriciano       | BRA         | Minas Gerais        |      95933 |
| 438 | Birigui                  | BRA         | São Paulo           |      94685 |
| 439 | Tatuí                    | BRA         | São Paulo           |      93897 |
| 440 | Ji-Paraná                | BRA         | Rondônia            |      93346 |
| 441 | Bacabal                  | BRA         | Maranhão            |      93121 |
| 442 | Cametá                   | BRA         | Pará                |      92779 |
| 443 | Guaíba                   | BRA         | Rio Grande do Sul   |      92224 |
| 444 | São Lourenço da Mata     | BRA         | Pernambuco          |      91999 |
| 445 | Santana do Livramento    | BRA         | Rio Grande do Sul   |      91779 |
| 446 | Votorantim               | BRA         | São Paulo           |      91777 |
| 447 | Campo Largo              | BRA         | Paraná              |      91203 |
| 448 | Patos                    | BRA         | Paraíba             |      90519 |
| 449 | Ituiutaba                | BRA         | Minas Gerais        |      90507 |
| 450 | Corumbá                  | BRA         | Mato Grosso do Sul  |      90111 |
| 451 | Palhoça                  | BRA         | Santa Catarina      |      89465 |
| 452 | Barra do Piraí           | BRA         | Rio de Janeiro      |      89388 |
| 453 | Bento Gonçalves          | BRA         | Rio Grande do Sul   |      89254 |
| 454 | Poá                      | BRA         | São Paulo           |      89236 |
| 455 | Águas Lindas de Goiás    | BRA         | Goiás               |      89200 |
+-----+--------------------------+-------------+---------------------+------------+
250 rows in set (0.01 sec)

mysql> SELECT langauge
    -> FROM countrylanguage 
    -> WHERE countrycode = 'FRA' OR 'ESP';
ERROR 1054 (42S22): Unknown column 'langauge' in 'field list'
mysql> SELECT countrycode, language
    -> FROM countrylanguage
    -> WHERE countrycode = 'FRA' OR "ESP';
    "> 
    "> ^C

^C
mysql> SELECT countrycode, language
    -> FROM countrylanguage
    -> WHERE countrycode = 'FRA' OR countrycode = 'ESP';
+-------------+------------+
| countrycode | language   |
+-------------+------------+
| ESP         | Basque     |
| ESP         | Catalan    |
| ESP         | Galecian   |
| ESP         | Spanish    |
| FRA         | Arabic     |
| FRA         | French     |
| FRA         | Italian    |
| FRA         | Portuguese |
| FRA         | Spanish    |
| FRA         | Turkish    |
+-------------+------------+
10 rows in set (0.01 sec)

mysql> SELECT name, countrycode, polpulation
    -> FROM ^C

^C
mysql> select code, name, headOfState from country order by name LIMIT 10;
+------+---------------------+--------------------------+
| code | name                | headOfState              |
+------+---------------------+--------------------------+
| AFG  | Afghanistan         | Mohammad Omar            |
| ALB  | Albania             | Rexhep Mejdani           |
| DZA  | Algeria             | Abdelaziz Bouteflika     |
| ASM  | American Samoa      | George W. Bush           |
| AND  | Andorra             |                          |
| AGO  | Angola              | José Eduardo dos Santos  |
| AIA  | Anguilla            | Elisabeth II             |
| ATA  | Antarctica          |                          |
| ATG  | Antigua and Barbuda | Elisabeth II             |
| ARG  | Argentina           | Fernando de la Rúa       |
+------+---------------------+--------------------------+
10 rows in set (0.00 sec)

