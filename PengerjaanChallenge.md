# Tokonyadia Challege
_[Yahya Faikar hanif]_
_Menggunakan navicat sebagai tool_

Pengerjaan => 

- Membuat database:
```sh
CREATE DATABASE tokonyadia;
```

- Melakukan connect atau masuk ke dalam database:
```sh
\c tokonyadia;
```
- Membuat table customer dan penambahan foreign key:
```sh
CREATE TABLE customers (
  customer_id int PRIMARY KEY,
  first_name varchar(15) COLLATE  NOT NULL,
  last_name varchar(15) 
  email varchar(25) ,
  phone_number int NOT NULL,
  domicile_id int NOT NULL);
```
```sh
ALTER TABLE customers (
  ADD CONSTRAINT fk_domicile
  FOREIGN KEY(domicile_id)
  REFERENCES customer(customer_id));
```

- Membuat table domisili dan penambahan foreign key: 
```sh
CREATE TABLE domicile (
  domicile_id int PRIMARY KEY,
  district varchar(20) NOT NULL,
  city varchar(20) NOT NULL);
```
```sh
ALTER TABLE domicile (
  ADD CONSTRAINT fk_customer
  FOREIGN KEY(customer_id)
  REFERENCES domicile(domicile_id));
```

- Membuat table toko dan penambahan foreign key: 
```sh
CREATE TABLE shops (
  shop_id int PRIMARY KEY,
  permit_number int4 NOT NULL,
  name varchar(30) ,
  phone_number int,
  address varchar(255) ,
  domicile_id int NOT NULL);
```
```sh
ALTER TABLE shops (
  ADD CONSTRAINT fk_domicile
  FOREIGN KEY(domicile_id)
  REFERENCES shop(shop_id));
```

- Membuat table produk dan penambahan foreign key: 
```sh
CREATE TABLE products (
  product_id int PRIMARY KEY,
  name varchar(30) ,
  price int,
  stock int,
  shop_id int NOT NULL,
  desc varchar(255));
```
```sh
ALTER TABLE products (
  ADD CONSTRAINT fk_shop
  FOREIGN KEY(shop_id)
  REFERENCES product(products_id));
```

- Membuat table transaksi dan penambahan foreign key: 
```sh
CREATE TABLE transaction (
  transaction_id int PRIMARY KEY,
  date date,
  customer_id int NOT NULL);
```
```sh
ALTER TABLE transaction (
  ADD CONSTRAINT fk_customer
  FOREIGN KEY(customer_id)
  REFERENCES transaction(transaction_id);
```

- Membuat table detail transaksi dan penambahan foreign key: 
```sh
CREATE TABLE transaction_details (
  detail_transaction_id int PRIMARY KEY,
  price int,
  quantity int,
  product_id int NOT NULL,
  transaction_id int NOT NULL);
```
```sh
ALTER TABLE transaction_details (
  ADD CONSTRAINT fk_product
  FOREIGN KEY(product_id)
  REFERENCES transaction_details(detail_transaction_id,
  CONSTRAINT fk_transaction
  FOREIGN KEY(transaction_id)
  REFERENCES transaction_details(detail_transaction_id);
```


## ADD VALUES
- Isi record pada tabel customers: 
```sh
INSERT INTO customers VALUES (1, 'Yahya', 'Faikar', 'yahyafaikar17@gmail.com', 081334031231, 1);
INSERT INTO customers VALUES (2, 'Emir', 'Ibrahim', 'ibrahimemir@yahoo.com', 081232127638, 2);
INSERT INTO customers VALUES (3, 'John', 'Cena', 'cencena@john.com', 08214423883, 4);
INSERT INTO customers VALUES (4, 'Kevin', 'Durant', 'durat@nba.com', 82144391129, 3);
INSERT INTO customers VALUES (5, 'Bruno', 'Fernandes', 'bruno@mu.co.uk', 088837237721, 2);
```

- Isi record pada tabel domisili: 
```sh
INSERT INTO domicile VALUES (1, 'Gayungan', 'Surabaya');
INSERT INTO domicile VALUES (2, 'Ciputat ', 'Tangerang Selatan');
INSERT INTO domicile VALUES (4, 'Trawas', 'Mojokerto');
INSERT INTO domicile VALUES (3, 'Gambir', 'Jakarta Pusat');
```

- Isi record pada tabel shops: 
```sh
INSERT INTO shops VALUES (1, 1234566, 'Tokopaedi', 82112121223, 'Jl. Buntu Gg 2', 2);
INSERT INTO shops VALUES (2, 1234568, 'Tokomura', 81121249537, 'Jl. Jalan No. 1', 1);
INSERT INTO shops VALUES (3, 1237654, 'Tesla Official Shop', 81243127432, 'Jl. Kenangan No. 99', 1);
INSERT INTO shops VALUES (4, 1234598, 'Eiger Official Store', 82143238721, 'Bumi Gambir No. 1B', 3);
```

- Isi record pada tabel products: 
```sh
INSERT INTO products VALUES (1, 'Macbook Pro 13 2020', 20000000, 10, 1, 'Laptop keluaran tahun 2020 warna hitam');
INSERT INTO products VALUES (4, 'Eiger Shoes Tarantula', 1000000, 5, 4, 'Outdoor shoes');
INSERT INTO products VALUES (5, 'Eiger Carrier Semeru', 1500000, 7, 4, 'Superb carrier for your outdoor activities');
INSERT INTO products VALUES (2, 'Kursi Lipat', 250000, 30, 2, 'Kursi lipat serbaguna');
INSERT INTO products VALUES (3, 'Tesla Model X - Charger', 35000000, 5, 2, 'Charger only for tesla model X');
INSERT INTO products VALUES (6, 'Meja Lipat', 300000, 21, 3, 'Meja lipat mantap');
```

- Isi record pada tabel transaction: 
```sh
INSERT INTO transaction VALUES (1, '2021-02-22', 1);
INSERT INTO transaction VALUES (2, '2020-12-22', 1);
INSERT INTO transaction VALUES (4, '2021-02-09', 3);
INSERT INTO transaction VALUES (5, '2020-12-12', 4);
INSERT INTO transaction VALUES (6, '2021-03-03', 5);
INSERT INTO transaction VALUES (3, '2021-02-21', 2);
```

- Isi record pada tabel transaction_details: 
```sh
INSERT INTO transaction_details VALUES (1, 500000, 2, 2, 1);
INSERT INTO transaction_details VALUES (2, 1500000, 1, 5, 2);
INSERT INTO transaction_details VALUES (3, 3000000, 2, 5, 3);
INSERT INTO transaction_details VALUES (4, 35000000, 1, 3, 4);
INSERT INTO transaction_details VALUES (6, 20000000, 1, 1, 6);
INSERT INTO transaction_details VALUES (7, 5000000, 5, 4, 6);
INSERT INTO transaction_details VALUES (5, 500000, 2, 2, 5);
```


## Melakukan Query sesuai dengan perintah pada soal

- Menampilkan data customer serta berapa jenis produk yang PERNAH dibeli
-- Menggunakan INNER JOIN karena ada kata PERNAH, sehingga yang belum pernah tidak ditampilkan
``` sh
SELECT customers.customer_id, customers.first_name || ' ' ||customers.last_name AS full_name, COUNT(transaction_details.product_id) AS product_bought_by_type
FROM customers
INNER JOIN transaction
ON customers.customer_id = transaction.customer_id
INNER JOIN transaction_details
ON transaction.transaction_id = transaction_details.transaction_id
GROUP BY customers.customer_id
ORDER BY customers.customer_id;
```


- Menampilkan data customer serta total unit produk apapun yang PERNAH dibeli
```sh
SELECT customers.customer_id, customers.first_name || ' ' ||customers.last_name AS full_name, SUM(transaction_details.quantity) AS total_product_bought
FROM customers
INNER JOIN transaction
ON customers.customer_id = transaction.customer_id
INNER JOIN transaction_details
ON transaction.transaction_id = transaction_details.transaction_id
GROUP BY customers.customer_id
ORDER BY customers.customer_id;
```


- Menampilkan data toko dan banyaknya produk yang dijual 
-- Menggunakan LEFT JOIN karena bisa saja ada toko yang belum menjual apa-apa
```sh
SELECT shops.shop_id, shops.permit_number, shops.name, shops.phone_number, COUNT(products.name) AS total_product
FROM shops
LEFT JOIN products
ON shops.shop_id = products.shop_id
GROUP BY shops.shop_id;
```


- Menampilkan Toko dan banyaknya produk yang terjual
```sh
SELECT shops.shop_id, shops.permit_number, shops.name, shops.phone_number, COUNT(products.name) AS total_product, SUM(transaction_details.quantity) AS total_product_sold
FROM shops
LEFT JOIN products
ON shops.shop_id = products.shop_id
INNER JOIN transaction_details
ON products.shop_id = transaction_details.product_id
GROUP BY shops.shop_id;
```


- Menampilkan produk mana yang belum terjual
```sh
SELECT products.product_id, products.name, products.stock, products.stock -  COUNT(transaction_details.quantity) AS stock_left
FROM products
LEFT JOIN transaction_details
ON products.product_id = transaction_details.product_id
WHERE transaction_details IS NULL
GROUP BY products.product_id, products.name, products.stock
ORDER BY products.product_id;
```


- Menampilkan siapa user yang telah mengeluarkan uang terbanyak
```sh
SELECT customers.first_name || ' ' ||customers.last_name AS full_name, SUM(transaction_details.price) AS total_money_spent
FROM customers
INNER JOIN transaction
ON customers.customer_id = transaction.customer_id
INNER JOIN transaction_details
ON transaction.transaction_id = transaction_details.transaction_id
GROUP BY customers.customer_id
ORDER BY SUM(transaction_details.price) DESC LIMIT 1;
```