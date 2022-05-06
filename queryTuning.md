
# Tuning query 
## I-The rules:
### 1.1 Only Select the data that you really need: 
+ avoid ```SELECT *```
  - Exclude fields or columns that do not need in the query
  - Name the field in select:
  - ```SQL
     SELECT driverslicensenr, name                                   
     FROM Drivers                                             
     WHERE EXISTS                                              
    (SELECT 'check'                                              
       FROM Fines                                               
    WHERE fines.driverslicensenr = drivers.driverslicensenr); 
    ```
+ avoid using ``` DISTINCT ```
  - **DISTINCT** will increasing query time if you don't need, the best is not use,especially the **unique** column
  - Limit the data returning by using 
  ```SQL
  TOP
  LIMIT
  ROWNUM
  ```
### 1.2 LIMIT the use of nested queries
The use of subQuery is not effective if these subQueries are uncorrelated (subQueries use value from parent query)
Example: 
```SQL
SELECT c.Name, 
       c.City,
       (SELECT CompanyName FROM Company WHERE ID = c.CompanyID) AS CompanyName 
FROM Customer c
```
Explain:
The inner query 
```SQL 
SELECT CompanyName…
```
is running for each row returns by outer query ```SELECT c.Name…``` .But why go through **Company** multiple times for each row process by outer query?

A more efficient SQL performance tuning technique would be to refactor the associative subQuery with **Join**:
```SQL
SELECT c.Name, 
       c.City, 
       co.CompanyName 
FROM Customer c 
	LEFT JOIN Company co
		ON c.CompanyID = co.CompanyID
```
In this case we only go through **Company** just only one time, when we start and ```JOIN``` that whit ```CUSTOMER```.Therefore we can choose the value that we actually need only one time.
### 1.3 NO "HAVING CLAUSE" when only "WHERE" is enough
```HAVING``` often go with ```GROUPBY```to consider the grouping limit condition return.However if you  use this clause in your Query,Index will not be used, that will cause slow down the query.
- Query_01:
```SQL
SELECT state, COUNT(*)
FROM Drivers
WHERE state IN ('GA', 'TX')
GROUP BY state
ORDER BY state
```
- Query_02:
```SQL
SELECT state, COUNT(*)
FROM Drivers
GROUP BY state
HAVING state IN ('GA', 'TX')
ORDER BY state
```
The two queries above will return the same data sets but  ```Query_01``` will be faster than ```Query_02``` because ```Query_01``` set the condition to limit the result returning, while
```Query_02``` will select all the result then it will use ```HAVING``` to reject other state
"GA" and  "TX"

### 1.4 Using ```clustering indexes``` to join
Consider the two queries:
- query_a
```SQL
SELECT Employee.ssnum
FROM Employee, Student
WHERE Employee.name = Student.name
```
- query_b:
```SQL
SELECT Employee.ssnum
FROM Employee, Student
WHERE Employee.ssnum = Student.ssnum
```
- Conclude:
  - ```query_b``` will be faster than ```query_a``` because join base in two cluster indexes will be accepted to merge join => The query  efficient will be better
  - Compare ```INT``` will be faster than compare ```VARCHAR```


### 1.5 Note about some system problems:
- ```LIKE``` AND```**INDEX```
  - when using ```LIKE``` operator, index will not be used if pattern contain ```%``` or ```_```. Furthermore, this query may access multiple records unnecessary.
- ```OR``` and ```INDEX```
  - Some System never use ```Index`` while using ```OR``` operator.The reason is indexes is used to locate or lookup data quickly without lookup all rows in the database

  ```SQL
  SELECT driverslicensenr, name
  FROM Drivers
  WHERE driverslicensenr = 123456
    OR driverslicensenr = 678910
    OR driverslicensenr = 345678;
  ```    
  Can replace to:
  ```SQL
  SELECT driverslicensenr, name
    FROM Drivers
  WHERE driverslicensenr IN (123456, 678910, 345678);
  ```
- ```NOT``` and ```INDEX```
  - Similar to ```OR```, Index will not be used with ```NOT``` operator 
  Instead of 
  ```SQL
  SELECT driverslicensenr, name
  FROM Drivers
  WHERE NOT (year > 1980);
  ```
  Replace to:
  ```SQL
  SELECT driverslicensenr, name
  FROM Drivers
  WHERE year <= 1980;
  ```
- ```ORDER``` in ```FROM``` Clause is not suitable and will slow your query except query ```JOIN``` **many table** 

