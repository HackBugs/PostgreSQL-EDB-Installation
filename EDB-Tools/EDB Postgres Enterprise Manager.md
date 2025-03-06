[Installing Postgres Enterprise Manager server on AlmaLinux 9 or Rocky Linux 9 x86_64](https://www.enterprisedb.com/docs/pem/latest/installing/linux_x86_64/pem_other_linux_9/)

## Local Repo

```
yum install edb-as15-server-sslutils-1.3-1.el8.x86_64.rpm --disablerepo="*" --enablerepo="localrepo" -y
yum clean all
yum update -y
dnf repolist
rpm -ivh edb-as15-server-sslutils-1.3-1.el8.x86_64.rpm
```
- vi /etc/yum.repos.d/local.repo
```
[localrepo]
name=Local Repo
baseurl=file:///opt/edb
gpgcheck=0
enabled=1
```
---

### **Issue: YUM `localrepo` Metadata Missing, Createrepo Install Nahi Ho Raha**  
Aapka `yum` command `localrepo` se package install karne ki koshish kar raha hai, par `localrepo` abhi properly configured nahi hai. Aapko pehle **`localrepo` disable karke** `createrepo` install karna hoga.

---

### **✅ Fix Step-by-Step**  

#### **Step 1: Local Repo Temporarily Disable Karo**  
YUM `localrepo` ko use karne ki koshish kar raha hai jo abhi properly configured nahi hai. Isko **temporarily disable** karke install karein:  
```bash
yum install -y createrepo --disablerepo=localrepo
```

---

#### **Step 2: Repodata Generate Karo**  
Ab `/opt/edb` ke andar `repodata` generate karo:  
```bash
createrepo /opt/edb
```

Yeh command `/opt/edb/repodata/repomd.xml` generate karegi.

---

#### **Step 3: YUM Cache Clean & Refresh Karo**  
```bash
yum clean all
yum makecache
```

---

#### **Step 4: Ab Package Install Karo**
```bash
yum install edb-as15-server-sslutils-1.3-1.el8.x86_64.rpm --enablerepo=localrepo
```

---

### **⚡ Extra Debugging Agar Fir Bhi Error Aaye**  
✅ **Check Karo `repodata` generate hua ya nahi:**  
```bash
ls -l /opt/edb/repodata/
```

✅ **Check Karo `localrepo` configuration sahi hai ya nahi:**  
```bash
cat /etc/yum.repos.d/local.repo
```

✅ **SELinux issue to nahi hai:**  
```bash
getenforce  # Agar `Enforcing` hai to disable karo: setenforce 0
```

✅ **File Permissions sahi hain ya nahi:**  
```bash
chmod -R 755 /opt/edb
```

---

> # To set up a **local repository** in **RHEL-based Linux** (Red Hat, CentOS, Rocky Linux, AlmaLinux), follow these steps:

---

### **1. Mount the DVD or ISO (If using a local disk)**
If you're using an **ISO image**, mount it to `/mnt`:

```bash
mkdir -p /mnt/cdrom
mount /dev/cdrom /mnt/cdrom
```

If using an **ISO file**, mount it like this:

```bash
mkdir -p /mnt/iso
mount -o loop /path/to/your.iso /mnt/iso
```

---

### **2. Copy Repo Files (Optional)**
If you want to copy the repo locally for permanent usage:

```bash
mkdir -p /repo
cp -r /mnt/cdrom/BaseOS /repo/
cp -r /mnt/cdrom/AppStream /repo/
```

---

### **3. Create a Local Repo File**
Edit or create the repo file:

```bash
vi /etc/yum.repos.d/local.repo
```

Add the following content:

```
[redhat]
name=Local Repository
baseurl=file:///repo/AppStream
gpgcheck=0
enabled=1
```

If you are using an **ISO mount**, change the `baseurl`:

```
[localrepo]
name=Local Repo
baseurl=file:///mnt/iso/AppStream
gpgcheck=0
enabled=1
```

---

### **4. Clear & Verify Repo**
Run these commands to clear old metadata and verify the repo:

```bash
dnf clean all
dnf repolist
```

---

### **5. Install Packages from Local Repo**
Now, you can install packages using:

```bash
dnf install <package-name> --disablerepo="*" --enablerepo="redhat"
```

<hr>

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

<hr>

\pset pager off
\du ;

psql -h 127.0.0.1 -p 5444 -U enterprisedb -W
psql -h 127.0.0.1 -p 5444 -U enterprisedb
sudo -i -u enterprisedb psql -p 5444
psql -h 127.0.0.1 -p 5444 -U enterprisedb -d pem

sudo -u enterprisedb psql -h 127.0.0.1 -p 5444 -U enterprisedb -l
sudo -u enterprisedb psql -h 127.0.0.1 -p 5444 -U enterprisedb

psql -h 192.168.226.142 -p 5444 -U enterprisedb -W

<hr>

> # PostgreSQL user (role) create give PRIVILEGES

user more than 5 attempt should locked
180 shoud be expired 
previous 5 time similler passwaor can't enter
if account inacitve 120 that account shoub atomticlay inactive
paswword lenth shoud be 8
and complicity rule should be of password

GRANT ALL PRIVILEGES ON DATABASE enterprisedb TO alam;

GRANT ALL PRIVILEGES ON SCHEMA public TO alam;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO alam;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO alam;
GRANT ALL PRIVILEGES ON ALL FUNCTIONS IN SCHEMA public TO alam;

ALTER USER alam WITH SUPERUSER;

<hr>

```
sudo systemctl stop pemagent
sudo systemctl stop pemhttpd
sudo systemctl stop edb-pem

sudo dnf install remove edb-pem-server edb-pem-agent edb-pem-httpd -y
```
