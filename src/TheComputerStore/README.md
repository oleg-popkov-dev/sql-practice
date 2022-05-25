
# SQL-practice

### SQL Practice №1 | The computer store

#### Relational Schema

![Computer-store-db](https://user-images.githubusercontent.com/69513400/130349372-8618bcfd-3b2e-435d-86b0-98f7e1c8477e.png)

#### Table creation code

``` sql
CREATE TABLE Manufacturers (
	Code INTEGER PRIMARY KEY NOT NULL,
	Name CHAR(50) NOT NULL 
);

CREATE TABLE Products (
	Code INTEGER PRIMARY KEY NOT NULL,
	Name CHAR(50) NOT NULL ,
	Price REAL NOT NULL ,
	Manufacturer INTEGER NOT NULL 
		CONSTRAINT fk_Manufacturers_Code REFERENCES Manufacturers(Code)
);
```

#### Sample dataset

``` sql
INSERT INTO Manufacturers (Code, Name) VALUES (1, 'Sony');
INSERT INTO Manufacturers (Code, Name) VALUES (2, 'Creative Labs');
INSERT INTO Manufacturers (Code, Name) VALUES (3, 'Hewlett-Packard');
INSERT INTO Manufacturers (Code, Name) VALUES (4, 'Iomega');
INSERT INTO Manufacturers (Code, Name) VALUES (5, 'Fujitsu');
INSERT INTO Manufacturers (Code, Name) VALUES (6, 'Winchester');
INSERT INTO Manufacturers (Code, Name) VALUES (7, 'Bose');
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (1, 'Hard drive', 240, 5);
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (2, 'Memory', 120, 6);
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (3, 'ZIP drive', 150, 4);
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (4, 'Floppy disk', 5, 6);
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (5, 'Monitor', 240, 1);
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (6, 'DVD drive', 180, 2);
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (7, 'CD drive', 90, 2);
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (8, 'Printer', 270, 3);
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (9, 'Toner cartridge', 66, 3);
INSERT INTO Products (Code, Name, Price, Manufacturer) VALUES (10, 'DVD burner', 180, 2);
```

#### Exercises
1.Select the names of all the products in the store.

2.Select the names and the prices of all the products in the store.

3.Select the name of the products with a price less than or equal to $200.

4.Select all the products with a price between $60 and $120.

5.Select the name and price in cents (i.e., the price must be multiplied by 100).

6.Compute the average price of all the products.

7.Compute the average price of all products with manufacturer code equal to 2.

8.Compute the number of products with a price larger than or equal to $180.

9.Select the name and price of all products with a price larger than or equal to $180, and sort first by price (in descending order), and then by name (in ascending order).

10.Select all the data from the products, including all the data for each product's manufacturer.

11.Select the product name, price, and manufacturer name of all the products.

12.Select the average price of each manufacturer's products, showing only the manufacturer's code.

13.Select the average price of each manufacturer's products, showing the manufacturer's name.

14.Select the names of manufacturer whose products have an average price larger than or equal to $150.

15.Select the name and price of the cheapest product.

16.Select the name of each manufacturer along with the name and price of its most expensive product.

17.Select the name of each manufacturer which have an average price above $145 and contain at least 2 different products.

18.Add a new product: Loudspeakers, $70, manufacturer 2.

19.Update the name of product 8 to "Laser Printer".

20.Apply a 10% discount to all products.

21.Apply a 10% discount to all products with a price larger than or equal to $120.


#### Answers

``` sql
1.SELECT name FROM products;
2.SELECT name, price FROM products;
3.SELECT name FROM products WHERE price <= 200;
4.SELECT * FROM products WHERE price BETWEEN 60 AND 120;
5.SELECT name, price * 100 as price_in_cents FROM products;
6.SELECT avg(price) FROM products;
7.SELECT avg(price) FROM products WHERE manufacturer = 2;
8.SELECT count(*) as count FROM products WHERE price >= 180;
9.SELECT name, price FROM products WHERE price >= 180 ORDER BY price DESC, name;
10.SELECT * FROM products, manufacturers WHERE manufacturers.code = products.manufacturer;
11.SELECT p.name, p.price, m.name FROM products p JOIN manufacturers m ON p.manufacturer = m.code;
12.SELECT AVG(price), manufacturer FROM products GROUP BY manufacturer;
13.SELECT AVG(price), manufacturers.name FROM products JOIN manufacturers ON products.manufacturer = manufacturers.code GROUP BY manufacturers.name;
14.SELECT manufacturers.name, avg(price) FROM products JOIN manufacturers ON products.manufacturer = manufacturers.code GROUP BY manufacturers.name HAVING avg(price) >= 150;
15.SELECT name, price FROM products WHERE price = (SELECT min(price) FROM products);
16.SELECT p.name, p.price, m.name FROM products p JOIN manufacturers m ON m.code = p.manufacturer AND price = (SELECT max(p.price) FROM products p WHERE m.code = p.manufacturer);
17.SELECT m.name, avg(p.price), count(p.manufacturer) FROM products p, manufacturers m WHERE m.code = p.manufacturer GROUP BY m.name HAVING avg(p.price) > 145 AND count(p.manufacturer) >= 2;
18.INSERT INTO products (code, name, price, manufacturer) VALUES (11, 'Loupspeakers', 70, 2);
19.UPDATE products SET name = 'Laser Printer' WHERE code = 8;
20.UPDATE products SET price = price - (price * 0.1);
21.UPDATE products SET price = price - (price * 0.1) WHERE price >= 120;
```