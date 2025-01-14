
Copyright (C) 2025 Penan Rajput  <br>
Name : Penan Rajput <br>
Email : <penanrajput1998@gmail.com> <br>
[Follow on LinkedIn ](https://www.linkedin.com/in/penanrajput)




![Alt Text](src/Cypher1.png)

# CYPHER Queries Cheatsheet

#1. Database

Cypher does not support Database, still can be used

    1. Show Databases

    ```
    show databases;
    ```

    2. Use particular database (not working)

    ```sql
    use <database_name>
    ```

    3. to view current database (not working)
    
    ```sql
    :db current
    ```


    4. Show All tables within current database = To show all labels (which represent node types):
    ```sql
    CALL db.labels()
    YIELD label
    RETURN label;
    ```


#1. CREATE/INSERT NODE in Cypher = CREATE TABLE in SQL

    i. CREATE 1 NODE

    ```sql
    
    CREATE (:Employee 
                {
                    id:1, 
                    first_name:"Penan",
                    last_name:"Rajput", 
                    age:20, 
                    salary:100000
                }
            )
    ```

    ii. CREATE 1 NODE and RETURN IT

    ```sql
    CREATE (:Employee 
            {
                id:1, 
                first_name:"Penan",
                last_name:"Rajput", 
                age:20, 
                salary:100000
            }
        )
    ```
    ii. +1 row

    ```sql
    # STRUCTURE
    CREATE (
                :Table_Name {id:1}
            ), 
            (
                :Table_Name {id:2}
            )
    ```
    ```sql
    CREATE (:Employee 
            {
                id:1, 
                first_name:"Penan",
                last_name:"Rajput", 
                age:20, 
                salary:100000
            }
        )
    ```
    iii. 1 column with pre-defined value
    ```sql
    MATCH (p:Employee) SET p.COUNTRY = "IND";
    ```

    iv. set value to particular node
    ```sql
    MATCH (n:Employee {id:2}) 
    SET n.salary = "20000";
    ```

3. RETURN in Cypher Queries = SELECT in SQL

    i. RETURN ALL COLUMNS
    ```sql
    MATCH(n:Employee) RETURN n;
    ```

    ```sql
    MATCH(n:Employee) RETURN properties(n);
    ```

    ii. RETURN +1 COLUMN 

    ```sql
    MATCH(n:Employee) RETURN n, properties(n);
    ```

    iii. RETURN +1 COLUMN 
    ```sql
    MATCH(n:Employee) RETURN n, properties(n);
    ```

4. MATCH + WHERE
    i.


