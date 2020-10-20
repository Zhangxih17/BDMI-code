### 1. Set operations
- nested queries 

### 2. Aggregation 
- operations
- GROUP BY


```python
%load_ext sql
%sql sqlite://
```

    The sql extension is already loaded. To reload it, use:
      %reload_ext sql
    


```sql
%%sql
pragma foreign_keys = ON; -- WARNING: by default off in sqlite
drop table if exists product; -- This needs to be dropped if exists, see why further down!
drop table if exists company;
create table company (
    cname varchar primary key, -- company name uniquely identifies the company.
    stockprice money, -- stock price is in money 
    country varchar); -- country is just a string
insert into company values ('GizmoWorks', 25.0, 'USA');
insert into company values ('Canon', 65.0, 'Japan');
insert into company values ('Hitachi', 15.0, 'Japan');
create table product(
       pname varchar primary key, -- name of the product
       price money, -- price of the product
       category varchar, -- category
       manufacturer varchar, -- manufacturer
       foreign key (manufacturer) references company(cname));
insert into product values('Gizmo', 19.99, 'Gadgets', 'GizmoWorks');
insert into product values('SingleTouch', 149.99, 'Photography', 'Canon');
insert into product values('PowerGizmo', 29.99, 'Gadgets', 'GizmoWorks');
insert into product values('MultiTouch', 203.99, 'Household', 'Hitachi');
```

     * sqlite://
    Done.
    Done.
    Done.
    Done.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    Done.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    




    []




```sql
%%sql
DROP TABLE IF EXISTS franchise;
CREATE TABLE franchise (name TEXT, db_type TEXT);
INSERT INTO franchise VALUES ('Bobs Bagels', 'NoSQL');
INSERT INTO franchise VALUES ('eBagel', 'NoSQL');
INSERT INTO franchise VALUES ('BAGEL CORP', 'MySQL');

DROP TABLE IF EXISTS store;
CREATE TABLE store (franchise TEXT, location TEXT);
INSERT INTO store VALUES ('Bobs Bagels', 'NYC');
INSERT INTO store VALUES ('eBagel', 'PA');
INSERT INTO store VALUES ('BAGEL CORP', 'Chicago');
INSERT INTO store VALUES ('BAGEL CORP', 'NYC');
INSERT INTO store VALUES ('BAGEL CORP', 'PA');

DROP TABLE IF EXISTS bagel;
CREATE TABLE bagel (name TEXT, price MONEY, made_by TEXT);
INSERT INTO bagel VALUES ('Plain with shmear', 1.99, 'Bobs Bagels');
INSERT INTO bagel VALUES ('Egg with shmear', 2.39, 'Bobs Bagels');
INSERT INTO bagel VALUES ('eBagel Drinkable Bagel', 27.99, 'eBagel');
INSERT INTO bagel VALUES ('eBagel Expansion Pack', 1.99, 'eBagel');
INSERT INTO bagel VALUES ('Plain with shmear', 0.99, 'BAGEL CORP');
INSERT INTO bagel VALUES ('Organic Flax-seed bagel chips', 0.99, 'BAGEL CORP');

DROP TABLE IF EXISTS purchase;
-- Note that date is an int here just to simplify things
CREATE TABLE purchase (bagel_name TEXT, franchise TEXT, date INT, quantity INT, purchaser_age INT);
INSERT INTO purchase VALUES ('Plain with shmear', 'Bobs Bagels', 1, 12, 28);
INSERT INTO purchase VALUES ('Egg with shmear', 'Bobs Bagels', 2, 6, 47);
INSERT INTO purchase VALUES ('Plain with shmear', 'BAGEL CORP', 2, 12, 24);
INSERT INTO purchase VALUES ('Plain with shmear', 'BAGEL CORP', 3, 1, 17);
INSERT INTO purchase VALUES ('eBagel Expansion Pack', 'eBagel', 1, 137, 5);
INSERT INTO purchase VALUES ('Plain with shmear', 'Bobs Bagels', 4, 24, NULL);
```

     * sqlite://
    Done.
    Done.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    Done.
    Done.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    Done.
    Done.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    Done.
    Done.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    




    []




```python
%sql SELECT * FROM Product;
```

     * sqlite://
    Done.
    




<table>
    <tr>
        <th>pname</th>
        <th>price</th>
        <th>category</th>
        <th>manufacturer</th>
    </tr>
    <tr>
        <td>Gizmo</td>
        <td>19.99</td>
        <td>Gadgets</td>
        <td>GizmoWorks</td>
    </tr>
    <tr>
        <td>SingleTouch</td>
        <td>149.99</td>
        <td>Photography</td>
        <td>Canon</td>
    </tr>
    <tr>
        <td>PowerGizmo</td>
        <td>29.99</td>
        <td>Gadgets</td>
        <td>GizmoWorks</td>
    </tr>
    <tr>
        <td>MultiTouch</td>
        <td>203.99</td>
        <td>Household</td>
        <td>Hitachi</td>
    </tr>
</table>




```sql
%%sql SELECT pname,price FROM Product
ORDER BY pname
```

     * sqlite://
    Done.
    




<table>
    <tr>
        <th>pname</th>
        <th>price</th>
    </tr>
    <tr>
        <td>Gizmo</td>
        <td>19.99</td>
    </tr>
    <tr>
        <td>MultiTouch</td>
        <td>203.99</td>
    </tr>
    <tr>
        <td>PowerGizmo</td>
        <td>29.99</td>
    </tr>
    <tr>
        <td>SingleTouch</td>
        <td>149.99</td>
    </tr>
</table>



### 3.postgreSQL


```python
import psycopg2
```


```python
conn = psycopg2.connect(database="BDMI", user="postgres",
                        password="", host="127.0.0.1", port="5432")
```


```sql
%%sql drop table if exists product;
create table product(
       pname        varchar primary key, -- name of the product
       price        money,               -- price of the product
       category     varchar,             -- category
       manufacturer varchar NOT NULL     -- manufacturer
);
insert into product values('Gizmo', 19.99, 'Gadgets', 'GizmoWorks');
insert into product values('PowerGizmo', 29.99, 'Gadgets', 'GizmoWorks');
insert into product values('SingleTouch', 149.99, 'Photography', 'Canon');
insert into product values('MultiTouch', 203.99, 'Household', 'Hitachi');

select * from product;
```

     * sqlite://
    Done.
    Done.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    1 rows affected.
    Done.
    




<table>
    <tr>
        <th>pname</th>
        <th>price</th>
        <th>category</th>
        <th>manufacturer</th>
    </tr>
    <tr>
        <td>Gizmo</td>
        <td>19.99</td>
        <td>Gadgets</td>
        <td>GizmoWorks</td>
    </tr>
    <tr>
        <td>PowerGizmo</td>
        <td>29.99</td>
        <td>Gadgets</td>
        <td>GizmoWorks</td>
    </tr>
    <tr>
        <td>SingleTouch</td>
        <td>149.99</td>
        <td>Photography</td>
        <td>Canon</td>
    </tr>
    <tr>
        <td>MultiTouch</td>
        <td>203.99</td>
        <td>Household</td>
        <td>Hitachi</td>
    </tr>
</table>




```python

```


```python

```
