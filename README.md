
Copyright (C) 2025 Penan Rajput  <br>
Name : Penan Rajput <br>
Email : <penanrajput1998@gmail.com> <br>
[Follow on LinkedIn ](https://www.linkedin.com/in/penanrajput)


# CYPHER Queries Cheatsheet
## SINGLE TABLE QUERIES

###  User Management (In SQL)
Neo4j doesn't have a direct equivalent for creating or managing users via Cypher. Neo4j uses its internal user management system via Neo4j Admin or Neo4j Browser for such tasks. To manage users, you would use the Neo4j admin console or the Neo4j Browser interface.


1. Initial Queries (User, Schema, DB)
    1. Users functions

        1. List all users: 
            ```sql
            <!-- SQL Query -->
            SELECT User,Host FROM mysql.user;
            ```

            In Neo4j, this would not apply directly. Neo4j doesn't expose users in a way that can be queried via Cypher directly. You would manage users through Neo4j's administrative tools.

    2. Schema + Data Queries

        1. Show Schema
            ```sql
            <!-- SQL Query -->
            Describe {table_name};
            ```
            In Neo4j, you can get a schema (list of labels and relationships) using:

            ```sql
            <!-- Cypher Query -->
            CALL db.schema.visualization();
            ```

        2. <b>Create Node Labels (Cypher)</b> / Create Table (SQL) 
            ```sql
            <!-- SQL Query -->
            CREATE TABLE new_table (id INT, name VARCHAR(50));
            ```
            In Neo4j, you create nodes with labels instead of tables:

            ```sql
            <!-- Cypher Query -->
            CREATE (:NewTable {id: 1, name: 'Example'});
            ```
            

            
        3. Cloning Table (Schema + Data)
            ```sql
            CREATE TABLE new_table LIKE original_table;

            INSERT INTO new_table SELECT * FROM original_table;
            ```
            SAME LIKE BELOW
            ```
            CREATE TABLE employees_dummy
            SELECT * 
            FROM employees;
            ```

            In Neo4j: Neo4j doesn't have the concept of directly copying tables. However, you can copy nodes and relationships:

            ```sql
            <!-- Cypher Query -->
            Copy code
            MATCH (n:OriginalTable)
            CREATE (m:NewTable) SET m = n;
            ``` 

    3. Database Queries
        1. show all database
            ```sql
            show databases;
            ```
            In Neo4j, the concept of databases is supported, but you'd use:

            ```sql
            SHOW DATABASES;
            ```
        2. create database
            ```sql
            create database {db_name};
            ```

            In Neo4j, creating a new database is done through the admin tools:

            ```bash
            # Using Neo4j Admin
            neo4j-admin dbms create-database <db_name>
            ```
        3. use database
            ```
            use {db_name}
            ```
            In Neo4j, use:

            ```sql
            :use <db_name>
            ```

        4. Deleting databases: 
            ```sql 
            DROP DATABASE [database]; 
            ```

            In Neo4j:


            ```sql
            <!-- Cypher Query -->
            neo4j-admin dbms drop-database <db_name>
            ```
2. Datatype
    1. NUMERIC
        1. INT: Whole numbers
        2. Float(m, d) : decimal numbers (approx)
        3. decimal(m, d) : decimal numbers (precise)
    2. NON-NUMERIC
        1. CHAR(N) : Fixed length character
        2. VARCHAR(N) : Varying length character
        3. ENUM('M','F') : Value from a defined list
        4. BOOLEAN : True of False values
    3. DATA AND TIME TYPES
        1. DATE : Date (YYYY-MM-DD)
        2. DATETIME : Date and the time (YYYY-MM-DD HH-MM-SS)
        3. TIME : Time (HHH-MM-SS)
        4. YEAR : Year (YYYY)
    
    Neo4j uses property types such as String, Boolean, Integer, Float, and DateTime. You would store data in nodes and relationships.

    ```sql
    <-- Cypher Query -->
    CREATE (n:Person {name: 'Alice', age: 30, isActive: true, birthday: date('1990-01-01')});
    ```

2. DDL (create, alter, drop, truncate)
    1. Create / <b>Create Node Labels (Similar to Table)</b>
        1. syntax
            ```sql
            create table {table_name}(
            {column_name_1} datatype_1 costraint_1,
            {column_name_2} datatype_2 costraint_2,
            {column_name_3} datatype_3 costraint_3
            );
            ```
        2. create table example-1
            ```sql
            CREATE TABLE PRODUCTS(
                NAME VARCHAR(20),
                PRICE int(6), 
                QTY int(6), 
                DISCOUNT float(6,4), 
                SELLER VARCHAR(20), 
                RATING INT(1)
                );
            ```

            ```sql
            <-- Cypher Query -->
            CREATE (:Product {name: 'Product 1', price: 100});
            ```
        3. copy from existing table 
            ```
            CREATE TABLE products_3  SELECT DISTINCT * FROM products_2;
            ```
        4. CREATE INDEX generates an index for a table. Indexes are used to retrieve data from a database faster.
            ```sql
            <-- SQL Query -->
            CREATE INDEX idx_name
            ON customers (name);
            ```
            In Neo4j:

            ```sql
            <!-- Cypher Query -->
            CREATE INDEX ON :Customer(name);
            ```
        
        5. CREATE VIEW creates a virtual table based on the result set of an SQL statement. A view is like a regular table (and can be queried like one), but it is not saved as a permanent table in the database.
            
            ```sql
            <-- SQL Query -->
            CREATE VIEW [Bob Customers] AS
            SELECT name, age
            FROM customers
            WHERE name = ‘Bob’;
            ```

            In Neo4j, you can create a "view" by writing a query that returns the result you need:

            ```sql
            <!-- Cypher Query -->
            MATCH (c:Customer)
            WHERE c.name = 'Bob'
            RETURN c.name, c.age;
            ```

        
    2. Drop / <b>Drop Node Labels (Similar to Drop Table)</b>
        1. drop existing
            ```
            <!-- SQL Query -->
            DROP TABLE {table_name};
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (n:Product) DETACH DELETE n;
            ```
        2. drop if exists
            ```
            DROP TABLE {table_name} if exists;
            ```
        3. Drop database
            DROP DATABASE dataquestDB;
        4. DROP INDEX
            DROP INDEX idx_name;
        
    3. Alter
        1. ADD COLUMN
            ```sql
            ALTER TABLE PRODUCTS 
            ADD productID INT(8);
            ```    

            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            SET p.productID = 123;
            ```
        1. Adding a column with an unique, auto-incrementing ID
            ```
            ALTER TABLE [table] 
            ADD COLUMN [column] int NOT NULL AUTO_INCREMENT PRIMARY KEY;
            ```
      
        3. delete 1 column 
            ```sql
            <!-- SQL Query -->
            ALTER TABLE products
            DROP COLUMN product_id;
            ```

            In Neo4j:

            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            REMOVE p.productID;
            ```
        4. drop two or more columns
            ```
            ALTER TABLE table
            DROP COLUMN column_1,
            DROP COLUMN column_2;
            ```
        5. update column value - way 1
            ```
            ALTER TABLE PRODUCTS 
            ALTER COLUMN RATING INT(2);
            ```
        6. update column value - way 2
            ```
            ALTER TABLE PRODUCTS 
            MODIFY COLUMN SELLER VARCHAR(60);
            ```
        7. Rearrangin the records: 
            ```
            ALTER TABLE foo
            CHANGE COLUMN bar
            bar COLUMN_DEFINITION_HERE
            FIRST;
            ......... AFTER OTHER_COLUMN;
        
            
            /*
            ALTER TABLE products
            CHANGE COLUMN productID 
            productID int(8)
            FIRST;
            */
            ``` 
        8. Removing table columns: 
            ```
            ALTER TABLE [table] DROP COLUMN [column];
            ```
        9. COLUMN NAME CHANGE
            ```sql
            <!-- SQL Query -->
            ALTER TABLE PRODUCTS CHANGE COLUMN productID new_productID INT;
            ```
            In Neo4j: You can't rename properties directly. You need to add a new property and delete the old one:

            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            SET p.new_productID = p.productID
            REMOVE p.productID;
            ```
    4. Alter + permanent way
        1. How to sort a MYSQL table in a permanent way?
            1. Syntax
                ```
                ALTER TABLE tablename ORDER BY columnname ASC;
                ```
            2. Ex-1

        2. How to rename a SQL Column in permanent way ?
            1. Syntax
                ```
                ALTER TABLE {column_name} 
                CHANGE {oldname} {newname} {datatype(size)};
                ```
            1. Ex-1 
                ```
                ALTER TABLE customers 
                CHANGE CUST_ID ID int(4);
                ```
        3.  Change datatype DATA TYPE
            1. Syntax
                ```
                ALTER TABLE {table_name} 
                MODIFY COLUMN {column_name} {new_datatype(new_size)};
                ```
                
            2. Ex 1
                ```
                ALTER TABLE products_2 
                MODIFY COLUMN productID int(8);
                ```
            


    4. truncate = Delete all records in a table: 
        1. syntax
            ```
            TRUNCATE table {table_name};    
            ```
2. DML (insert, update, delete)
    1. insert
        1. insert 1 rows
            ```sql
            <!-- SQL Query -->
            INSERT INTO PRODUCTS VALUES("PENAN0",20);
            ```
            ```sql
            <!-- Cypher Query -->
            CREATE (:Product {name: "PENAN0", price: 20});
            ```
        2. insert 2+ rows
            ```sql
            <!-- SQL Query -->
            insert into products values
            ("BOOKS",20000,300,20,"EBAY",4.7,16,'2018-12-22'), 
            ("CARDS",2000,34,25.5,"AMAZON INC INTERNATIONAL",5,17,'2018-12-23'), 
            ("TV SETS",770000,200,20,"AMAZON INC. INDIA",4.5,18,'2018-5-2');
            ```

            ```sql
            <!-- Cypher Query -->
            CREATE (:Product {name: "BOOKS", price: 20000, qty: 300, discount: 20, seller: "EBAY", rating: 4.7, productID: 16, date: date("2018-12-22")}),

            (:Product {name: "CARDS", price: 2000, qty: 34, discount: 25.5, seller: "AMAZON INC INTERNATIONAL", rating: 5, productID: 17, date: date("2018-12-23")}),

            (:Product {name: "TV SETS", price: 770000, qty: 200, discount: 20, seller: "AMAZON INC. INDIA", rating: 4.5, productID: 18, date: date("2018-05-02")});
            ```
        3. insert in selected columns 1 row
            ```sql
            <!-- SQL Query -->
            insert into products (NAME, PRICE,SELLER, RATING) 
            values ("MOUSE",150,"AMAZON INDIA",5);
            ```
            ```sql
            <!-- Cypher Query -->
            CREATE (:Product {name: "MOUSE", price: 150, seller: "AMAZON INDIA", rating: 5});
            ```

        4. insert in selected columns 2+ row
            ```sql
            <!-- SQL Query -->
            insert into products (NAME,RATING, PRICE,SELLER) 
            values ("SCREEN",3.5,1150,"FLIPKART"), 
            ("BEDSHEETS",4.1,75,"FLIPKART");;
            ```

            ```sql
            <!-- Cypher Query -->
            CREATE (:Product {name: "SCREEN", rating: 3.5, price: 1150, seller: "FLIPKART"}),

            (:Product {name: "BEDSHEETS", rating: 4.1, price: 75, seller: "FLIPKART"});
            ```
    2. update
        1. syntax
            ```
            UPDATE tableName 
            SET [columnName = new_value]
            [WHERE condition]
            ```
        2. example 1
            ```sql
            <!-- SQL Query -->
            update products 
            set SELLER="AMAZON" 
            where NAME = "LAPTOP";
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product {name: "LAPTOP"})
            SET p.seller = "AMAZON";
            ```
    3. delete
        1. syntax
            ```
            DELETE FROM [table] 
            WHERE [column] = [value];
            ```
 
        1. delete + where
            ```sql
            <!-- SQL Query -->
            DELETE FROM CUSTOMERS
            WHERE CUST_ID=2;
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (c:Customer {custID: 2})
            DETACH DELETE c;
            ```
            
        2. delete + in 
            ```sql
            <!-- SQL Query -->
            DELETE FROM products_2 
            where qty in (300,200);
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            WHERE p.qty IN [300, 200]
            DETACH DELETE p;
            ```
        3. Delete *all records* from a table (without dropping the table itself):  
            ```sql
            <!-- SQL Query -->
            DELETE FROM [table];
            ```

            (This also resets the incrementing counter for auto generated columns like an id column.)


            ```sql
            <!-- Cypher Query -->
            MATCH (n:Product)
            DETACH DELETE n;
            ```

4. DQL (select)

    1. select
        1. Selecting specific records: 
            ```sql
            <!-- SQL Query -->
            SELECT * FROM [table] WHERE [column] = [value];` 
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (n:Table {column: value})
            RETURN n;
            ```
        2. Basic Math Operations
            ```sql
            <!-- SQL Query -->
            select 25 % 6 AS "MOD ";
            select 25 * 6 AS "MULTIPLICATION";
            select 25 + 6 AS "ADDITION";
            select 25 / 6 AS "DIVISION";
            select 25 - 6 AS "SUBSTRACTION";
            select 25-6, 25*6;
            ```

            ```sql
            <!-- Cypher Query -->
            RETURN 25 % 6 AS MOD, 
            25 * 6 AS MULTIPLICATION, 
            25 + 6 AS ADDITION, 
            25 / 6 AS DIVISION, 
            25 - 6 AS SUBTRACTION, 
            25 - 6 AS Result1, 
            25 * 6 AS Result2;
            ```
        3. To generate random integer numbers
            ```sql
            <!-- SQL Query -->
            select round(rand()*100) 
            as "RANDOM RESULT";
            ```

            `````sql
            <!-- Cypher Query -->
            RETURN toInteger(rand() * 100) AS RANDOM_RESULT;
            ```
        4. generate concat rows
            ```sql
            <!-- SQL Query -->
            select concat(name,' is of price ',price,' in qty => ',qty,' with discount ',discount,' is brought from ',seller) 
            AS 'DESCRIPTION' 
            from products;
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            RETURN p.name + ' is of price ' + toString(p.price) + ' in qty => ' + toString(p.qty) + 
                ' with discount ' + toString(p.discount) + ' is brought from ' + p.seller AS DESCRIPTION;
            ```
        5. only left characters to be shown
            ```
            <!-- SQL Query -->
            select LEFT(MANUFACTURE_DATE,4) 
            AS "MANUFACTURING DATE" 
            from products;
            ```
            
            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            RETURN substring(p.manufacture_date, 0, 4) AS MANUFACTURING_DATE;
            ```
        6. Select Name and Left Characters = example 2
            ```sql
            <!-- SQL Query -->
            select NAME, LEFT(MANUFACTURE_DATE,4) 
            AS "MANUFACTURING DATE",
            SELLER,QTY 
            from products;
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            RETURN p.name, substring(p.manufacture_date, 0, 4) AS MANUFACTURING_DATE, p.seller, p.qty;
            ```

        7. right characters to be shown
            ```sql
            <!-- SQL Query -->
            select NAME, 
            RIGHT(NAME,4) AS "NAME(LAST 4)",
            SELLER,
            QTY 
            from products;
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            RETURN p.name, substring(p.name, size(p.name) - 4, 4) AS NAME_LAST_4, p.seller, p.qty;
            ```

        8.  Custom column output names: 
            ```sql
            <!-- SQL Query -->
            SELECT [column] AS [custom-column] FROM [table];
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (n:Table)
            RETURN n.column AS custom_column;
            ```
        9. select All Records(*)
            ```sql
            <!-- SQL Query -->
            SELECT * 
            FROM customers;
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (c:Customer)
            RETURN c;
            ```


        10. select distince (=without duplicates)
            ```sql
            <!-- SQL Query -->
            SELECT DISTINCT name 
            FROM customers;
            ```

            ```sql
            <!-- SQL Query -->
            SELECT distinct name, email, acception FROM owners WHERE acception = 1 AND date >= 2015-01-01 00:00:00
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (c:Customer)
            RETURN DISTINCT c.name;
            ```



        11. SELECT INTO copies the specified data from one table into another.
            ```sql
            <!-- SQL Query -->
            SELECT * INTO customers
            FROM customers_backup;
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (b:CustomerBackup)
            CREATE (c:Customer)
            SET c = b;
            ```

        12. SELECT TOP only returns the top x number or percent from a table.
            ```sql
            <!-- SQL Query -->
            SELECT TOP 50 * FROM customers;
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (c:Customer)
            RETURN c
            LIMIT 50;
            ```

          
        13. AS Rename column or table using an _alias_: 
            ```
            SELECT [table1].[column] AS '[value]', [table2].[column] AS '[value]' FROM [table1], [table2];
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (t1:Table1), (t2:Table2)
            RETURN t1.column AS Value1, t2.column AS Value2;
            ```
        14. WHERE
            (Selectors: `<`, `>`, `!=`; combine multiple selectors with `AND`, `OR`)


        
        15. AND
            SELECT name
            FROM customers
            WHERE name = ‘Bob’ AND age = 55;

            ```sql
            <!-- SQL Query -->
            SELECT * FROM products WHERE QTY > 400 AND RATING > 4;
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            WHERE p.qty > 400 AND p.rating > 4
            RETURN p;
            ```

        16. OR
            SELECT name
            FROM customers
            WHERE name = ‘Bob’ OR age = 55;

            ```
            select * from products where QTY < 400 or RATING > 4;
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            WHERE p.qty < 400 OR p.rating > 4
            RETURN p;
            ```


        17. BETWEEN
            ```sql
            <!-- SQL Query -->
            SELECT name
            FROM customers
            WHERE age BETWEEN 45 AND 55;
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (c:Customer)
            WHERE c.age BETWEEN 45 AND 55
            RETURN c;
            ```



    2. Explain records: 
        ```sql
        <!-- SQL Query -->
        EXPLAIN SELECT * FROM [table];
        ```
        ```sql
        <!-- Cypher Query -->
        EXPLAIN MATCH (n:Table)
        RETURN n;
        ```

5. Date
    ```sql
    <!-- SQL Query -->
    select * from products 
    where MANUFACTURE_DATE < '2014-01-01';
    ```

    ```sql
    <!-- Cypher Query -->
    MATCH (p:Product)
    WHERE p.manufacture_date < date('2014-01-01')
    RETURN p;
    ```

6. Time 
    1. Current Time: 
        ```sql
        <!-- SQL Query -->
        SELECT NOW()
        ```

        ```sql
        <!-- Cypher Query -->
        RETURN datetime() AS CurrentTime
        ```


7. Commands 

    1. in, not in
        1. in
            ```sql
            <!-- SQL Query -->
            select * from products where rating in (3,5); 
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            WHERE p.rating IN [3, 5]
            RETURN p
            ```
        4. not in 
            ```sql
            <!-- SQL Query -->
            select * from products where rating not in (3,5);
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (p:Product)
            WHERE p.rating NOT IN [3, 5]
            RETURN p
            ```
    2. IS NULL will return only rows with a NULL value.
        ```sql
        <!-- SQL Query -->
        SELECT name
        FROM customers
        WHERE name IS NULL;
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (c:Customer)
        WHERE c.name IS NULL
        RETURN c;
        ```
    3. IS NOT NULL
        ```sql
        <!-- SQL Query -->
        SELECT name
        FROM customers
        WHERE name IS NOT NULL;
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (c:Customer)
        WHERE c.name IS NOT NULL
        RETURN c;
        ```
    2. LIKE 
        ```
        %x — will select all values that begin with x
        %x% — will select all values that include x
        x% — will select all values that end with x
        x%y — will select all values that begin with x and end with y
        _x% — will select all values have x as the second character
        x_% — will select all values that begin with x and are at least two characters long. You can add additional _ characters to extend the length requirement, i.e. x___%
        ```

        1. Select records containing `[value]`: 
            ```sql
            <!-- SQL Query -->
            SELECT * FROM [table] WHERE [column] LIKE '%[value]%';
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (n:Table)
            WHERE n.column CONTAINS '[value]'
            RETURN n;
            ```
        

        2. Select records starting with `[value]`: 
            ```sql
            <!-- SQL Query -->
            SELECT * FROM [table] WHERE [column] LIKE '[value]%';
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (n:Table)
            WHERE n.column STARTS WITH '[value]'
            RETURN n;
            ```

        3. Select records starting with `val` and ending with `ue`: 
            ```sql
            <!-- SQL Query -->
            SELECT * FROM [table] WHERE [column] LIKE '[val_ue]';
            ```    

            ```sql
            <!-- Cypher Query -->
            MATCH (n:Table)
            WHERE n.column STARTS WITH "val" AND n.column ENDS WITH "ue"
            RETURN n;


    4. ORDER BY
        1. Select with custom order and only limit: 
            ```sql
            <!-- SQL Query -->
            SELECT * FROM [table] WHERE [column] ORDER BY [column] ASC LIMIT [value];` (Order: `DESC`, `ASC`)
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (n:Table)
            RETURN n
            ORDER BY n.column ASC
            LIMIT [value]
            ```
    
7. Aggregate functions (count, min, max, sum, avg)

    2. SUM Calculate total number of records: 
        ```sql
        <!-- SQL Query -->
        SELECT SUM([column]) FROM [table];
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (n:Table)
        RETURN sum(n.column) AS Total
        ```

    3. SUM Count total number of `[column]` and group by 
        ```sql
        <!-- SQL Query -->
        [category-column]`: `SELECT [category-column], SUM([column]) FROM [table] GROUP BY [category-column];
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (n:Table)
        RETURN n.categoryColumn, sum(n.column) AS Total
        ```

    4. MAX Get largest value in `[column]`: 
        ```sql
        <!-- SQL Query -->
        SELECT MAX([column]) FROM [table];
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (n:Table)
        RETURN max(n.column) AS MaxValue
        ```


    5. MIN Get smallest value: 
        ```sql
        <!-- SQL Query -->
        SELECT MIN([column]) FROM [table];
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (n:Table)
        RETURN min(n.column) AS MinValue
        ```

    6. AVG Get average value: 
        ```sql
        <!-- SQL Query -->
        SELECT AVG([column]) FROM [table];
        ```
        ```sql
        <!-- Cypher Query -->
        MATCH (n:Table)
        RETURN avg(n.column) AS AvgValue
        ```

    7. ROUND(AVG) Get rounded average value and group by 
        ```sql
        <!-- SQL Query -->
        SELECT [category-column], ROUND(AVG([column]), 2) FROM [table] GROUP BY [category-column];
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (n:Table)
        RETURN n.categoryColumn, round(avg(n.column), 2) AS RoundedAverage
        ```
    
    8. COUNT() Counting records: 
        ```sql
        <!-- SQL Query -->
        SELECT COUNT([column]) FROM [table];
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (n:Table)
        RETURN count(n.column) AS Count
        ```

    9. COUNT(*) using *, so the total row count for customers would be returned.
        ```sql
        SELECT COUNT(*)
        FROM customers;
        ```

        ```sql
        MATCH (c:Customer)
        RETURN count(*) AS TotalCustomers
        ```

    9. COUNT Counting and selecting grouped records: 
        ```
        SELECT *, (SELECT COUNT([column]) FROM [table]) AS count FROM [table] GROUP BY [column];
        ```
    8. GROUP BY groups rows with the same values into summary rows  + aggregate function
        ```sql
        <!-- SQL Query -->
        SELECT name, AVG(age)
        FROM customers
        GROUP BY name;
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (c:Customer)
        RETURN c.name, avg(c.age) AS AverageAge
        ```

    9. HAVING is used with group by only \
    HAVING checks condition. \
    select -> where,  \
    group by -> order by
        ```sql
        <!-- SQL Query -->
        SELECT COUNT(customer_id), name
        FROM customers
        GROUP BY name
        HAVING COUNT(customer_id) > 2;
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (c:Customer)
        WITH c.name, count(c.customerId) AS CustomerCount
        WHERE CustomerCount > 2
        RETURN c.name, CustomerCount
        ```


    10. ORDER BY
        1. ORDER BY sets the order of the returned results. The order will be ascending by default.
            ```sql
            <!-- SQL Query -->
            SELECT name
            FROM customers
            ORDER BY age;
            ```
            ```sql
            <!-- Cypher Query -->
            MATCH (c:Customer)
            RETURN c.name
            ORDER BY c.age ASC
            ```

        2. DESC
        DESC will return the results in descending order.
            ```sql
            <!-- SQL Query -->
            SELECT name
            FROM customers
            ORDER BY age DESC;
            ```

            ```sql
            <!-- Cypher Query -->
            MATCH (c:Customer)
            RETURN c.name
            ORDER BY c.age DESC
            ```

## MULTIPLE TABLE QUERIES

8. JOINs = Multiple tables (Types = INNER, LEFT, RIGHT, FULL)

    1. Select from multiple tables: 
        ```sql
        <!-- SQL Query -->
        SELECT [table1].[column], [table1].[another-column], [table2].[column] FROM [table1], [table2]; 
        ```
        ```sql
        <!-- Cypher Query -->
        MATCH (t1:Table1), (t2:Table2)
        RETURN t1.column, t1.anotherColumn, t2.column;
        ```


    2. Combine rows from different tables: 
        ```sql
        <!-- SQL Query -->
        SELECT * FROM [table1] INNER JOIN [table2] ON [table1].[column] = [table2].[column];
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (t1:Table1)-[:RELATIONSHIP]->(t2:Table2)
        WHERE t1.column = t2.column
        RETURN t1, t2;
        ```

    3. Combine rows from different tables but do not require the join condition: 
        ```sql
        <!-- SQL Query -->
        SELECT * FROM [table1] LEFT OUTER JOIN [table2] ON [table1].[column] = [table2].[column];
        ```
        (The left table is the first table that appears in the statement.)

        ```sql
        <!-- Cypher Query -->
        MATCH (t1:Table1)
        OPTIONAL MATCH (t1)-[:RELATIONSHIP]->(t2:Table2)
        RETURN t1, t2;
        ```
    


    4. INNER JOIN
        INNER JOIN selects records that have matching values in both tables.
        ```sql
        <!-- SQL Query -->
        SELECT name
        FROM customers
        INNER JOIN orders
        ON customers.customer_id = orders.customer_id;
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (c:Customer)-[:PLACED]->(o:Order)
        RETURN c.name;
        ```

    5. LEFT JOIN
        LEFT JOIN selects records from the left table that match records in the right table. In the below example the left table is customers.
        ```sql
        <!-- SQL Query -->
        SELECT name
        FROM customers
        LEFT JOIN orders
        ON customers.customer_id = orders.customer_id;
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (c:Customer)
        OPTIONAL MATCH (c)-[:PLACED]->(o:Order)
        RETURN c.name, o;
        ```
    6. RIGHT JOIN
        RIGHT JOIN selects records from the right table that match records in the left table. In the below example the right table is orders.
        ```sql
        <!-- SQL Query -->
        SELECT name
        FROM customers
        RIGHT JOIN orders
        ON customers.customer_id = orders.customer_id;
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (o:Order)
        OPTIONAL MATCH (c:Customer)-[:PLACED]->(o)
        RETURN c.name, o;
        ```


    7. FULL JOIN
        FULL JOIN selects records that have a match in the left or right table. Think of it as the “OR” JOIN compared with the “AND” JOIN (INNER JOIN).
        ```sql
        <!-- SQL Query -->
        SELECT name
        FROM customers
        FULL OUTER JOIN orders
        ON customers.customer_id = orders.customer_id;
        ```
        ```sql
        <!-- Cypher Query -->
        MATCH (c:Customer)
        OPTIONAL MATCH (c)-[:PLACED]->(o:Order)
        WITH c, o
        MATCH (o:Order)
        OPTIONAL MATCH (o)<-[:PLACED]-(c:Customer)
        RETURN c.name, o;
        ```

    8. EXISTS
        EXISTS is used to test for the existence of any record in a subquery.
        ```sql
        <!-- SQL Query -->
        SELECT name
        FROM customers
        WHERE EXISTS
        (SELECT order FROM ORDERS WHERE customer_id = 1);
        ```

        ```sql
        <!-- Cypher Query -->
        MATCH (c:Customer)
        WHERE EXISTS {
        MATCH (c)-[:PLACED]->(:Order {customer_id: 1})
        }
        RETURN c.name;
        ```


10. CONSTRAINTS & KEYS

    show all constraints
    ```
        select * from information_schema.table_constraints
        where constraint_schema = 'db_name';
    ```
    ```
    Constraints are commonly used in SQL are :

    1. NOT NULL - Ensures that a column cannot have a NULL value

    2. UNIQUE - Ensures that all values in a column are different

    3. PRIMARY KEY - A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table (UNIQUE + NOT NULL)

    4. FOREIGN KEY - Prevents actions that would destroy links between tables

    5. CHECK - Ensures that the values in a column satisfies a specific condition

    6. DEFAULT - Sets a default value for a column if no value is specified

    7. CREATE INDEX - Used to create and retrieve data from the database very quickly
    ```
    1. NOT NULL
        1. on CREATE TABLE
            ```sql
            <!-- SQL Query -->
            CREATE TABLE Persons (
            ID int NOT NULL,
            LastName varchar(255) NOT NULL,
            FirstName varchar(255) NOT NULL,
            Age int
            );
            ```
            ```sql
            <!-- Cypher Query -->
            CREATE CONSTRAINT ON (p:Person) ASSERT EXISTS(p.ID);
            CREATE CONSTRAINT ON (p:Person) ASSERT EXISTS(p.LastName);
            CREATE CONSTRAINT ON (p:Person) ASSERT EXISTS(p.FirstName);
            ```

        2. on ALTER TABLE
            ```sql
            <!-- SQL Query -->
            ALTER TABLE Persons
            MODIFY Age int NOT NULL;
            ```
            ```sql
            <!-- Cypher Query -->
            CREATE CONSTRAINT ON (p:Person) ASSERT EXISTS(p.Age);

    2. UNIQUE
        1. on CREATE TABLE
            <br>Example 1
            ```sql
            <!-- SQL Query -->
            CREATE TABLE Persons (
            ID int NOT NULL UNIQUE,
            LastName varchar(255) NOT NULL,
            FirstName varchar(255),
            Age int
            );
            ```

            ```sql
            <!-- Cypher Query -->
            CREATE CONSTRAINT ON (p:Person) ASSERT p.ID IS UNIQUE;
            ```

            <br>Example 2

            ```sql
            <!-- SQL Query -->
            CREATE TABLE Persons (
            ID int NOT NULL,
            LastName varchar(255) NOT NULL,
            FirstName varchar(255),
            Age int,
            UNIQUE (ID)
            );
            ```

            ```sql
            <!-- Cypher Query -->
            CREATE CONSTRAINT ON (p:Person) ASSERT p.ID IS UNIQUE;
            ```


            <br>Example 3
            ```sql
            <!-- SQL Query -->
            CREATE TABLE Persons (
            ID int NOT NULL,
            LastName varchar(255) NOT NULL,
            FirstName varchar(255),
            Age int,
            CONSTRAINT UC_Person UNIQUE (ID,LastName)
            );
            ```
            ```sql
            <!-- Cypher Query -->
            CREATE CONSTRAINT ON (p:Person) ASSERT (p.ID, p.LastName) IS NODE KEY;
            ```
        2. on ALTER TABLE
            1. without constraint_name

                ```sql
                <!-- SQL Query -->
                ALTER TABLE Persons
                ADD UNIQUE ({column_name});
                ```
                ```sql
                <!-- Cypher Query -->
                CREATE CONSTRAINT ON (p:Person) ASSERT p.column_name IS UNIQUE;
                ```
            2. EX-1
                
                ```sql
                <!-- SQL Query -->
                ALTER TABLE Persons
                ADD UNIQUE (ID);
                ```

                ```sql
                <!-- Cypher Query -->
                CREATE CONSTRAINT ON (p:Person) ASSERT p.ID IS UNIQUE;
                ```


            3. with constraint_name

                ```
                ALTER TABLE Persons
                ADD CONSTRAINT {constraint_name} 
                UNIQUE (column_name1, column_name2);
                ```
            4. EX-2
                 ```sql
                <!-- SQL Query -->
                ALTER TABLE Persons
                ADD CONSTRAINT UC_Person UNIQUE (ID,LastName);
                ```

                ```sql
                <!-- Cypher Query -->
                CREATE CONSTRAINT ON (p:Person) ASSERT (p.ID, p.LastName) IS NODE KEY;
                ```
        3. DROP a UNIQUE Constraint
            1. by INDEX 

                ```sql
                <!-- SQL Query -->
                ALTER TABLE Persons
                DROP INDEX UC_Person;
                ```

                ```sql
                <!-- Cypher Query -->
                DROP CONSTRAINT UC_Person;
                ```


           2. by CONSTRAINT

                ```sql
                <!-- SQL Query -->
                ALTER TABLE Persons
                DROP CONSTRAINT UC_Person;
                ```
                ```sql  
                <!-- Cypher Query -->
                DROP CONSTRAINT UC_Person;
                ```
            
    1. PRIMARY KEYS
        1. IMPLICITELY on create table
            ```sql
            <!-- SQL Query -->
            CREATE TABLE Persons (
            ID int NOT NULL,
            LastName varchar(255) NOT NULL,
            FirstName varchar(255),
            Age int,
            PRIMARY KEY (ID)
            );
            ```

            ```sql
            <!-- Cypher Query -->
            CREATE CONSTRAINT ON (p:Person) ASSERT p.ID IS UNIQUE;

            OR ----

            CREATE CONSTRAINT ON (p:Person) ASSERT (p.ID) IS NODE KEY;
            ```


            ```sql
            <!-- SQL Query -->
            CREATE TABLE Persons (
            ID int NOT NULL PRIMARY KEY,
            LastName varchar(255) NOT NULL,
            FirstName varchar(255),
            Age int
            );
            ```

            ```sql
            <!-- Cypher Query -->
            CREATE CONSTRAINT ON (p:Person) ASSERT (p.ID) IS NODE KEY;

            OR

            CREATE CONSTRAINT ON (p:Person) ASSERT p.ID IS UNIQUE;
            ```

            ```sql
            <!-- SQL Query -->
            CREATE TABLE Persons (
            ID int NOT NULL,
            LastName varchar(255) NOT NULL,
            FirstName varchar(255),
            Age int,
            CONSTRAINT PK_Person PRIMARY KEY (ID,LastName)
            );
            ```
            ```sql
            <!-- Cypher Query -->
            CREATE CONSTRAINT ON (p:Person) ASSERT (p.ID, p.LastName) IS NODE KEY;
            ```
        2. EXPLICITELY on alter table
            ```sql
            <!-- SQL Query -->
            ALTER TABLE Persons
            ADD PRIMARY KEY (ID);
            ```
            ```sql
            <!-- Cypher Query -->
            CREATE CONSTRAINT ON (p:Person) ASSERT p.ID IS UNIQUE;
            ```

            ```
            ALTER TABLE Persons
            ADD CONSTRAINT PK_Person PRIMARY KEY (ID,LastName);
            ```
        3.  DROP a PRIMARY KEY Constraint
            ```sql
            <!-- SQL Query -->
            ALTER TABLE Persons
            DROP PRIMARY KEY;
            ```

            ```sql
            <!-- Cypher Query -->
            DROP CONSTRAINT ON (p:Person) ASSERT p.ID IS UNIQUE;
            ```

            ```
            ALTER TABLE Persons
            DROP CONSTRAINT PK_Person;
            ```

    4. FOREIGN KEY
        1. on CREATE TABLE
            ```
            CREATE TABLE Orders (
                OrderID int NOT NULL,
                OrderNumber int NOT NULL,
                PersonID int,
                PRIMARY KEY (OrderID),
                FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
            );
            ```

             ```
            CREATE TABLE Orders (
            OrderID int NOT NULL PRIMARY KEY,
            OrderNumber int NOT NULL,
            PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
            );
            ```

            ```
            CREATE TABLE Orders (
            OrderID int NOT NULL,
            OrderNumber int NOT NULL,
            PersonID int,
            PRIMARY KEY (OrderID),
            CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID)
            REFERENCES Persons(PersonID)
            );
             ```

        2. on ALTER TABLE
            1. without constraint_name
                ```
                ALTER TABLE Orders
                ADD FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
                ```
            2. with costraint_name
                ```
                ALTER TABLE Orders
                ADD CONSTRAINT {constraint_name}
                FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
                ```
            3. Ex - 1
                ```
                ALTER TABLE Orders
                ADD CONSTRAINT FK_PersonOrder
                FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
                ```

        3. DROP a FOREIGN KEY Constraint
            1. by FOREIGN KEY {constraint_name}
                ```
                ALTER TABLE Orders
                DROP FOREIGN KEY {constraint_name};
                ```
            2. Ex 1
                  ```
                ALTER TABLE Orders
                DROP FOREIGN KEY FK_PersonOrder;
                ```

            2. by CONSTRAINT {constraint_name}
                 ```
                ALTER TABLE Orders
                DROP CONSTRAINT {constraint_name};
                ```
            4. Ex 1
                ```
                ALTER TABLE Orders
                DROP CONSTRAINT FK_PersonOrder;
                ```


    5. CHECK
        1. on CREATE TABLE
            ```
            CREATE TABLE Persons (
                ID int NOT NULL,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int,
                CHECK (Age>=18)
            );
            ```

            ```
            CREATE TABLE Persons (
                ID int NOT NULL,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int CHECK (Age>=18)
            );
            ```


            ```
            CREATE TABLE Persons (
                ID int NOT NULL,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int,
                City varchar(255),
                CONSTRAINT CHK_Person CHECK (Age>=18 AND City='Sandnes')
            );
            ```
        2. on ALTER TABLE
            ```
            ALTER TABLE Persons
            ADD CHECK (Age>=18);
            ```

            ```
            ALTER TABLE Persons
            ADD CONSTRAINT CHK_PersonAge CHECK (Age>=18 AND City='Sandnes');
            ```
        3. DROP a CHECK Constraint
            ```
            ALTER TABLE Persons
            DROP CONSTRAINT CHK_PersonAge;
            ```
        
            ```
            ALTER TABLE Persons
            DROP CHECK CHK_PersonAge;
            ```
    6. DEFAULT
        1. on CREATE TABLE
            ```
            CREATE TABLE Persons (
                ID int NOT NULL,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int,
                City varchar(255) DEFAULT 'Sandnes'
            );
            ```

            ```
            CREATE TABLE Orders (
                ID int NOT NULL,
                OrderNumber int NOT NULL,
                OrderDate date DEFAULT GETDATE()
            );
            ```
        2. on ALTER TABLE
            ```
            ALTER TABLE Persons
            ALTER City SET DEFAULT 'Sandnes';
            ```
            ```
            ALTER TABLE Persons
            ADD CONSTRAINT df_City
            DEFAULT 'Sandnes' FOR City;
            ```
            ```
            ALTER TABLE Persons
            ALTER COLUMN City SET DEFAULT 'Sandnes';
            ```
            ```
            ALTER TABLE Persons
            MODIFY City DEFAULT 'Sandnes';
            ```
        3. DROP a DEFAULT Constraint
            ```
            ALTER TABLE Persons
            ALTER City DROP DEFAULT;
            ```
            ```
            ALTER TABLE Persons
            ALTER COLUMN City DROP DEFAULT;
            ```
            ```
            ALTER TABLE Persons
            ALTER COLUMN City DROP DEFAULT;
            ```
    7. INDEX = used to retrieve data from the database more quickly than otherwise. The users cannot see the indexes, they are just used to speed up searches/queries.
        1. CREATE INDEX Syntax = Creates an index on a table. Duplicate values are allowed:
            ```
            CREATE INDEX index_name
            ON table_name (column1, column2, ...);
            ```

        2. CREATE UNIQUE INDEX Syntax = Creates a unique index on a table. Duplicate values are not allowed:

            ```
            CREATE UNIQUE INDEX index_name
            ON table_name (column1, column2, ...);
            ```
        3. CREATE INDEX Example
            The SQL statement below creates an index named "idx_lastname" on the "LastName" column in the "Persons" table:

            ```
            CREATE INDEX idx_lastname
            ON Persons (LastName);
            ```

            If you want to create an index on a combination of columns, you can list the column names within the parentheses, separated by commas:

            ```
            CREATE INDEX idx_pname
            ON Persons (LastName, FirstName)
            ```
        4. DROP INDEX Statement

            ```
            DROP INDEX index_name ON table_name;
            ```


            ```
            DROP INDEX table_name.index_name;
            ```


            ```
            DROP INDEX index_name;
            ```


            ```
            ALTER TABLE table_name
            DROP INDEX index_name;
            ```
    8.  AUTO INCREMENT
        1. Syntax  
            ```
            CREATE TABLE Persons (
                Personid int NOT NULL AUTO_INCREMENT,
                LastName varchar(255) NOT NULL,
                FirstName varchar(255),
                Age int,
                PRIMARY KEY (Personid)
            );
            ```
        2. on ALTER TABLE
            ```
            ALTER TABLE Persons AUTO_INCREMENT=100;
            ```


10. Operators -> Set Operations
    1. UNION operator
        1. UNION = only distinct values
            1. Syntax
                ```
                SELECT column_name(s) FROM table1
                UNION
                SELECT column_name(s) FROM table2;
                ```
            2. Ex 1
                ```
                SELECT City FROM Customers
                UNION
                SELECT City FROM Suppliers
                ORDER BY City;
                ```
        2. UNION ALL = + allows duplicate values
            1. Syntax
                ```
                SELECT column_name(s) FROM table1
                UNION ALL
                SELECT column_name(s) FROM table2;
                ```
            2. Ex 1
                ```
                SELECT City FROM Customers
                UNION ALL
                SELECT City FROM Suppliers
                ORDER BY City;
                ```
        3. UNION + WHERE
            1. Ex 1
                ```
                SELECT City, Country FROM Customers
                WHERE Country='Germany'
                UNION
                SELECT City, Country FROM Suppliers
                WHERE Country='Germany'
                ORDER BY City;
                ```
        4. UNION ALL + WHERE
            1. Ex 1
                ```
                SELECT City, Country FROM Customers
                WHERE Country='Germany'
                UNION ALL
                SELECT City, Country FROM Suppliers
                WHERE Country='Germany'
                ORDER BY City;
                ```
    2. INTERSECT
        1. Syntax
            ```
            SELECT column1 [, column2 ]
            FROM table1 [, table2 ]
            [WHERE condition]

            INTERSECT

            SELECT column1 [, column2 ]
            FROM table1 [, table2 ]
            [WHERE condition]
            ```
        2. Ex 1
            ```
            SELECT  ID, NAME, AMOUNT, DATE
                FROM CUSTOMERS
                LEFT JOIN ORDERS
                ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID

            INTERSECT

            SELECT  ID, NAME, AMOUNT, DATE
                FROM CUSTOMERS
                RIGHT JOIN ORDERS
                ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
            ```

    3. MINUS



11. SUBQUERIES
    1. Single Row
        ```
        SELECT id, last_name, salary
        FROM employee
        WHERE salary = (
            SELECT MAX(salary)
            FROM employee
        );
        ```

    2. Multi Row 
        ```
        SELECT id, last_name, salary
        FROM employee
        WHERE salary IN (
            SELECT salary
            FROM employee
            WHERE last_name LIKE 'C%'
        );
        ```

12. Operators
    ```
    ALL	-> TRUE if all of the subquery values meet the condition	

    AND	-> TRUE if all the conditions separated by AND is TRUE	

    ANY	-> TRUE if any of the subquery values meet the condition	

    BETWEEN	-> TRUE if the operand is within the range of comparisons	

    EXISTS	-> TRUE if the subquery returns one or more records	

    IN -> TRUE if the operand is equal to one of a list of expressions	
    
    LIKE -> TRUE if the operand matches a pattern	

    NOT	-> Displays a record if the condition(s) is NOT TRUE	

    OR -> TRUE if any of the conditions separated by OR is TRUE	

    SOME -> TRUE if any of the subquery values meet the condition

    ```


13. VIEW [[GFG Link]](https://www.geeksforgeeks.org/sql-views/)
    1. Basics
        1. Create View - Syntax

            ```
            CREATE VIEW <VIEW_NAME> AS SELECT <COLUMN_NAME(S)> FROM <TABLE_NAME>;
            ```

        2. Create - EXAMPLE 1
            ```
            CREATE VIEW <view_name> AS 
            SELECT <Column_Names(S)> 
            FROM <Table_Name> 
            WHERE <Condition>;
            ```
        3. Drop View -  Syntax
            ```
            DROP VIEW <View_Name>;
            ```
        4. Drop View - Example
            ```
            DROP VIEW MarksView;
            ```
        5. Updating View - Syntax
        [[GFG Link]](https://www.geeksforgeeks.org/sql-views/#:~:text=DROP%20VIEW%20MarksView%3B-,UPDATING%20VIEWS,-There%20are%20certain)
    2. Create 
14. PRACTICAL HACKS
    1. Maintain order of search
        ```
        ORDER BY CASE ItemId
            WHEN 9 THEN 1
            WHEN 1 THEN 2
            ELSE        3
            END;
        ```
        Reference Links: <br />
            1. https://stackoverflow.com/questions/15155930/how-do-i-preserve-the-order-of-a-sql-query-using-the-in-command <br />
            2. https://stackoverflow.com/questions/866465/order-by-the-in-value-list  <br />
            3. https://stackoverflow.com/questions/2813884/how-do-you-keep-the-order-using-select-where-in

    2. Find values Not in Second Table

        ```
        drop table if exists t_left, t_right;

        create table t_left (value integer);
        create table t_right (value integer);

        insert into t_left values(50), (60);
        insert into t_right values(50);
        ```

        LEFT JOIN with IS NULL
        ```
        SELECT  l.*
        FROM    t_left l
        LEFT JOIN
                t_right r
        ON      r.value = l.value
        WHERE   r.value IS NULL;
        ```


        NOT IN
        ```
        SELECT  l.*
        FROM    t_left l
        WHERE   l.value NOT IN
                (
                SELECT  value
                FROM    t_right r
                );
        ```


        NOT EXISTS
        ```
        SELECT  l.*
        FROM    t_left l
        WHERE   NOT EXISTS
                (
                SELECT  NULL
                FROM    t_right r
                WHERE   r.value = l.value
                );
        ```
                
                
                
        EXCEPT
        ```
        SELECT l.* FROM
        t_left l
        EXCEPT
        SELECT r.value FROM
        t_right r;
        ```

        OUTPUT
        ```
        value
        60
        ```
        
        REFERENCE LINKS

        1. https://dba.stackexchange.com/questions/37627/identifying-which-values-do-not-match-a-table-row/37628#37628?newreg=2040ae28dfd141978710f03d2fa6630f
        2. https://dba.stackexchange.com/questions/83684/except-operator-vs-not-in
        3. https://stackoverflow.com/questions/2973558/select-a-value-where-it-doesnt-exist-in-another-table
        4. https://stackoverflow.com/questions/12048633/sql-query-to-find-record-with-id-not-in-another-table
        5. https://stackoverflow.com/questions/28945251/sql-server-select-n-from-values0-0-0-0-tn


Some Important Notes
1. [Slideshare - SQL vs MySQL vs Oracle Syntax Difference](https://www.slideshare.net/SteveStarc/sql-mysqloracle) 
2. [IDE MS SQL SERVER](https://onecompiler.com/sqlserver)
3. [Temp Table Creation for Experiment](https://stackoverflow.com/questions/1564956/how-can-i-select-from-list-of-values-in-sql-server)