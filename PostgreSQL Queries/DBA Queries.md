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
