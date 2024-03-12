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

