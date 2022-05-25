# SQL PRACTICE

### SQL Practice №3 | The Warehouse

#### Relational Schema

![Sql_warehouse](https://user-images.githubusercontent.com/69513400/130347774-96d6aab6-cc9e-4813-b312-17d851fe81f8.png)

#### Table creation code
``` sql
CREATE TABLE Warehouses (
   Code INTEGER PRIMARY KEY NOT NULL,
   Location TEXT NOT NULL ,
   Capacity INTEGER NOT NULL 
 );
 
 CREATE TABLE Boxes (
   Code TEXT PRIMARY KEY NOT NULL,
   Contents TEXT NOT NULL ,
   Value REAL NOT NULL ,
   Warehouse INTEGER NOT NULL, 
   CONSTRAINT fk_Warehouses_Code FOREIGN KEY (Warehouse) REFERENCES Warehouses(Code)
 );
 ```

#### Sample dataset

 ``` sql
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (1, 'Chicago', 3);
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (2, 'Chicago', 4);
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (3, 'New York', 7);
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (4, 'Los Angeles', 2);
 INSERT INTO Warehouses (Code, Location, Capacity) VALUES (5, 'San Francisco', 8);
 
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('0MN7', 'Rocks', 180,3);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('4H8P', 'Rocks', 250,1);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('4RT3', 'Scissors', 190,4);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('7G3H', 'Rocks', 200,1);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('8JN6', 'Papers', 75,1);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('8Y6U', 'Papers', 50,3);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('9J6F', 'Papers', 175,2);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('LL08', 'Rocks', 140,4);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('P0H6', 'Scissors', 125,1);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('P2T6', 'Scissors', 150,2);
 INSERT INTO Boxes (Code, Contents, Value, Warehouse) VALUES ('TU55', 'Papers', 90,5);
 ```


#### Exercises

1. Select all warehouses.

2. Select all boxes with a value larger than $150.

3. Select all distinct contents in all the boxes.

4. Select the average value of all the boxes.

5. Select the warehouse code and the average value of the boxes in each warehouse.

6. Same as previous exercise, but select only those warehouses where the average value of the boxes is greater than 150.

7. Select the code of each box, along with the name of the city the box is located in.

8. Select the warehouse codes, along with the number of boxes in each warehouse. Optionally, take into account that some warehouses are empty (i.e., the box count should show up as zero, instead of omitting the warehouse from the result).

9. Select the codes of all warehouses that are saturated (a warehouse is saturated if the number of boxes in it is larger than the warehouse's capacity).

10. Select the codes of all the boxes located in Chicago.

11. Create a new warehouse in New York with a capacity for 3 boxes.

12. Create a new box, with code "H5RT", containing "Papers" with a value of $200, and located in warehouse 2.

13. Reduce the value of all boxes by 15%.

14. Apply a 20% value reduction to boxes with a value larger than the average value of all the boxes.

15. Remove all boxes with a value lower than $100.

16. Remove all boxes from saturated warehouses.


#### Answers

``` sql
1.SELECT * FROM warehouses;
2.SELECT * FROM boxes WHERE value > 150;
3.SELECT DISTINCT contents FROM boxes;
4.SELECT AVG(value) FROM boxes;
5.SELECT warehouse, AVG(value) FROM boxes GROUP BY warehouse;
6.SELECT warehouse, AVG(value) FROM boxes GROUP BY warehouse HAVING AVG(value) > 150;
7.SELECT b.code, w.location FROM boxes b JOIN warehouses w on b.warehouse = w.code;
8.SELECT w.code, COUNT(b.code) FROM warehouses w JOIN boxes b on w.code = b.warehouse GROUP BY w.code;
9.SELECT code FROM warehouses WHERE capacity < (SELECT COUNT(*) FROM boxes WHERE warehouse = warehouses.code);
10.SELECT code FROM boxes WHERE warehouse IN (SELECT code FROM warehouses WHERE location = 'Chicago');
11.INSERT INTO warehouses(code,location,capacity) VALUES(6,'New York',3);
12.INSERT INTO boxes(code,contents,value,warehouse) VALUES('H5RT','Papers',200,2);
13.UPDATE boxes SET value = value - (value * 0.15);
14.UPDATE boxes SET value = value - (value * 0.2) WHERE value > (SELECT AVG(value) FROM (SELECT * FROM boxes) AS val);
15.DELETE FROM boxes WHERE value < 100;
16.DELETE FROM boxes WHERE warehouse IN (SELECT code FROM warehouses WHERE capacity < (SELECT COUNT(*) FROM boxes WHERE warehouse = warehouses.code));
```