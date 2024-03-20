# PostgreSQL Guide

- [PostgreSQL Guide](#postgresql-guide)
  - [Introduction](#introduction)
    - [SQL Shell (psql)](#sql-shell-psql)
    - [pgAdmin 4](#pgadmin-4)
  - [SQL Syntax](#sql-syntax)
    - [Create Table](#create-table)
    - [Insert Data](#insert-data)
      - [INSERT INTO](#insert-into)
      - [Insert Multiple Rows](#insert-multiple-rows)
    - [Select Data](#select-data)
    - [ADD COLUMN](#add-column)
    - [UPDATE](#update)
    - [ALTER COLUMN](#alter-column)
    - [DROP COLUMN](#drop-column)
    - [DELETE](#delete)
    - [DROP TABLE](#drop-table)
  - [Demo Database](#demo-database)
  - [PostgreSQL Syntax](#postgresql-syntax)
    - [Operators](#operators)
      - [BETWEEN](#between)
      - [LIKE](#like)
      - [ILIKE](#ilike)
      - [IN](#in)
      - [IS NULL](#is-null)
      - [AND](#and)
      - [OR](#or)
      - [NOT](#not)
    - [SELECT](#select)
    - [SELECT DISTINCT](#select-distinct)
    - [WHERE](#where)
    - [ORDER BY](#order-by)

## Introduction

PostgreSQL คือ database ที่รองรับ ทั้ง database ที่เป็น SQL และ NoSQL

ในการเชื่อมต่อกับ database เราสามารถทำได้ 2 ทางคือ

1. SQL Shell (psql)
2. pgAdmin 4

### SQL Shell (psql)

ในการเข้าโดยวิธี CLI ให้เราใส่คำสั่ง `psql` แล้วเราจะเห็น command เป็นลักษณะแบบนี้ `postgres=#` หรือ `[user]=#` คือ เราสามารถเชื่อมต่อและเข้ามาภายใน database ได้แล้ว

เราสามารถตรวจสอบ version ของ db ไดัโดย

```bash
SELECT version();
```
ข้อควรจำ คือ ทุกๆ statment ต้องจบด้วย `;` เสมอ

### pgAdmin 4

การเข้าโดย pgAdmin 4 ให้ทำตามขั้นตอนนี้

1. เมื่อ login มาแล้ว ให้กด Servers > PostgreSQL 15 (ถ้าโดน popup ให้ใส่ password ก่อน)
2. กด Databases > คลิกขวาที่ postgres > กด Query Tool
3. จะมีหน้าต่าง Query Tool ขึ้นมา สามารถใส่คำสั่งได้โดยตรง
4. ลองใส่ `SELECT version();` ก็จะเห็น version ได้เหมือนกัน

## SQL Syntax

### Create Table

ในการสร้าง table ให้ใช้คำสั่ง

```sql
CREATE TABLE cars (
  brand VARCHAR(255),
  model VARCHAR(255),
  year INT
);
```

ในตัวอย่างนี้ เราสร้าง table เปล่า ชื่อ `cars` มี column คือ `brand`, `model`, `year` โดยทั้งหมดเป็น data type ของ `VARCHAR` และ `INT`

ผลลัพธ์ที่ได้จะเป็น

```bash
CREATE TABLE
```
ให้แสดงตารางที่สร้างขึ้นมาด้วยคำสั่ง

```sql
SELECT * FROM cars;
```

จะได้ผลลัพธ์เป็น

```bash
 brand | model | year
-------+-------+------
(0 rows)
```

### Insert Data

คือ การเพิ่มข้อมูลเข้าไปใน table สามารถทำได้ 2 วิธีคือ

#### INSERT INTO
ในการเพิ่มข้อมูลให้ใช้ statement `INSERT INTO` และใส่ข้อมูลที่ต้องการเพิ่มในรูปแบบของ `VALUES` 

ลองเพิ่ม row เข้าไปใน table `cars` ด้วยคำสั่ง

```sql
INSERT INTO cars (brand, model, year)
VALUES ('Toyota', 'Corolla', 1999);
```

จะได้ผลลัพธ์เป็น

```bash
INSERT 0 1
```

เลข `1` คือ จำนวน row ที่ถูกเพิ่มเข้าไปใน table `cars`
เลข `0` คือ จำนวน row ที่ถูกเปลี่ยนแปลง

ให้แสดงข้อมูลทั้งหมดที่อยู่ใน table `cars` ด้วยคำสั่ง

```sql
SELECT * FROM cars;
```

จะได้ผลลัพธ์เป็น

```bash
 brand  | model  | year
--------+--------+------
Toyota | Corolla| 1999
(1 row)
```

ในตัวอย่างนี้ เราได้เพิ่มข้อมูลเข้าไปใน table `cars` แล้ว โดยมีข้อมูลคือ `Toyota`, `Corolla`, `1999`

#### Insert Multiple Rows

ในการเพิ่มข้อมูลหลายๆ row ให้ใช้ statement `INSERT INTO`, `VALUES` เหมือนกัน และใส่ข้อมูลที่ต้องการเพิ่มในรูปแบบของ `()` และ `,` เพื่อแบ่งแต่ละ row

ลองเพิ่ม row เข้าไปใน table `cars` ด้วยคำสั่ง

```sql
INSERT INTO cars (brand, model, year)
VALUES ('Toyota', 'Corolla', 1999),
       ('Toyota', 'Camry', 2000),
       ('Honda', 'Civic', 2001);
```

จะได้ผลลัพธ์เป็น

```bash
INSERT 0 3
```

### Select Data

ในการดึงข้อมูลออกมาจาก table ให้ใช้คำสั่ง `SELECT` โดยใส่ column ที่ต้องการดึงข้อมูลออกมาในรูปแบบของ `*` หรือ `column_name` และใส่ `FROM ` ตามด้วย table ที่ต้องการดึงข้อมูลออกมา เช่น
  
```sql
SELECT * FROM cars;
```

จะได้ผลลัพธ์เป็น

```bash
 brand  | model  | year
--------+--------+------
Toyota | Corolla| 1999
Toyota | Camry  | 2000
Honda  | Civic  | 2001
(3 rows)
```

หรือถ้าต้องการดึงข้อมูลบาง column ออกมาเท่านั้น ให้ใส่ชื่อ column ที่ต้องการดึงข้อมูลออกมา

```sql
SELECT brand, model FROM cars;
```

จะได้ผลลัพธ์เป็น

```bash
 brand  | model
--------+--------
Toyota | Corolla
Toyota | Camry
Honda  | Civic
(3 rows)
```

### ADD COLUMN

`ALTER TABLE` คือ คำสั่งที่ใช้ในการเพิ่ม, ลบ, เปลี่ยน column ใน table ที่สร้างอยู่แล้วใน database

ในการเพิ่ม column ให้ใช้คำสั่ง `ALTER TABLE` และใส่ชื่อ column ที่ต้องการเพิ่ม และ data type ของ column นั้นๆ

```sql
ALTER TABLE cars
ADD COLUMN price INT;
```

จะได้ผลลัพธ์เป็น

```bash
ALTER TABLE
```

แปลว่า column `price` ถูกเพิ่มเข้าไปใน table `cars` แล้ว

ลองดูข้อมูลทั้งหมดที่อยู่ใน table `cars` ด้วยคำสั่ง `SELECT * FROM cars;` จะได้ผลลัพธ์เป็น

```bash
 brand  | model  | year | price
--------+--------+------+------
Toyota | Corolla| 1999 |
Toyota | Camry  | 2000 |
Honda  | Civic  | 2001 |
(3 rows)
```

### UPDATE

`UPDATE` คือ คำสั่งที่ใช้ในการเปลี่ยนแปลงข้อมูลใน table โดยใช้คำสั่ง `SET` และใส่ข้อมูลที่ต้องการเปลี่ยนแปลง และใส่ `WHERE` ตามด้วยเงื่อนไขที่ต้องการเปลี่ยนแปลง

```sql
UPDATE cars
SET price = 300000
WHERE year = 1999;
```

จะได้ผลลัพธ์เป็น

```bash
UPDATE 1
```

แปลว่า มี row ที่ถูกเปลี่ยนแปลงเป็น `1` แล้ว

อย่าลืมใส่ `WHERE` เพื่อกำหนดเงื่อนไขที่ต้องการเปลี่ยนแปลง ถ้าไม่ใส่ `WHERE` ข้อมูลทั้งหมดใน column นั้นๆ จะถูกเปลี่ยนแปลง

ลองดูข้อมูลทั้งหมดที่อยู่ใน table `cars` ด้วยคำสั่ง `SELECT * FROM cars;` จะได้ผลลัพธ์เป็น

```bash
 brand  | model  | year | price
--------+--------+------+------
Toyota | Corolla| 1999 | 300000
Toyota | Camry  | 2000 |
Honda  | Civic  | 2001 |
(3 rows)
```

สามารถอัพเดทได้หลายๆ column พร้อมๆ กัน โดยใช้ `,` เพื่อแบ่งแต่ละ column

```sql
UPDATE cars
SET price = 300000, model = 'Corolla 2'
WHERE year = 1999;
```

จะได้ผลลัพธ์เป็น

```bash
UPDATE 1
```

ลองดูข้อมูลทั้งหมดที่อยู่ใน table `cars` ด้วยคำสั่ง `SELECT * FROM cars;` จะได้ผลลัพธ์เป็น

```bash
 brand  | model    | year | price
--------+----------+------+------
Toyota | Corolla 2| 1999 | 300000
Toyota | Camry    | 2000 |
Honda  | Civic    | 2001 |
(3 rows)
```

### ALTER COLUMN

`ALTER COLUMN` คือ คำสั่งที่ใช้เพื่อ
- เปลี่ยน data type ของ column
- เปลี่ยน size ของ column
จะใช้คำสั่ง `ALTER TABLE` และใส่ชื่อ column ที่ต้องการเปลี่ยนแปลง ตามด้วย `TYPE` และใส่ data type ของ column นั้นๆ

ตัวอย่างการเปลี่ยน data type ของ column `price` จาก `INT` เป็น `VARCHAR`

```sql
ALTER TABLE cars
ALTER COLUMN price TYPE VARCHAR(255);
```

จะได้ผลลัพธ์เป็น

```bash
ALTER TABLE
```

แต่บาง data type ไม่สามารถเปลี่ยนแปลงได้ ถ้าเปลี่ยนแปลงไม่ได้ จะได้ผลลัพธ์เป็น

```bash
ERROR:  column "price" cannot be cast automatically to type character varying
HINT:  You might need to specify "USING price::character varying".
```

ตัวอย่างการเปลี่ยน size ของ column `price` จาก `VARCHAR(255)` เป็น `VARCHAR(100)`

```sql
ALTER TABLE cars
ALTER COLUMN price TYPE VARCHAR(100);
```

จะได้ผลลัพธ์เป็น

```bash
ALTER TABLE
```

### DROP COLUMN

`DROP COLUMN` คือ คำสั่งที่ใช้เพื่อลบ column ออกจาก table ใน database โดยใช้คำสั่ง `ALTER TABLE` และใส่ชื่อ column ที่ต้องการลบ

```sql
ALTER TABLE cars
DROP COLUMN price;
```

จะได้ผลลัพธ์เป็น

```bash
ALTER TABLE
```

แปลว่า column `price` ถูกลบออกจาก table `cars` แล้ว

ลองดูข้อมูลทั้งหมดที่อยู่ใน table `cars` ด้วยคำสั่ง `SELECT * FROM cars;` จะได้ผลลัพธ์เป็น

```bash
 brand  | model  | year
--------+--------+------
Toyota | Corolla| 1999
Toyota | Camry  | 2000
Honda  | Civic  | 2001
(3 rows)
```

### DELETE

`DELETE` คือ คำสั่งที่ใช้เพื่อลบข้อมูลออกจาก table ใน database โดยใช้คำสั่ง `DELETE FROM` และใส่ `WHERE` ตามด้วยเงื่อนไขที่ต้องการลบ

```sql
DELETE FROM cars
WHERE year = 1999;
```

จะได้ผลลัพธ์เป็น

```bash
DELETE 1
```

แปลว่า มี row ที่ถูกลบออกจาก table `cars` แล้ว

ลองดูข้อมูลทั้งหมดที่อยู่ใน table `cars` ด้วยคำสั่ง `SELECT * FROM cars;` จะได้ผลลัพธ์เป็น

```bash
 brand  | model  | year
--------+--------+------
Toyota | Camry  | 2000
Honda  | Civic  | 2001
(2 rows)
```

ถ้าไม่ใส่ `WHERE` ข้อมูลทั้งหมดใน table นั้นๆ จะถูกลบ

```sql
DELETE FROM cars;
```

จะได้ผลลัพธ์เป็น

```bash
DELETE 2
```

ลองดูข้อมูลทั้งหมดที่อยู่ใน table `cars` ด้วยคำสั่ง `SELECT * FROM cars;` จะได้ผลลัพธ์เป็น

```bash
 brand  | model  | year
--------+--------+------
(0 rows)
```

เราสามารถ truncate ข้อมูลทั้งหมดใน table ได้ด้วยคำสั่ง `TRUNCATE`

truncate คือ คำสั่งที่ใช้เพื่อลบข้อมูลทั้งหมดใน table โดยใช้คำสั่ง `TRUNCATE` ตามด้วยชื่อ table ที่ต้องการลบ

หลังจากลบข้อมูลทั้งหมดใน table แล้ว ข้อมูลทั้งหมดใน table จะหายไป และไม่สามารถกู้คืนได้

```sql
TRUNCATE cars;
```

จะได้ผลลัพธ์เป็น

```bash
TRUNCATE TABLE
```

ลองดูข้อมูลทั้งหมดที่อยู่ใน table `cars` ด้วยคำสั่ง `SELECT * FROM cars;` จะได้ผลลัพธ์เป็น

```bash
 brand  | model  | year
--------+--------+------
(0 rows)
```

### DROP TABLE

`DROP TABLE` คือ คำสั่งที่ใช้เพื่อลบ table ออกจาก database โดยใช้คำสั่ง `DROP TABLE` ตามด้วยชื่อ table ที่ต้องการลบ

หลังจากลบ table แล้ว ข้อมูลทั้งหมดใน table จะหายไป และไม่สามารถกู้คืนได้

```sql
DROP TABLE cars;
```

จะได้ผลลัพธ์เป็น

```bash
DROP TABLE
```

ลองดูข้อมูลทั้งหมดที่อยู่ใน table `cars` ด้วยคำสั่ง `SELECT * FROM cars;` จะได้ผลลัพธ์เป็น

```bash
ERROR:  relation "cars" does not exist
LINE 1: SELECT * FROM cars;
                      ^
```

## Demo Database

เตรียมข้อมูลก่อนเริ่ม PostgreSQL Syntax ได้ที่นี่ [Demo Database](https://www.w3schools.com/postgresql/postgresql_create_demodatabase.php)

## PostgreSQL Syntax

เมื่อเตรียมข้อมูลเรียบร้อยแล้ว สามารถเริ่มบทเรียน PostgreSQL Syntax

### Operators

`WHERE` clause ใช้เพื่อกรองข้อมูล โดยใช้ operator ต่างๆ เช่น
- `=` เท่ากับ
- `>` มากกว่า
- `<` น้อยกว่า
- `>=` มากกว่าหรือเท่ากับ
- `<=` น้อยกว่าหรือเท่ากับ
- `<>` ไม่เท่ากับ
- `!==` ไม่เท่ากับ
- `BETWEEN` อยู่ระหว่าง
- `LIKE` คล้ายกับ (case sensitive)
- `ILIKE` คล้ายกับ (case-insensitive)
- `IN` อยู่ใน
- `IS NULL` เป็นค่า `NULL`
- `AND` และ
- `OR` หรือ
- `NOT` ไม่ใช่

ตัวอย่างการใช้ `WHERE` clause ในการกรองข้อมูล

```sql
SELECT * FROM cars
WHERE brand = 'Toyota';
```

ที่ผ่านมาจะเห็นตัวอย่างการใช้ `=`, `>`, `<`, `>=`, `<=`, `<>`, `!==` ในการกรองข้อมูลหรือคุ้นเคยมาบ้างแล้ว ในที่นี้จะขอยกตัวอย่างการใช้ `BETWEEN`, `LIKE`, `IN`, `IS NULL`, `AND`, `OR`, `NOT` ในการกรองข้อมูล

#### BETWEEN

`BETWEEN` ใช้เพื่อกรองข้อมูลที่อยู่ระหว่าง 2 ค่า

ตัวอย่างการใช้ `BETWEEN` ในการกรองข้อมูล

```sql
SELECT * FROM cars
WHERE year BETWEEN 1970 AND 1980;
```

จะได้ข้อมูลที่มี `year` อยู่ระหว่าง 1970 ถึง 1980
  
```bash
  brand  | model  | year
 --------+--------+------
  Toyota | Corolla| 1975
  Toyota | Camry  | 1976
  Honda  | Civic  | 1977
  (3 rows)
```

#### LIKE

`LIKE` ใช้เพื่อกรองข้อมูลที่คล้ายกับ คำที่กำหนด (case-sensitive)

ตัวอย่างการใช้ `LIKE` ในการกรองข้อมูล

```sql
SELECT * FROM cars
WHERE model ILIKE 'C%';
```

จะได้ข้อมูลที่มี `model` ขึ้นต้นด้วย `C`

```bash
  brand  | model  | year
 --------+--------+------
  Toyota | Corolla| 1975
  Toyota | Camry  | 1976
  Honda  | Civic  | 1977
  (3 rows)
```

#### ILIKE

`ILIKE` ใช้เพื่อกรองข้อมูลที่คล้ายกับ คำที่กำหนด (case-insensitive)

ตัวอย่างการใช้ `ILIKE` ในการกรองข้อมูล

```sql
SELECT * FROM cars
WHERE model ILIKE 'c%';
```

จะได้ข้อมูลที่มี `model` ขึ้นต้นด้วย `c` โดยไม่สนใจตัวพิมพ์เล็กหรือตัวพิมพ์ใหญ่

```bash
  brand  | model  | year
 --------+--------+------
  Toyota | Corolla| 1975
  Toyota | Camry  | 1976
  Honda  | Civic  | 1977
  (3 rows)
```

#### IN

`IN` ใช้เพื่อกรองข้อมูลที่อยู่ใน list ที่กำหนด

ตัวอย่างการใช้ `IN` ในการกรองข้อมูล

```sql
SELECT * FROM cars
WHERE year IN (1975, 1976);
```

จะได้ข้อมูลที่มี `year` อยู่ใน list ที่กำหนด

```bash
  brand  | model  | year
 --------+--------+------
  Toyota | Corolla| 1975
  Toyota | Camry  | 1976
  (2 rows)
```

#### IS NULL

`IS NULL` ใช้เพื่อกรองข้อมูลที่เป็นค่า `NULL`

ตัวอย่างการใช้ `IS NULL` ในการกรองข้อมูล

```sql
SELECT * FROM cars
WHERE year IS NULL;
```

จะได้ข้อมูลที่มี `year` เป็นค่า `NULL`

```bash
  brand  | model  | year
 --------+--------+------
  Toyota | Corolla| 
  Toyota | Camry  | 
  Honda  | Civic  | 
  (3 rows)
```

#### AND

`AND` ใช้เพื่อกรองข้อมูลที่เป็นจริงทั้ง 2 เงื่อนไข

ตัวอย่างการใช้ `AND` ในการกรองข้อมูล

```sql
SELECT * FROM cars
WHERE year > 1975 AND year < 1980;
```

จะได้ข้อมูลที่มี `year` มากกว่า 1975 และน้อยกว่า 1980

```bash
  brand  | model  | year
 --------+--------+------
  Toyota | Camry  | 1976
  Honda  | Civic  | 1977
  (2 rows)
```

#### OR

`OR` ใช้เพื่อกรองข้อมูลที่เป็นจริงอย่างน้อย 1 เงื่อนไข

ตัวอย่างการใช้ `OR` ในการกรองข้อมูล

```sql
SELECT * FROM cars
WHERE year < 1975 OR year > 1980;
```

จะได้ข้อมูลที่มี `year` น้อยกว่า 1975 หรือมากกว่า 1980

```bash
  brand  | model  | year
 --------+--------+------
  Toyota | Corolla| 1975
  Toyota | Camry  | 1976
  Honda  | Civic  | 1977
  (3 rows)
```

#### NOT

`NOT` ใช้เพื่อกรองข้อมูลที่เป็นเท็จ

ตัวอย่างการใช้ `NOT` ในการกรองข้อมูล

```sql
SELECT * FROM cars
WHERE NOT year = 1975;
```

จะได้ข้อมูลที่มี `year` ไม่เท่ากับ 1975

เราสามารถใช้ `NOT` ร่วมกับ Operator อื่นๆ ได้ เช่น

ใช้ร่วมกับ `IS NULL`

```sql
SELECT * FROM cars
WHERE year IS NOT NULL;
```

จะได้ข้อมูลที่มี `year` ไม่เป็นค่า `NULL`

ใช้ร่วมกับ `AND`

```sql
SELECT * FROM cars
WHERE year > 1975 AND NOT year = 1977;
```

จะได้ข้อมูลที่มี `year` มากกว่า 1975 และไม่เท่ากับ 1977

ใช้ร่วมกับ `OR`

```sql
SELECT * FROM cars
WHERE year < 1975 OR NOT year = 1977;
```

จะได้ข้อมูลที่มี `year` น้อยกว่า 1975 หรือไม่เท่ากับ 1977

ใช้ร่วมกับ `LIKE`

```sql
SELECT * FROM cars
WHERE model NOT LIKE 'C%';
```

จะได้ข้อมูลที่มี `model` ไม่ขึ้นต้นด้วย `C` (case-insensitive)

### SELECT

`SELECT` คือ คำสั่งที่ใช้เพื่อดึงข้อมูลออกมาจาก table ใน database

ตัวอย่างการใช้ `SELECT`

```sql
SELECT customer_name, country FROM customers;
```

จะได้ข้อมูลทั้งหมดที่อยู่ใน column `customer_name`, `country` ของ table `customers`

```bash
            customer_name             |   country
--------------------------------------+-------------
 Alfreds Futterkiste                  | Germany
 Ana Trujillo Emparedados y helados   | Mexico
 Antonio Moreno Taquera               | Mexico
 ...
 Wolski                               | Poland
  (91 rows)
```

เราสามารถใช้ `*` ในการดึงข้อมูลทั้งหมดออกมาจาก table ใน database

```sql
SELECT * FROM customers;
```

จะได้ข้อมูลทั้งหมดที่อยู่ใน table `customers`

```bash
 customer_id |            customer_name             |     contact_name     |                    address                     |      city       | postal_code |   country
-------------+--------------------------------------+----------------------+------------------------------------------------+-----------------+-------------+-------------
           1 | Alfreds Futterkiste                  | Maria Anders         | Obere Str. 57                                  | Berlin          | 12209       | Germany
           2 | Ana Trujillo Emparedados y helados   | Ana Trujillo         | Avda. de la Constitucion 2222                  | Mexico D.F.     | 05021       | Mexico
           3 | Antonio Moreno Taquera               | Antonio Moreno       | Mataderos 2312                                 | Mexico D.F.     | 05023       | Mexico
           ...
          91 | Wolski                               | Zbyszek              | ul. Filtrowa 68                                | Walla           | 01-012      | Poland
(91 rows)
```

### SELECT DISTINCT

`SELECT DISTINCT` คือ คำสั่งที่ใช้เพื่อดึงข้อมูลที่ไม่ซ้ำกันออกมาจาก table ใน database

ตัวอย่างการใช้ `SELECT DISTINCT`

```sql
SELECT DISTINCT country FROM customers;
```

จะได้ข้อมูลที่ไม่ซ้ำกันทั้งหมดที่อยู่ใน column `country` ของ table `customers`

เราสามารถใช้ร่วมกับ `COUNT` เพื่อนับจำนวนข้อมูลที่ไม่ซ้ำกันทั้งหมดที่อยู่ใน column `country` ของ table `customers`

```sql
SELECT COUNT(DISTINCT country) FROM customers;
```

จะได้จำนวนข้อมูลที่ไม่ซ้ำกันทั้งหมดที่อยู่ใน column `country` ของ table `customers`

```bash
 count
-------
    21
(1 row)
```

### WHERE

`WHERE` clause ใช้เพื่อกรองข้อมูล โดยใช้ operator ต่างๆ

### ORDER BY

`ORDER BY` clause ใช้เพื่อเรียงลำดับข้อมูล โดยใช้ column ที่ต้องการเรียงลำดับ ตามด้วย `ASC` หรือ `DESC` ถ้าไม่ใส่ ข้อมูลจะถูกเรียงลำดับเป็น `ASC` เป็นค่า default

ตัวอย่างการใช้ `ORDER BY`

```sql
SELECT * FROM products
ORDER BY price;
```

จะได้ข้อมูลทั้งหมดที่อยู่ใน table `products` ที่ถูกเรียงลำดับตาม column `price` จากน้อยไปมาก

```bash
 product_id |           product_name           | category_id |         unit         | price
------------+----------------------------------+-------------+----------------------+--------
         33 | Geitost                          |           4 | 500 g                |   2.50
         24 | Guarani Fantastica               |           1 | 12 - 355 ml cans     |   4.50
         13 | Konbu                            |           8 | 2 kg box             |   6.00
         ...
         29 | Thoringer Rostbratwurst          |           6 | 50 bags x 30 sausgs. | 123.79
         38 | Cote de Blaye                    |           1 | 12 - 75 cl bottles   | 263.50
(77 rows)
```

เราสามารถใช้ร่วมกับ `DESC` เพื่อเรียงลำดับข้อมูลจากมากไปน้อย

```sql
SELECT * FROM products
ORDER BY price DESC;
```

จะได้ข้อมูลทั้งหมดที่อยู่ใน table `products` ที่ถูกเรียงลำดับตาม column `price` จากมากไปน้อย

```bash
 product_id |           product_name           | category_id |         unit         | price
------------+----------------------------------+-------------+----------------------+--------
         38 | Cote de Blaye                    |           1 | 12 - 75 cl bottles   | 263.50
         29 | Thoringer Rostbratwurst          |           6 | 50 bags x 30 sausgs. | 123.79
         ...
         13 | Konbu                            |           8 | 2 kg box             |   6.00
         24 | Guarani Fantastica               |           1 | 12 - 355 ml cans     |   4.50
         33 | Geitost                          |           4 | 500 g                |   2.50
(77 rows)
```
