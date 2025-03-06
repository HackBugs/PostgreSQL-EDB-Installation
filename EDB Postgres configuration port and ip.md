- [Installing Postgres Enterprise Manager server on AlmaLinux 9 or Rocky Linux 9 x86_64](https://www.enterprisedb.com/docs/pem/latest/installing/linux_x86_64/pem_other_linux_9/)

> # Aap **EDB Postgres Advanced Server** ki IP aur Port configuration **`postgresql.conf`** file me change kar sakte ho.  

### **Step 1: `postgresql.conf` file ko edit karo**  
```bash
sudo vi /pgdata/data/postgresql.conf
```

---

### **Step 2: `listen_addresses` aur `port` update karo**  
👉 **IP address badalne ke liye (`listen_addresses`)**  
Find this line:  
```ini
listen_addresses = '*'
```
Yeh `*` ka matlab hai ki **sabhi IPs pe listen karega**.  
Agar sirf ek specific IP pe listen karwana hai to:  
```ini
listen_addresses = '127.0.0.1'   # Sirf localhost ke liye
```
Ya agar kisi external machine se access chahiye to:  
```ini
listen_addresses = '0.0.0.0'     # Sabhi IPs allow karega
```

---

👉 **Port number change karne ke liye (`port`)**  
Find this line:  
```ini
port = 5444
```
Isko apni required port se replace kar do, jaise:  
```ini
port = 5432
```

---

### **Step 3: `pg_hba.conf` ko update karo (Agar external access chahiye ho)**  
```bash
sudo vi /pgdata/data/pg_hba.conf
```
Add this line **(Agar remote access chahiye ho to)**  
```
host    all             all             0.0.0.0/0            md5
```

---

### **Step 4: PostgreSQL Restart Karo**  
```bash
sudo systemctl restart edb-as-15
```

Phir check karo ki naya port listen ho raha hai ya nahi:  
```bash
ss -tulnp | grep 5444
```

### **Ab aap naye port/IP par connect kar sakte ho**  
```bash
psql -h 127.0.0.1 -p 5444 -U enterprisedb
```

---

> # **🔹 Summary of the PEM Installation Issue**  

#### **⚡ Problem:**  
User is trying to configure **Postgres Enterprise Manager (PEM)** on **EDB Advanced Server 15 (AS15)** using the script:  
```bash
sudo /usr/edb/pem/bin/configure-pem-server.sh
```
However, the **PEM Agent fails to register** due to a **missing "pem" database**.

---

### **📌 Steps Taken & Observations:**  

1. **Started the PEM configuration script.**  
   - Chose **Install type: 2 (Web Services only).**  
   - Confirmed that required packages (`httpd`, `mod_ssl`, `edb-python310-mod-wsgi`) are already installed.  

2. **Entered PostgreSQL installation details:**  
   - Database path: `/usr/edb/as15`  
   - Host: `127.0.0.1`  
   - Superuser: `enterprisedb`  
   - Port: `5444`  

3. **Entered SSL certificate details.**  
   - Provided a certificate subject for the PEM web server.  

4. **Created the required SSL extension:**  
   ```sql
   CREATE EXTENSION sslutils;
   ```
   ✅ Extension `sslutils` was already installed.  

5. **Error Encountered:**  
   - **PEM Agent failed to connect to the database**:  
     ```
     connection to server at "127.0.0.1", port 5444 failed: FATAL:  database "pem" does not exist
     ```
   - **Critical Error:** `PEM Agent failed to register with PEM server.`  

---

### **🚀 Solution & Next Steps:**  

1. **Manually create the missing "pem" database** in PostgreSQL:  
   ```bash
   sudo -i -u enterprisedb psql -p 5444
   ```
   Then, run:  
   ```sql
   CREATE DATABASE pem;
   ```

2. **Grant necessary permissions:**  
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE pem TO enterprisedb;
   ```

3. **Restart PostgreSQL:**  
   ```bash
   sudo systemctl restart edb-as-15
   ```

4. **Retry the PEM configuration script:**  
   ```bash
   sudo /usr/edb/pem/bin/configure-pem-server.sh
   ```

---

### **🔹 Conclusion:**  
- The PEM configuration **failed due to the missing "pem" database**.  
- **Manually creating the database and granting permissions should fix the issue.**  
- If the problem persists, check PostgreSQL logs for more details:  
  ```bash
  sudo journalctl -u edb-as-15 --no-pager | tail -50
  ```

---

```
psql -h 127.0.0.1 -p 5444 -U enterprisedb -W
psql -h 127.0.0.1 -p 5444 -U enterprisedb
sudo -i -u enterprisedb psql -p 5444
psql -h 127.0.0.1 -p 5444 -U enterprisedb -d pem

sudo -u enterprisedb psql -h 127.0.0.1 -p 5444 -U enterprisedb -l
sudo -u enterprisedb psql -h 127.0.0.1 -p 5444 -U enterprisedb

ls -l /usr/edb/as15/share/extension/pem.control
sudo cp /usr/edb/pem/share/extension/pem.control /usr/edb/as15/share/extension/
```
