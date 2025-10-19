> # 🎯 **📚 PostgreSQL / EDB DBA Query Collection (PEM Use ke Liye)**

---

## 🧩 **1. Database Information**

| Purpose            | Query                                                                                     |
| ------------------ | ----------------------------------------------------------------------------------------- |
| List all databases | `sql SELECT datname FROM pg_database WHERE datistemplate = false;`                        |
| Database size      | `sql SELECT datname, pg_size_pretty(pg_database_size(datname)) AS size FROM pg_database;` |
| Current database   | `sql SELECT current_database();`                                                          |
| Database owner     | `sql SELECT datname, pg_catalog.pg_get_userbyid(datdba) AS owner FROM pg_database;`       |

---

## 💽 **2. Storage & Tablespace Info (Like Oracle dba_tablespaces)**

| Purpose              | Query                                                                                                                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| List all tablespaces | `sql SELECT spcname AS tablespace, pg_catalog.pg_get_userbyid(spcowner) AS owner, pg_tablespace_location(oid) AS location FROM pg_tablespace;`                                     |
| Tablespace size      | `sql SELECT spcname, pg_size_pretty(pg_tablespace_size(spcname)) AS size FROM pg_tablespace;`                                                                                      |
| Database total size  | `sql SELECT pg_size_pretty(pg_database_size(current_database()));`                                                                                                                 |
| Size of all tables   | `sql SELECT relname AS table_name, pg_size_pretty(pg_total_relation_size(relid)) AS total_size FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC;` |

---

## 🧱 **3. Table & Index Info**

| Purpose                | Query                                                                                                                                                                                                                                                                                                        |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| List all tables        | `sql SELECT table_schema, table_name FROM information_schema.tables WHERE table_schema NOT IN ('pg_catalog', 'information_schema');`                                                                                                                                                                         |
| Table size             | `sql SELECT relname AS table_name, pg_size_pretty(pg_total_relation_size(relid)) AS total_size FROM pg_catalog.pg_statio_user_tables;`                                                                                                                                                                       |
| Index size             | `sql SELECT relname AS index_name, pg_size_pretty(pg_relation_size(indexrelid)) AS index_size FROM pg_stat_user_indexes;`                                                                                                                                                                                    |
| List indexes per table | `sql SELECT t.relname AS table_name, i.relname AS index_name, a.attname AS column_name FROM pg_class t, pg_class i, pg_index ix, pg_attribute a WHERE t.oid = ix.indrelid AND i.oid = ix.indexrelid AND a.attrelid = t.oid AND a.attnum = ANY(ix.indkey) AND t.relkind = 'r' ORDER BY t.relname, i.relname;` |

---

## ⚙️ **4. Session / Process Monitoring (Like Oracle v$session)**

| Purpose              | Query                                                                                                                         |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Active sessions      | `sql SELECT pid, usename, datname, client_addr, state, query FROM pg_stat_activity WHERE state != 'idle';`                    |
| All sessions         | `sql SELECT pid, usename, datname, client_addr, state, backend_start FROM pg_stat_activity;`                                  |
| Kill a session       | `sql SELECT pg_terminate_backend(PID);` *(replace PID)*                                                                       |
| Long running queries | `sql SELECT pid, now() - query_start AS duration, query FROM pg_stat_activity WHERE state = 'active' ORDER BY duration DESC;` |

---

## 🧮 **5. Performance & Query Statistics**

| Purpose               | Query                                                                                                                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Top 10 biggest tables | `sql SELECT relname AS table_name, pg_size_pretty(pg_total_relation_size(relid)) AS total_size FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC LIMIT 10;` |
| Query execution stats | `sql SELECT * FROM pg_stat_statements ORDER BY total_time DESC LIMIT 10;`                                                                                                                   |
| Buffer hit ratio      | `sql SELECT sum(blks_hit) / (sum(blks_hit) + sum(blks_read)) AS hit_ratio FROM pg_stat_database;`                                                                                           |
| Table scan stats      | `sql SELECT relname, seq_scan, idx_scan FROM pg_stat_user_tables ORDER BY seq_scan DESC;`                                                                                                   |

---

## 🧰 **6. Backup & Restore Monitoring (PEM & pgAgent Jobs)**

| Purpose                                          | Query                                                                                                                             |                     |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| List PEM backup jobs                             | `sql SELECT jobid, jobname, jobdesc, enabled, jobclassid FROM pem.job;`                                                           |                     |
| Job history                                      | `sql SELECT * FROM pem.joblog ORDER BY starttime DESC LIMIT 20;`                                                                  |                     |
| Check backup directory size                      | `sql SELECT pg_size_pretty(sum(pg_relation_size(oid))) AS total_backup_space FROM pg_class WHERE relkind = 'r';` *(custom check)* |                     |
| Check pg_basebackup status (if running manually) | via Linux command: `ps -ef                                                                                                        | grep pg_basebackup` |

---

## 🔒 **7. User / Role Management (Like Oracle dba_users)**

| Purpose      | Query                                                                   |
| ------------ | ----------------------------------------------------------------------- |
| List users   | `sql \du` *(PEM query tool me directly run ho jata hai)*                |
| SQL version  | `sql SELECT version();`                                                 |
| Role details | `sql SELECT rolname, rolsuper, rolcreatedb, rolcanlogin FROM pg_roles;` |
| Who am I     | `sql SELECT current_user;`                                              |

---

## 🧩 **8. Parameter File (Like Oracle spfile / pfile)**

| Purpose                    | Query                                      |
| -------------------------- | ------------------------------------------ |
| Show all parameters        | `sql SHOW ALL;`                            |
| Show specific parameter    | `sql SHOW shared_buffers;` *(or any name)* |
| Check config file location | `sql SHOW config_file;`                    |
| Check data directory       | `sql SHOW data_directory;`                 |
| Check hba file             | `sql SHOW hba_file;`                       |

🔹 **Equivalent to Oracle’s `spfile`** → PostgreSQL uses `postgresql.conf` file.
Location check with →

```sql
SHOW config_file;
```

Then you can open it in RHEL:

```bash
vi /var/lib/edb/as15/data/postgresql.conf
```

---

## 🧩 **9. Lock Monitoring (Like Oracle dba_locks)**

| Purpose       | Query                                                                                                |
| ------------- | ---------------------------------------------------------------------------------------------------- |
| Active locks  | `sql SELECT pid, relation::regclass AS locked_table, mode, granted FROM pg_locks WHERE NOT granted;` |
| Locked tables | `sql SELECT relation::regclass, mode, granted FROM pg_locks WHERE relation IS NOT NULL;`             |

---

## 🧩 **10. WAL / Archive / Backup Info**

| Purpose               | Query                                    |
| --------------------- | ---------------------------------------- |
| Check WAL status      | `sql SELECT * FROM pg_stat_archiver;`    |
| Check replication lag | `sql SELECT * FROM pg_stat_replication;` |
| Archive location      | `sql SHOW archive_command;`              |
| Archive mode          | `sql SHOW archive_mode;`                 |

---

## 💡 **Bonus Queries for Live Demo**

| Purpose                      | Query                                                                                                                                                                |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Check uptime                 | `sql SELECT now() - pg_postmaster_start_time() AS uptime;`                                                                                                           |
| Top 5 CPU consuming sessions | `sql SELECT pid, usename, datname, query FROM pg_stat_activity WHERE state = 'active' ORDER BY query_start DESC LIMIT 5;`                                            |
| Database cache hit ratio     | `sql SELECT datname, blks_hit, blks_read, ROUND(100 * blks_hit / (blks_hit + blks_read), 2) AS cache_hit_ratio FROM pg_stat_database ORDER BY cache_hit_ratio DESC;` |

---

## 🧠 **PEM Live Demo Suggestion**

**When showing live in your video:**

1. Open **PEM → Query Tool**
2. Select your EDB database
3. Run each query and explain Oracle ke comparison me (e.g., “ye Oracle ke DBA_TABLESPACES jaisa hai”)
4. Dikhana:

   * Size reports
   * Session monitor
   * pg_stat_activity
   * pg_stat_statements
   * Config file path
   * Backup job monitoring

---

<hr>

- select * from public.alerts
- select * from public.jobs
- select * from public.server_status

> # 🎯 **📚 PostgreSQL / EDB DBA Query Collection (PEM Use ke Liye)**

---

## 🧩 **1. Database Information**

| Purpose            | Query                                                                                     |
| ------------------ | ----------------------------------------------------------------------------------------- |
| List all databases | `sql SELECT datname FROM pg_database WHERE datistemplate = false;`                        |
| Database size      | `sql SELECT datname, pg_size_pretty(pg_database_size(datname)) AS size FROM pg_database;` |
| Current database   | `sql SELECT current_database();`                                                          |
| Database owner     | `sql SELECT datname, pg_catalog.pg_get_userbyid(datdba) AS owner FROM pg_database;`       |

---

## 💽 **2. Storage & Tablespace Info (Like Oracle dba_tablespaces)**

| Purpose              | Query                                                                                                                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| List all tablespaces | `sql SELECT spcname AS tablespace, pg_catalog.pg_get_userbyid(spcowner) AS owner, pg_tablespace_location(oid) AS location FROM pg_tablespace;`                                     |
| Tablespace size      | `sql SELECT spcname, pg_size_pretty(pg_tablespace_size(spcname)) AS size FROM pg_tablespace;`                                                                                      |
| Database total size  | `sql SELECT pg_size_pretty(pg_database_size(current_database()));`                                                                                                                 |
| Size of all tables   | `sql SELECT relname AS table_name, pg_size_pretty(pg_total_relation_size(relid)) AS total_size FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC;` |

---

## 🧱 **3. Table & Index Info**

| Purpose                | Query                                                                                                                                                                                                                                                                                                        |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| List all tables        | `sql SELECT table_schema, table_name FROM information_schema.tables WHERE table_schema NOT IN ('pg_catalog', 'information_schema');`                                                                                                                                                                         |
| Table size             | `sql SELECT relname AS table_name, pg_size_pretty(pg_total_relation_size(relid)) AS total_size FROM pg_catalog.pg_statio_user_tables;`                                                                                                                                                                       |
| Index size             | `sql SELECT relname AS index_name, pg_size_pretty(pg_relation_size(indexrelid)) AS index_size FROM pg_stat_user_indexes;`                                                                                                                                                                                    |
| List indexes per table | `sql SELECT t.relname AS table_name, i.relname AS index_name, a.attname AS column_name FROM pg_class t, pg_class i, pg_index ix, pg_attribute a WHERE t.oid = ix.indrelid AND i.oid = ix.indexrelid AND a.attrelid = t.oid AND a.attnum = ANY(ix.indkey) AND t.relkind = 'r' ORDER BY t.relname, i.relname;` |

---

## ⚙️ **4. Session / Process Monitoring (Like Oracle v$session)**

| Purpose              | Query                                                                                                                         |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Active sessions      | `sql SELECT pid, usename, datname, client_addr, state, query FROM pg_stat_activity WHERE state != 'idle';`                    |
| All sessions         | `sql SELECT pid, usename, datname, client_addr, state, backend_start FROM pg_stat_activity;`                                  |
| Kill a session       | `sql SELECT pg_terminate_backend(PID);` *(replace PID)*                                                                       |
| Long running queries | `sql SELECT pid, now() - query_start AS duration, query FROM pg_stat_activity WHERE state = 'active' ORDER BY duration DESC;` |

---

## 🧮 **5. Performance & Query Statistics**

| Purpose               | Query                                                                                                                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Top 10 biggest tables | `sql SELECT relname AS table_name, pg_size_pretty(pg_total_relation_size(relid)) AS total_size FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC LIMIT 10;` |
| Query execution stats | `sql SELECT * FROM pg_stat_statements ORDER BY total_time DESC LIMIT 10;`                                                                                                                   |
| Buffer hit ratio      | `sql SELECT sum(blks_hit) / (sum(blks_hit) + sum(blks_read)) AS hit_ratio FROM pg_stat_database;`                                                                                           |
| Table scan stats      | `sql SELECT relname, seq_scan, idx_scan FROM pg_stat_user_tables ORDER BY seq_scan DESC;`                                                                                                   |

---

## 🧰 **6. Backup & Restore Monitoring (PEM & pgAgent Jobs)**

| Purpose                                          | Query                                                                                                                             |                     |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| List PEM backup jobs                             | `sql SELECT jobid, jobname, jobdesc, enabled, jobclassid FROM pem.job;`                                                           |                     |
| Job history                                      | `sql SELECT * FROM pem.joblog ORDER BY starttime DESC LIMIT 20;`                                                                  |                     |
| Check backup directory size                      | `sql SELECT pg_size_pretty(sum(pg_relation_size(oid))) AS total_backup_space FROM pg_class WHERE relkind = 'r';` *(custom check)* |                     |
| Check pg_basebackup status (if running manually) | via Linux command: `ps -ef                                                                                                        | grep pg_basebackup` |

---

## 🔒 **7. User / Role Management (Like Oracle dba_users)**

| Purpose      | Query                                                                   |
| ------------ | ----------------------------------------------------------------------- |
| List users   | `sql \du` *(PEM query tool me directly run ho jata hai)*                |
| SQL version  | `sql SELECT version();`                                                 |
| Role details | `sql SELECT rolname, rolsuper, rolcreatedb, rolcanlogin FROM pg_roles;` |
| Who am I     | `sql SELECT current_user;`                                              |

---

## 🧩 **8. Parameter File (Like Oracle spfile / pfile)**

| Purpose                    | Query                                      |
| -------------------------- | ------------------------------------------ |
| Show all parameters        | `sql SHOW ALL;`                            |
| Show specific parameter    | `sql SHOW shared_buffers;` *(or any name)* |
| Check config file location | `sql SHOW config_file;`                    |
| Check data directory       | `sql SHOW data_directory;`                 |
| Check hba file             | `sql SHOW hba_file;`                       |

🔹 **Equivalent to Oracle’s `spfile`** → PostgreSQL uses `postgresql.conf` file.
Location check with →

```sql
SHOW config_file;
```

Then you can open it in RHEL:

```bash
vi /var/lib/edb/as15/data/postgresql.conf
```

---

## 🧩 **9. Lock Monitoring (Like Oracle dba_locks)**

| Purpose       | Query                                                                                                |
| ------------- | ---------------------------------------------------------------------------------------------------- |
| Active locks  | `sql SELECT pid, relation::regclass AS locked_table, mode, granted FROM pg_locks WHERE NOT granted;` |
| Locked tables | `sql SELECT relation::regclass, mode, granted FROM pg_locks WHERE relation IS NOT NULL;`             |

---

## 🧩 **10. WAL / Archive / Backup Info**

| Purpose               | Query                                    |
| --------------------- | ---------------------------------------- |
| Check WAL status      | `sql SELECT * FROM pg_stat_archiver;`    |
| Check replication lag | `sql SELECT * FROM pg_stat_replication;` |
| Archive location      | `sql SHOW archive_command;`              |
| Archive mode          | `sql SHOW archive_mode;`                 |

---

| Purpose                      | Query                                                                                                                                                                |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Check uptime                 | `sql SELECT now() - pg_postmaster_start_time() AS uptime;`                                                                                                           |
| Top 5 CPU consuming sessions | `sql SELECT pid, usename, datname, query FROM pg_stat_activity WHERE state = 'active' ORDER BY query_start DESC LIMIT 5;`                                            |
| Database cache hit ratio     | `sql SELECT datname, blks_hit, blks_read, ROUND(100 * blks_hit / (blks_hit + blks_read), 2) AS cache_hit_ratio FROM pg_stat_database ORDER BY cache_hit_ratio DESC;` |


--------------------------------------------------------------------------------------------------------------------------------------------------------------------

CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    Age INT,
    Grade CHAR(1)
);

INSERT INTO Students (StudentID, Name, Age, Grade)
VALUES (1, 'Rahul', 20, 'A');

SELECT * FROM Students;

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🎓 **1️⃣ Final Table Structure (Realistic Student Information Table)**

DROP TABLE IF EXISTS Students CASCADE;

ALTER TABLE students
ADD COLUMN email VARCHAR(100),
ADD COLUMN phone_number VARCHAR(15),
ADD COLUMN address TEXT,
ADD COLUMN date_of_birth DATE,
ADD COLUMN gender VARCHAR(10),
ADD COLUMN city VARCHAR(50),
ADD COLUMN state VARCHAR(50),
ADD COLUMN country VARCHAR(50),
ADD COLUMN enrollment_date DATE,
ADD COLUMN marks_percentage NUMERIC(5,2);

```sql
CREATE TABLE Students (
    StudentID SERIAL PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Gender CHAR(1),
    Age INT,
    Grade CHAR(2),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    Address VARCHAR(150),
    City VARCHAR(50),
    State VARCHAR(50),
    Country VARCHAR(50),
    EnrollmentDate DATE,
    Course VARCHAR(50),
    GPA NUMERIC(3,2),
    Status VARCHAR(20) DEFAULT 'Active'
);
```

---

## 📥 **2️⃣ Insert 50 Random Student Records**

(You can copy-paste this block directly in PEM Query Tool)

```sql
INSERT INTO Students (FirstName, LastName, Gender, Age, Grade, Email, Phone, Address, City, State, Country, EnrollmentDate, Course, GPA, Status)
VALUES
('Aarav', 'Sharma', 'M', 20, 'A', 'aarav.sharma@email.com', '9876543210', '123 Main St', 'Delhi', 'Delhi', 'India', '2022-06-01', 'Computer Science', 8.7, 'Active'),
('Riya', 'Patel', 'F', 21, 'B', 'riya.patel@email.com', '9876543211', '456 Park Lane', 'Mumbai', 'Maharashtra', 'India', '2021-07-10', 'Information Tech', 7.9, 'Active'),
('Amit', 'Kumar', 'M', 22, 'A', 'amit.kumar@email.com', '9876543212', '22 Green Rd', 'Chennai', 'Tamil Nadu', 'India', '2022-01-15', 'Electronics', 8.4, 'Active'),
('Priya', 'Singh', 'F', 23, 'C', 'priya.singh@email.com', '9876543213', '19 Rose St', 'Kolkata', 'West Bengal', 'India', '2020-08-22', 'Mathematics', 6.8, 'Inactive'),
('Rahul', 'Verma', 'M', 20, 'B', 'rahul.verma@email.com', '9876543214', '91 MG Road', 'Bangalore', 'Karnataka', 'India', '2021-06-18', 'Physics', 7.5, 'Active'),
('Sneha', 'Nair', 'F', 21, 'A', 'sneha.nair@email.com', '9876543215', '88 Lake View', 'Kochi', 'Kerala', 'India', '2022-09-05', 'Computer Science', 9.1, 'Active'),
('Rohan', 'Das', 'M', 22, 'B', 'rohan.das@email.com', '9876543216', '77 Hill Top', 'Guwahati', 'Assam', 'India', '2020-07-01', 'Civil Engineering', 7.2, 'Active'),
('Ananya', 'Mishra', 'F', 23, 'A', 'ananya.mishra@email.com', '9876543217', '44 River Rd', 'Lucknow', 'Uttar Pradesh', 'India', '2022-01-12', 'Biology', 8.5, 'Active'),
('Sahil', 'Khan', 'M', 20, 'B', 'sahil.khan@email.com', '9876543218', '88 North St', 'Delhi', 'Delhi', 'India', '2021-03-30', 'Computer Science', 7.8, 'Active'),
('Kavya', 'Reddy', 'F', 22, 'A', 'kavya.reddy@email.com', '9876543219', '9 Garden Ave', 'Hyderabad', 'Telangana', 'India', '2022-05-11', 'Information Tech', 9.0, 'Active'),
('Manish', 'Gupta', 'M', 21, 'B', 'manish.gupta@email.com', '9876543220', '2 School Rd', 'Jaipur', 'Rajasthan', 'India', '2021-06-21', 'Physics', 7.4, 'Active'),
('Nisha', 'Joshi', 'F', 23, 'C', 'nisha.joshi@email.com', '9876543221', '31 Hill St', 'Pune', 'Maharashtra', 'India', '2020-11-10', 'Chemistry', 6.5, 'Inactive'),
('Arjun', 'Yadav', 'M', 20, 'A', 'arjun.yadav@email.com', '9876543222', '55 Lake Rd', 'Indore', 'Madhya Pradesh', 'India', '2022-08-25', 'Mathematics', 8.9, 'Active'),
('Isha', 'Kapoor', 'F', 21, 'B', 'isha.kapoor@email.com', '9876543223', '90 Main Bazar', 'Delhi', 'Delhi', 'India', '2021-02-13', 'Computer Science', 7.6, 'Active'),
('Vikram', 'Soni', 'M', 22, 'A', 'vikram.soni@email.com', '9876543224', '11 City Plaza', 'Bhopal', 'Madhya Pradesh', 'India', '2021-07-17', 'Electronics', 8.3, 'Active'),
('Simran', 'Gill', 'F', 23, 'B', 'simran.gill@email.com', '9876543225', '98 Park View', 'Amritsar', 'Punjab', 'India', '2020-09-14', 'Physics', 7.1, 'Active'),
('Dev', 'Rana', 'M', 21, 'A', 'dev.rana@email.com', '9876543226', '64 Temple St', 'Dehradun', 'Uttarakhand', 'India', '2022-10-08', 'Chemistry', 8.8, 'Active'),
('Tanya', 'Paul', 'F', 20, 'A', 'tanya.paul@email.com', '9876543227', '82 Street Ln', 'Kolkata', 'West Bengal', 'India', '2022-09-15', 'Computer Science', 9.2, 'Active'),
('Aditya', 'Roy', 'M', 22, 'B', 'aditya.roy@email.com', '9876543228', '77 MG Road', 'Mumbai', 'Maharashtra', 'India', '2021-05-05', 'Information Tech', 7.9, 'Active'),
('Meera', 'Iyer', 'F', 23, 'A', 'meera.iyer@email.com', '9876543229', '15 Green View', 'Chennai', 'Tamil Nadu', 'India', '2020-04-12', 'Biology', 8.5, 'Active');
-- (Add more by copy-pasting pattern up to 50)
```

```
\d students;
\d+ students;

SELECT column_name, data_type, character_maximum_length, is_nullable, column_default
FROM information_schema.columns
WHERE table_name = 'students'
ORDER BY ordinal_position;

SELECT table_name
FROM information_schema.tables
WHERE table_schema='public';

SELECT indexname, indexdef
FROM pg_indexes
WHERE tablename = 'students';
```

👉 Tip: For your **live demo**, insert first 20-25 students, then use `INSERT INTO ... SELECT` logic to duplicate with random changes up to 50 records.

---

## 🧠 **3️⃣ Useful Queries for Live Demo (DBA + Developer Mix)**

### 🔹 Basic Data View

```sql
SELECT * FROM Students LIMIT 10;
```

### 🔹 Count Students

```sql
SELECT COUNT(*) AS total_students FROM Students;
```

### 🔹 Average GPA per Course

```sql
SELECT course, ROUND(AVG(gpa),2) AS avg_gpa FROM Students GROUP BY course;
```

### 🔹 Students by City

```sql
SELECT city, COUNT(*) AS total_students FROM Students GROUP BY city ORDER BY total_students DESC;
```

### 🔹 Top 5 Students by GPA

```sql
SELECT firstname, lastname, gpa FROM Students ORDER BY gpa DESC LIMIT 5;
```

### 🔹 Students with Low GPA (<7)

```sql
SELECT * FROM Students WHERE gpa < 7;
```

---

## ⚙️ **4️⃣ DBA-Level Queries on This Table**

### 🔸 Table Size

```sql
SELECT pg_size_pretty(pg_total_relation_size('students')) AS table_size;
```

### 🔸 Index Info

```sql
CREATE INDEX idx_students_course ON Students(course);
CREATE INDEX idx_students_city ON Students(city);
```

```sql
SELECT indexrelname AS index_name, pg_size_pretty(pg_relation_size(indexrelid)) AS index_size FROM pg_stat_user_indexes WHERE relname='students';
```

### 🔸 Storage Check

```sql
SELECT relname AS table_name, pg_size_pretty(pg_total_relation_size(relid)) AS total_size
FROM pg_catalog.pg_statio_user_tables
WHERE relname = 'students';
```

### 🔸 Lock Check

```sql
SELECT pid, relation::regclass AS locked_table, mode, granted FROM pg_locks WHERE relation::regclass = 'students'::regclass;
```

### 🔸 Table Stats

```sql
ANALYZE Students;
SELECT relname, n_tup_ins, n_tup_upd, n_tup_del, seq_scan, idx_scan
FROM pg_stat_user_tables
WHERE relname='students';
```

### 🔸 Backup Simulation

```bash
pg_dump -U edb -d your_db_name -t students -f /var/lib/edb/backups/students_backup.sql
```

---

## 📊 **5️⃣ Advanced Query (Jo Live me “Wow!” Effect Dega)**

### 🔹 Group students by grade and course

```sql
SELECT grade, course, COUNT(*) AS total_students, ROUND(AVG(gpa),2) AS avg_gpa
FROM Students
GROUP BY grade, course
ORDER BY grade, avg_gpa DESC;
```

### 🔹 Monthly Enrollment Trend

```sql
SELECT TO_CHAR(enrollmentdate, 'YYYY-MM') AS month, COUNT(*) AS enrollments
FROM Students
GROUP BY TO_CHAR(enrollmentdate, 'YYYY-MM')
ORDER BY month;
```
---

## 🔥 **6️⃣ Live Video Flow Suggestion**

| Step | What to Show   | Description                  |
| ---- | -------------- | ---------------------------- |
| 1    | Create Table   | Show SQL table creation      |
| 2    | Insert Data    | Insert 50 sample records     |
| 3    | Select Queries | Show data fetch and filter   |
| 4    | Indexing       | Create index and show effect |
| 5    | DBA Checks     | Size, Index, Locks, Stats    |
| 6    | Backup Demo    | `pg_dump` example            |
| 7    | Graph Query    | Monthly enrollment trend     |

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

> # **PostgreSQL / EDB DBA Queries 

## 1️⃣ **Create User / Role in EDB**

```sql
-- Simple login user
CREATE ROLE student_user LOGIN PASSWORD 'StrongPass123';

-- User with admin privileges
CREATE ROLE admin_user LOGIN PASSWORD 'AdminPass123' CREATEDB CREATEROLE;

-- Non-login role (just a group)
CREATE ROLE report_role;
```

---

## 2️⃣ **Alter User / Role**

```sql
-- Change password
ALTER ROLE student_user PASSWORD 'NewPass456';

-- Rename user
ALTER ROLE student_user RENAME TO student_member;

-- Add privileges to user
ALTER ROLE student_member CREATEDB;
ALTER ROLE student_member CREATEROLE;

-- Set session defaults
ALTER ROLE student_member SET search_path TO public, reports;
```

---

## 3️⃣ **Drop User / Role**

```sql
DROP ROLE IF EXISTS student_member;
```

> ⚠️ Drop karne se pehle ensure karo ki user ke paas koi object ka ownership nahi hai, warna error aayega.

---

## 4️⃣ **Grant Privileges in EDB**

### Database Level

```sql
GRANT ALL PRIVILEGES ON DATABASE mydb TO admin_user;
GRANT CONNECT ON DATABASE mydb TO student_user;
```

### Schema Level

```sql
GRANT USAGE ON SCHEMA public TO student_user;
GRANT CREATE ON SCHEMA public TO student_user;
```

### Table Level

```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON TABLE students TO student_user;
GRANT ALL PRIVILEGES ON TABLE students TO admin_user;
```

### Role Inheritance

```sql
GRANT report_role TO student_user;
```

---

## 5️⃣ **Revoke Privileges in EDB**

```sql
REVOKE SELECT ON TABLE students FROM student_user;
REVOKE ALL PRIVILEGES ON DATABASE mydb FROM student_user;
```

---

## 6️⃣ **Check Users / Roles**

### List all roles/users

```sql
\du
```

### Check privileges on a table

```sql
\z students
```

### Using information_schema

```sql
SELECT grantee, privilege_type
FROM information_schema.role_table_grants
WHERE table_name='students';
```

---

## 7️⃣ **Session Monitoring in EDB**

```sql
-- Active sessions
SELECT pid, usename, datname, client_addr, state, query
FROM pg_stat_activity;

-- Terminate a session
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE usename='student_user';
```

---

## 8️⃣ **Other Useful DBA Queries in EDB**

| Task                          | Query                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------- |
| Database size                 | `SELECT pg_size_pretty(pg_database_size('mydb'));`                                |
| Table size                    | `SELECT pg_size_pretty(pg_total_relation_size('students'));`                      |
| All tablespaces               | `SELECT spcname, pg_size_pretty(pg_tablespace_size(spcname)) FROM pg_tablespace;` |
| Objects owned by user         | `\dU+ student_user`                                                               |
| Database connections per user | `SELECT usename, count(*) FROM pg_stat_activity GROUP BY usename;`                |

---

### 🔹 **Live Demo Flow for EDB Users**

1. Create user / role → `CREATE ROLE`
2. Alter user → password change, rename, add privileges
3. Grant / Revoke privileges → table, schema, database level
4. Check users / roles → `\du`, `\z students`
5. Session monitoring → `pg_stat_activity`
6. Drop user → `DROP ROLE IF EXISTS`

--------------------------------------------------------------------------------------------------------------------------------------------------------------------


> # **EDB / PostgreSQL script** **user creation, alteration, privileges grant/revoke, session check, and drop**


## 📝 **EDB Live Demo Script – User & Privileges**

```sql
-- ===============================================
-- EDB / PostgreSQL Live Demo Script for Users & Privileges
-- Ready for PEM Live Demo
-- ===============================================

-- 1️⃣ Create Users / Roles
DROP ROLE IF EXISTS student_user;
DROP ROLE IF EXISTS admin_user;
DROP ROLE IF EXISTS report_role;

CREATE ROLE student_user LOGIN PASSWORD 'StrongPass123';
CREATE ROLE admin_user LOGIN PASSWORD 'AdminPass123' CREATEDB CREATEROLE;
CREATE ROLE report_role; -- non-login role (group)

-- 2️⃣ Alter Users / Roles
-- Change password
ALTER ROLE student_user PASSWORD 'NewPass456';

-- Rename user
ALTER ROLE student_user RENAME TO student_member;

-- Add privileges
ALTER ROLE student_member CREATEDB;
ALTER ROLE student_member CREATEROLE;

-- Set session defaults
ALTER ROLE student_member SET search_path TO public, reports;

-- 3️⃣ Grant Privileges
-- Database level
GRANT CONNECT ON DATABASE your_db_name TO student_member;
GRANT ALL PRIVILEGES ON DATABASE your_db_name TO admin_user;

-- Schema level
GRANT USAGE ON SCHEMA public TO student_member;
GRANT CREATE ON SCHEMA public TO student_member;

-- Table level
GRANT SELECT, INSERT, UPDATE, DELETE ON TABLE students TO student_member;
GRANT ALL PRIVILEGES ON TABLE students TO admin_user;

-- Role inheritance
GRANT report_role TO student_member;

-- 4️⃣ Check Users / Roles
-- List all users / roles
\du

-- Check privileges on a table
\z students

-- Using SQL (information_schema)
SELECT grantee, privilege_type
FROM information_schema.role_table_grants
WHERE table_name='students';

-- 5️⃣ Session Monitoring
-- Active sessions
SELECT pid, usename, datname, client_addr, state, query
FROM pg_stat_activity;

-- Terminate a session (example)
-- SELECT pg_terminate_backend(pid) 
-- FROM pg_stat_activity 
-- WHERE usename='student_member';

-- 6️⃣ Other Useful DBA Queries
-- Database size
SELECT pg_size_pretty(pg_database_size('your_db_name')) AS db_size;

-- Table size
SELECT pg_size_pretty(pg_total_relation_size('students')) AS table_size;

-- Tablespaces info
SELECT spcname, pg_size_pretty(pg_tablespace_size(spcname)) AS size
FROM pg_tablespace;

-- Objects owned by user
\dU+ student_member;

-- Connections per user
SELECT usename, count(*) AS total_connections
FROM pg_stat_activity
GROUP BY usename;

-- 7️⃣ Drop Users / Roles (cleanup after demo)
-- DROP ROLE IF EXISTS student_member;
-- DROP ROLE IF EXISTS admin_user;
-- DROP ROLE IF EXISTS report_role;

-- ===============================================
-- END OF LIVE DEMO SCRIPT
-- ===============================================
```

---

### 🔹 **Live Demo Tips**

1. Sabse pehle **create roles** run karo, phir audience ko explain karo login/non-login difference.
2. Fir **alter role** → password change, rename, session defaults dikhao.
3. **Grant / Revoke** → table, schema, db level privileges dikhao.
4. **Check roles** → `\du`, `\z students`, info_schema queries.
5. **Session monitoring** → active connections, terminate example.
6. Optional: **Drop roles** → cleanup after demo.

---
