> # SQL (Structured Query Language) ek powerful language hai jo databases ko manage karne aur manipulate karne ke liye use hota hai. Agar aap SQL seekhna chahte hain, toh main aapko step-by-step sequence mein sab kuch samjhaunga. Yeh sequence beginner-friendly hai aur aapko basics se advanced concepts tak le jayega.

### SQL Seekhne ka Sequence:

---

#### 1. **Introduction to SQL**
   - SQL kya hai?
   - SQL ka use kahan hota hai?
   - Relational Databases ka concept samjhein.

---

#### 2. **Database and Tables**
   - Database kya hota hai?
   - Table kya hota hai?
   - **CREATE DATABASE**: Naya database banane ka syntax.
   - **CREATE TABLE**: Naya table banane ka syntax.
   - **Data Types**: Table mein columns ke liye data types (INT, VARCHAR, DATE, etc.).

---

#### 3. **Basic SQL Commands**
   - **INSERT INTO**: Table mein data insert karna.
   - **SELECT**: Table se data retrieve karna.
   - **UPDATE**: Table mein existing data ko modify karna.
   - **DELETE**: Table se data delete karna.

---

#### 4. **SQL Clauses**
   - **WHERE**: Specific conditions ke hisab se data filter karna.
   - **ORDER BY**: Data ko ascending ya descending order mein sort karna.
   - **GROUP BY**: Data ko groups mein organize karna.
   - **HAVING**: GROUP BY ke saath use hone wali condition.

---

#### 5. **SQL Operators**
   - **Arithmetic Operators**: +, -, *, /, %
   - **Comparison Operators**: =, !=, >, <, >=, <=
   - **Logical Operators**: AND, OR, NOT
   - **IN Operator**: Multiple values ke saath compare karna.
   - **BETWEEN Operator**: Range ke beech mein values filter karna.
   - **LIKE Operator**: Pattern matching ke liye.

---

#### 6. **Distinct Keyword**
   - **DISTINCT**: Unique values retrieve karna.

---

#### 7. **Aggregate Functions**
   - **COUNT**: Rows ki counting karna.
   - **SUM**: Column ke values ka sum nikalna.
   - **AVG**: Column ke values ka average nikalna.
   - **MIN**: Column mein se minimum value nikalna.
   - **MAX**: Column mein se maximum value nikalna.

---

#### 8. **Joins**
   - **INNER JOIN**: Common records ko combine karna.
   - **LEFT JOIN**: Left table ke saare records aur right table ke matching records.
   - **RIGHT JOIN**: Right table ke saare records aur left table ke matching records.
   - **FULL JOIN**: Dono tables ke saare records.

---

#### 9. **Constraints**
   - **PRIMARY KEY**: Unique identifier for each record.
   - **FOREIGN KEY**: Do tables ke beech mein relationship banane ke liye.
   - **NOT NULL**: Column mein NULL values allow nahi karna.
   - **UNIQUE**: Column mein unique values ensure karna.
   - **CHECK**: Column mein specific condition enforce karna.
   - **DEFAULT**: Column mein default value set karna.

---

#### 10. **Indexes**
   - **CREATE INDEX**: Table ke data ko quickly retrieve karne ke liye index banana.
   - **DROP INDEX**: Index ko delete karna.

---

#### 11. **Advanced SQL Concepts**
   - **Subqueries**: Query ke andar query.
   - **Views**: Virtual tables create karna.
   - **Stored Procedures**: Reusable SQL code blocks.
   - **Triggers**: Automatic actions perform karna (e.g., INSERT, UPDATE, DELETE ke baad).

---

#### 12. **Normalization**
   - Database normalization kya hai?
   - 1NF, 2NF, 3NF samjhein.

---

#### 13. **Transactions**
   - **BEGIN TRANSACTION**: Transaction start karna.
   - **COMMIT**: Changes save karna.
   - **ROLLBACK**: Changes undo karna.

---

#### 14. **Practice and Projects**
   - Small projects banayein (e.g., Student Management System, Library Management System).
   - Online platforms par SQL queries practice karein (e.g., LeetCode, HackerRank).

---

#### 20. **SQL String Functions **

- **CONCAT()**: Do ya do se zyada strings ko combine karta hai.
- **CONCAT_WS()**: Strings ko separator ke saath combine karta hai.
- **LENGTH() / LEN()**: String ki length return karta hai.
- **UPPER() / UCASE()**: String ko uppercase mein convert karta hai.
- **LOWER() / LCASE()**: String ko lowercase mein convert karta hai.
- **TRIM()**: String ke start aur end se extra spaces remove karta hai.
- **LTRIM()**: String ke start se extra spaces remove karta hai.
- **RTRIM()**: String ke end se extra spaces remove karta hai.
- **SUBSTRING() / SUBSTR()**: String ka ek part (substring) extract karta hai.
- **REPLACE()**: String mein specific text ko replace karta hai.
- **REVERSE()**: String ko reverse karta hai.
- **LEFT()**: String ke left side se specific characters extract karta hai.
- **RIGHT()**: String ke right side se specific characters extract karta hai.
- **CHAR_LENGTH()**: String mein characters ki count return karta hai.
- **LOCATE() / INSTR() / CHARINDEX()**: String mein substring ka position find karta hai.
- **LPAD()**: String ko left side se specific length tak pad karta hai.
- **RPAD()**: String ko right side se specific length tak pad karta hai.
- **REPEAT()**: String ko specific number of times repeat karta hai.
- **SPACE()**: Specific number of spaces return karta hai.
- **STRCMP()**: Do strings ko compare karta hai aur result return karta hai.

---

### **Examples in Short:**

- `CONCAT('Hello', 'World')` → `HelloWorld`
- `LENGTH('Hello')` → `5`
- `UPPER('hello')` → `HELLO`
- `TRIM('   Hello   ')` → `Hello`
- `SUBSTRING('Hello World', 7, 5)` → `World`
- `REPLACE('Hello World', 'World', 'SQL')` → `Hello SQL`
- `REVERSE('Hello')` → `olleH`
- `LEFT('Hello World', 5)` → `Hello`
- `RIGHT('Hello World', 5)` → `World`
- `LOCATE('World', 'Hello World')` → `7`
- `LPAD('Hello', 10, '*')` → `*****Hello`
- `REPEAT('Hello ', 3)` → `Hello Hello Hello `

---

### Example Queries:

1. **Table Creation:**
   ```sql
   CREATE TABLE Students (
       StudentID INT PRIMARY KEY,
       Name VARCHAR(50),
       Age INT,
       Grade CHAR(1)
   );
   ```

2. **Data Insertion:**
   ```sql
   INSERT INTO Students (StudentID, Name, Age, Grade)
   VALUES (1, 'Rahul', 20, 'A');
   ```

3. **Data Retrieval:**
   ```sql
   SELECT * FROM Students;
   ```

4. **Using WHERE Clause:**
   ```sql
   SELECT * FROM Students WHERE Age > 18;
   ```

5. **Using DISTINCT:**
   ```sql
   SELECT DISTINCT Grade FROM Students;
   ```

6. **Using Aggregate Functions:**
   ```sql
   SELECT AVG(Age) FROM Students;
   ```

7. **Using JOIN:**
   ```sql
   SELECT Students.Name, Courses.CourseName
   FROM Students
   INNER JOIN Courses ON Students.StudentID = Courses.StudentID;
   ```

---

### Resources for Learning SQL:
- **Books**: "SQL for Dummies", "Learning SQL" by Alan Beaulieu.
- **Websites**: W3Schools, SQLZoo, Mode Analytics.
- **YouTube Channels**: Programming with Mosh, freeCodeCamp.

<hr>

> # SQL commands ko categories mein divide kiya jaata hai, aur yeh categories hain: **DDL**, **DML**, **DCL**, aur **TCL**. Har category ke commands ka alag purpose hota hai. Main aapko har category ke baare mein detail mein bataunga aur usmein kaun-kaun se commands use hote hain.

---

### 1. **DDL (Data Definition Language)**  
DDL commands ka use database aur table ki structure define karne ke liye hota hai. Yeh commands directly database schema (structure) ko modify karte hain.

#### DDL Commands:
- **CREATE**: Database ya table create karne ke liye.
  ```sql
  CREATE TABLE Students (
      StudentID INT PRIMARY KEY,
      Name VARCHAR(50),
      Age INT
  );
  ```

- **ALTER**: Existing table mein changes karne ke liye (jaise column add/remove karna).
  ```sql
  ALTER TABLE Students ADD COLUMN Grade CHAR(1);
  ```

- **DROP**: Table ya database ko delete karne ke liye.
  ```sql
  DROP TABLE Students;
  ```

- **TRUNCATE**: Table ke saare data ko delete karne ke liye (structure remain karti hai).
  ```sql
  TRUNCATE TABLE Students;
  ```

- **RENAME**: Table ya column ka naam change karne ke liye.
  ```sql
  ALTER TABLE Students RENAME COLUMN Age TO StudentAge;
  ```

---

### 2. **DML (Data Manipulation Language)**  
DML commands ka use database mein existing data ko manipulate karne ke liye hota hai. Yeh commands data ko insert, update, delete, aur retrieve karne ke liye use hote hain.

#### DML Commands:
- **INSERT**: Table mein naya data insert karne ke liye.
  ```sql
  INSERT INTO Students (StudentID, Name, Age)
  VALUES (1, 'Rahul', 20);
  ```

- **UPDATE**: Table mein existing data ko modify karne ke liye.
  ```sql
  UPDATE Students SET Age = 21 WHERE StudentID = 1;
  ```

- **DELETE**: Table se specific data delete karne ke liye.
  ```sql
  DELETE FROM Students WHERE StudentID = 1;
  ```

- **SELECT**: Table se data retrieve karne ke liye.
  ```sql
  SELECT * FROM Students;
  ```

---

### 3. **DCL (Data Control Language)**  
DCL commands ka use database par control rakhte hue permissions aur access manage karne ke liye hota hai.

#### DCL Commands:
- **GRANT**: Users ko specific permissions dene ke liye.
  ```sql
  GRANT SELECT, INSERT ON Students TO 'User1';
  ```

- **REVOKE**: Users se permissions wapas lene ke liye.
  ```sql
  REVOKE INSERT ON Students FROM 'User1';
  ```

---

### 4. **TCL (Transaction Control Language)**  
TCL commands ka use database transactions ko manage karne ke liye hota hai. Transactions ek ya multiple SQL commands ka group hota hai jo ek unit ki tarah treat kiya jaata hai.

#### TCL Commands:
- **BEGIN TRANSACTION**: Transaction start karne ke liye.
  ```sql
  BEGIN TRANSACTION;
  ```

- **COMMIT**: Transaction ko permanently save karne ke liye.
  ```sql
  COMMIT;
  ```

- **ROLLBACK**: Transaction ko undo karne ke liye (agar kuch galat ho jaye).
  ```sql
  ROLLBACK;
  ```

- **SAVEPOINT**: Transaction mein ek point save karne ke liye, jahan tak rollback kiya ja sake.
  ```sql
  SAVEPOINT Savepoint1;
  ```

---

### Summary of Commands:

| **Category** | **Commands**                     | **Purpose**                                                                 |
|--------------|----------------------------------|-----------------------------------------------------------------------------|
| **DDL**      | CREATE, ALTER, DROP, TRUNCATE    | Database aur table ki structure define aur modify karna.                    |
| **DML**      | INSERT, UPDATE, DELETE, SELECT   | Database mein data manipulate karna (insert, update, delete, retrieve).     |
| **DCL**      | GRANT, REVOKE                    | Users ko permissions dena ya wapas lena.                                    |
| **TCL**      | BEGIN, COMMIT, ROLLBACK, SAVEPOINT | Transactions ko manage karna (start, save, undo).                           |

---

### Example of All Commands in Action:

1. **DDL**: Table create karna.
   ```sql
   CREATE TABLE Employees (
       EmployeeID INT PRIMARY KEY,
       Name VARCHAR(50),
       Salary DECIMAL(10, 2)
   );
   ```

2. **DML**: Data insert karna.
   ```sql
   INSERT INTO Employees (EmployeeID, Name, Salary)
   VALUES (1, 'Rahul', 50000);
   ```

3. **DCL**: User ko permission dena.
   ```sql
   GRANT SELECT ON Employees TO 'User1';
   ```

4. **TCL**: Transaction commit karna.
   ```sql
   COMMIT;
   ```

<hr>

 > # **SQL ka complete tree** **sabhi major concepts** aur **commands**

---

### **Updated SQL Tree Structure**

```
SQL (Structured Query Language)
├── **1. DDL (Data Definition Language)**
│   ├── CREATE
│   ├── ALTER
│   ├── DROP
│   ├── TRUNCATE
│   └── RENAME
│
├── **2. DML (Data Manipulation Language)**
│   ├── INSERT
│   ├── UPDATE
│   ├── DELETE
│   └── SELECT
│       ├── WHERE
│       ├── ORDER BY
│       ├── GROUP BY
│       ├── HAVING
│       ├── DISTINCT
│       ├── LIMIT (MySQL) / TOP (SQL Server)
│       └── Aggregate Functions
│           ├── COUNT
│           ├── SUM
│           ├── AVG
│           ├── MIN
│           └── MAX
│
├── **3. DCL (Data Control Language)**
│   ├── GRANT
│   └── REVOKE
│
├── **4. TCL (Transaction Control Language)**
│   ├── BEGIN TRANSACTION
│   ├── COMMIT
│   ├── ROLLBACK
│   └── SAVEPOINT
│
├── **5. Advanced Concepts**
│   ├── Joins
│   │   ├── INNER JOIN
│   │   ├── LEFT JOIN
│   │   ├── RIGHT JOIN
│   │   └── FULL JOIN
│   ├── Subqueries
│   ├── Views
│   ├── Stored Procedures
│   ├── Triggers
│   ├── Indexes
│   ├── UNION / UNION ALL
│   ├── CASE Statements
│   └── Window Functions (ROW_NUMBER, RANK, etc.)
│
├── **6. Database Design**
│   ├── Normalization
│   │   ├── 1NF (First Normal Form)
│   │   ├── 2NF (Second Normal Form)
│   │   └── 3NF (Third Normal Form)
│   └── Constraints
│       ├── PRIMARY KEY
│       ├── FOREIGN KEY
│       ├── NOT NULL
│       ├── UNIQUE
│       ├── CHECK
│       └── DEFAULT
│
└── **7. Miscellaneous**
    ├── Wildcard Characters (%, _)
    ├── NULL Values Handling (IS NULL, IS NOT NULL)
    ├── Aliases (AS)
    ├── Comments (--, /* */)
    └── Data Import/Export (BULK INSERT, LOAD DATA)
```

### **Tree ka Breakdown:**

1. **DDL**: Database aur table ki structure define aur modify karne ke liye.
2. **DML**: Data ko insert, update, delete, aur retrieve karne ke liye.
3. **DCL**: Users ko permissions dena ya wapas lena.
4. **TCL**: Transactions ko manage karna.
5. **Advanced Concepts**: Joins, subqueries, views, stored procedures, triggers, aur indexes.
6. **Database Design**: Normalization aur constraints ka use karke database design karna.

---

### **Example Recall:**

- Agar aapko **data retrieve** karna hai, toh `SELECT` command use karenge.
- Agar aapko **table structure** change karna hai, toh `ALTER` command use karenge.
- Agar aapko **user ko permission** deni hai, toh `GRANT` command use karenge.
- Agar aapko **transaction save** karna hai, toh `COMMIT` command use karenge.

---

### **Tips for Recall:**
- **DDL**: Structure define karne ke liye (CREATE, ALTER, DROP).
- **DML**: Data manipulate karne ke liye (INSERT, UPDATE, DELETE, SELECT).
- **DCL**: Permissions manage karne ke liye (GRANT, REVOKE).
- **TCL**: Transactions manage karne ke liye (BEGIN, COMMIT, ROLLBACK).

---

### **Kuch Additional Points Jo Pehle Chhoot Gaye The:**

1. **LIMIT / TOP**:
   - `LIMIT` (MySQL) ya `TOP` (SQL Server) ka use karke aap specific number of rows retrieve kar sakte hain.
   ```sql
   SELECT * FROM Students LIMIT 5; -- MySQL
   SELECT TOP 5 * FROM Students;   -- SQL Server
   ```

2. **UNION / UNION ALL**:
   - `UNION` do ya do se zyada queries ke results ko combine karta hai (unique values).
   - `UNION ALL` bhi same kaam karta hai, lekin duplicate values allow karta hai.
   ```sql
   SELECT Name FROM Students
   UNION
   SELECT Name FROM Teachers;
   ```

3. **CASE Statements**:
   - Conditional logic ke liye use hota hai.
   ```sql
   SELECT Name,
          CASE
              WHEN Age < 18 THEN 'Minor'
              ELSE 'Adult'
          END AS AgeGroup
   FROM Students;
   ```

4. **Window Functions**:
   - Advanced calculations ke liye use hota hai, jaise `ROW_NUMBER`, `RANK`, `DENSE_RANK`, etc.
   ```sql
   SELECT Name, Salary,
          ROW_NUMBER() OVER (ORDER BY Salary DESC) AS RowNum
   FROM Employees;
   ```

5. **Wildcard Characters**:
   - `%` aur `_` ka use `LIKE` operator ke saath pattern matching ke liye hota hai.
   ```sql
   SELECT * FROM Students WHERE Name LIKE 'R%'; -- Names starting with 'R'
   ```

6. **NULL Values Handling**:
   - `IS NULL` aur `IS NOT NULL` ka use NULL values ko handle karne ke liye hota hai.
   ```sql
   SELECT * FROM Students WHERE Age IS NULL;
   ```

7. **Aliases**:
   - `AS` keyword ka use columns ya tables ko temporary naam dene ke liye hota hai.
   ```sql
   SELECT Name AS StudentName FROM Students;
   ```

8. **Comments**:
   - SQL queries mein comments add karne ke liye `--` (single-line) ya `/* */` (multi-line) use hota hai.
   ```sql
   -- This is a single-line comment
   /* This is a
      multi-line comment */
   ```

9. **Data Import/Export**:
   - `BULK INSERT` (SQL Server) ya `LOAD DATA` (MySQL) ka use large datasets ko import/export karne ke liye hota hai.

---
