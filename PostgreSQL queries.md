
> # Alter a database password in PostgreSQL or EDB:

```bash
# Step 1: Log in to the PostgreSQL database as a superuser
psql -U postgres -d postgres

# Step 2: Change the user's password
ALTER USER username WITH PASSWORD 'new_password';

# Replace 'username' with the target database user
# Replace 'new_password' with the desired password

# Step 3: Verify the password change by logging in with the updated credentials
psql -U username -h localhost -d database_name

# Step 4: If using PgBouncer, update the userlist.txt file
sudo nano /etc/pgbouncer/userlist.txt

# Add or update the line with:
# "username" "new_password"

# Step 5: Restart PgBouncer to apply changes
sudo systemctl restart pgbouncer
``` 

Replace placeholders like `username` and `new_password` with your actual values.

<hr>

> # Important PostgreSQL queries and commands, categorized and explained for clarity:

---

### **General Commands**
1. `/q` - **Exit** the PostgreSQL interactive terminal (`psql`).
2. `/h` - **List of keywords** or available commands with explanations.
3. `/l ?` - **Check information** about databases.

---

### **Buffer Management**
1. `/a` - **Format** query output for readability (aligned format).
2. `/x` - **Expanded display** for wide columns or detailed output.
3. `/r` - **Reset query buffer**, clearing any text in the buffer.
4. `/p` - **Check query buffer** contents before execution.

---

### **Connection Management**
1. `/c` - **Check database connection** status and switch databases.
2. `/z ?` - **Get information** about access privileges.

---

### **Database and Table Information**
1. `/d pg_database;` - **Describe the `pg_database` table**, showing details about databases.
2. `/d pg_user;` - **Describe the `pg_user` table**, showing user-related details.
3. `/d pg_catalog.pg_tables;` - **Describe `pg_catalog.pg_tables`**, listing system and user tables.

---

### **Query for PostgreSQL System Tables**
- **List tables with a prefix `pg_`:**
  ```sql
  SELECT tablename
  FROM pg_catalog.pg_tables
  WHERE tablename LIKE 'pg_%';
  ```
  - Displays all system tables starting with `pg_`.

---

### **Notes**
- Prefixes like `pg_` in PostgreSQL indicate **system catalogs** or metadata tables.  
- Use `\dt` for a quick **list of user-defined tables**.  
- Combine `/x` and `/d` commands for detailed and formatted descriptions.

<hr>

> # PostgreSQL and EnterpriseDB (EDB) DBA (Database Administrator) day-to-day queries, categorized into **monitoring**, **user management**, **backup/restore**, **performance tuning**, **maintenance**, and **troubleshooting** tasks:

---

## **1. Monitoring and Database Insights**
### List all Databases:
```sql
\l
SELECT datname FROM pg_database;
```

### Check Active Connections:
```sql
SELECT pid, usename, datname, client_addr, backend_start
FROM pg_stat_activity;
```

### Check Database Size:
```sql
SELECT pg_size_pretty(pg_database_size('your_database_name')) AS size;
```

### List Tables in a Schema:
```sql
\dt schema_name.*
SELECT tablename FROM pg_tables WHERE schemaname = 'schema_name';
```

### Index Usage Statistics:
```sql
SELECT relname AS table_name,
       idx_scan AS index_usage,
       seq_scan AS seq_usage
FROM pg_stat_user_tables;
```

### Check Long-Running Queries:
```sql
SELECT pid, usename, query, state, age(clock_timestamp(), query_start) AS runtime
FROM pg_stat_activity
WHERE state != 'idle' AND now() - query_start > interval '5 minutes';
```

### View Locks:
```sql
SELECT locktype, database, relation, mode, granted, pid, query
FROM pg_locks l
JOIN pg_stat_activity a ON l.pid = a.pid;
```

---

## **2. User and Role Management**
### Create a User:
```sql
CREATE ROLE user_name WITH LOGIN PASSWORD 'password';
```

### Grant Privileges:
```sql
GRANT CONNECT ON DATABASE your_database TO user_name;
GRANT USAGE ON SCHEMA schema_name TO user_name;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA schema_name TO user_name;
```

### Change Password:
```sql
ALTER ROLE user_name PASSWORD 'new_password';
```

### List Users:
```sql
\du
SELECT * FROM pg_roles;
```

---

## **3. Backup and Restore**
### Backup a Database:
```bash
pg_dump -U username -h hostname -Fc database_name > backup_file.dump
```

### Restore a Database:
```bash
pg_restore -U username -h hostname -d database_name backup_file.dump
```

---

## **4. Performance Tuning**
### Analyze and Vacuum Tables:
```sql
VACUUM (VERBOSE, ANALYZE);
```

### Check Indexes for Bloat:
```sql
SELECT indexrelid::regclass AS index, 
       pg_size_pretty(pg_relation_size(indexrelid)) AS size
FROM pg_stat_user_indexes
WHERE idx_scan = 0;
```

### Show Query Execution Plan:
```sql
EXPLAIN ANALYZE SELECT * FROM table_name WHERE column = 'value';
```

### Check Table Bloat:
```sql
SELECT schemaname, relname, 
       pg_size_pretty(pg_table_size(oid)) AS table_size,
       pg_size_pretty(pg_total_relation_size(oid)) AS total_size
FROM pg_class
WHERE relkind = 'r';
```

---

## **5. Maintenance**
### Reindex a Table or Database:
```sql
REINDEX TABLE table_name;
REINDEX DATABASE database_name;
```

### Drop and Recreate Unused Indexes:
```sql
DROP INDEX IF EXISTS index_name;
CREATE INDEX index_name ON table_name(column_name);
```

### Reset Statistics:
```sql
SELECT pg_stat_reset();
```

---

## **6. Troubleshooting**
### Check Logs for Errors:
- **PostgreSQL Logs:**
  ```bash
  tail -f /var/log/postgresql/postgresql.log
  ```

- **EDB Logs:**
  ```bash
  tail -f /var/log/edb/edb.log
  ```

### Check Deadlocks:
```sql
SELECT pid, usename, query, wait_event, wait_event_type
FROM pg_stat_activity
WHERE wait_event_type = 'Lock';
```

### Identify Missing Constraints:
```sql
SELECT conname, conrelid::regclass AS table_name
FROM pg_constraint
WHERE NOT contype = 'f';
```

---

## **7. EDB-Specific Queries**
### EDB Failover Manager (EFM) Cluster Status:
```bash
efm cluster-status
```

### EDB Postgres Advanced Server Audit Logs:
```sql
SELECT * FROM edb_audit;
```

### Check EDB Package Privileges:
```sql
SELECT * FROM all_tab_privs WHERE table_name = 'DBMS_OUTPUT';
```

---

## **8. Essential Administrative Queries**
### Kill a Session:
```sql
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE pid = session_id;
```

### Change Default Transaction Isolation Level:
```sql
ALTER DATABASE database_name SET default_transaction_isolation TO 'read committed';
```

---

## **9. Schema Management**
### Create a Schema:
```sql
CREATE SCHEMA schema_name;
```

### Rename a Schema:
```sql
ALTER SCHEMA old_name RENAME TO new_name;
```

---
