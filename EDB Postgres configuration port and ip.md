> ## Aap **EDB Postgres Advanced Server** ki IP aur Port configuration **`postgresql.conf`** file me change kar sakte ho.  

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

```
psql -h 127.0.0.1 -p 5444 -U enterprisedb -W
psql -h 127.0.0.1 -p 5444 -U enterprisedb
sudo -u enterprisedb psql -h 127.0.0.1 -p 5444 -U enterprisedb -l
sudo -u enterprisedb psql -h 127.0.0.1 -p 5444 -U enterprisedb

ls -l /usr/edb/as15/share/extension/pem.control
sudo cp /usr/edb/pem/share/extension/pem.control /usr/edb/as15/share/extension/
```
