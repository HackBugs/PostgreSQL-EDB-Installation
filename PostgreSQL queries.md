Here's a well-organized list of important PostgreSQL queries and commands, categorized and explained for clarity:

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

Would you like an explanation or example for any of these commands?
