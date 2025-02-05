> ## **Zabbix Installation & Troubleshooting Guide**  

### **Download Zabbix**  
> **[Zabbix Download](https://www.zabbix.com/download?zabbix=6.0&os_distribution=red_hat_enterprise_linux&os_version=9&components=server_frontend_agent&db=pgsql&ws=apache)**
> **[install-zabbix-on-rhel-8/](https://www.tecmint.com/install-zabbix-on-rhel-8/)**

### **Search for Zabbix Packages**  
```bash
yum search zabbix
```

---

## **Troubleshooting Steps**  

### **1. Ensure System is Up to Date**  
```bash
sudo dnf update
```

### **2. Check and Enable Required Repositories**  

#### **For OpenSSL and LDAP**  
Install missing dependencies:  
```bash
sudo dnf install openssl-libs openldap
```

#### **For GLIBC**  
Ensure your system has the required version. If missing, update it.

### **3. Try Installing with `--nobest` Option**  
```bash
sudo dnf install zabbix-server-pgsql zabbix-web-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent --nobest
```

### **4. Skip Broken Packages If Needed**  
```bash
dnf install --skip-broken zabbix-server-pgsql zabbix-web-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent
```

### **5. Verify Required Libraries**  
```bash
find /usr/lib /usr/lib64 -name libssl.so.3
find /usr/lib /usr/lib64 -name libldap.so.2
```

### **6. Manually Install Missing Dependencies**  

#### **For OpenSSL**  
```bash
sudo dnf install openssl3
```

#### **For OpenLDAP**  
```bash
sudo dnf install openldap
```

### **7. Enable EPEL Repository**  
```bash
dnf install epel-release
dnf update
```

### **8. Check Enabled Repositories**  
```bash
dnf repolist
```

### **9. Verify RHEL Version Compatibility**  
Ensure your RHEL version meets Zabbix dependency requirements.

---

## **PostgreSQL Setup for Zabbix**  

### **1. Allow `zabbixuser` to Connect**  
```sql
ALTER USER zabbixuser SET default_transaction_isolation = 'read committed';
```

### **2. Fix Schema Missing Error**  

#### **Check if Schema Exists**  
```sql
SELECT schema_name FROM information_schema.schemata;
```

#### **Create Schema If Missing**  
```sql
CREATE SCHEMA zabbix;
```

#### **Create `zabbixuser` If Not Exists**  
```sql
CREATE USER zabbixuser WITH PASSWORD '@#123';
```

#### **Grant Privileges to `zabbixuser`**  
```sql
GRANT ALL PRIVILEGES ON SCHEMA zabbix TO zabbixuser;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA zabbix TO zabbixuser;
```

---

## **Zabbix Database Initialization**  

### **For PostgreSQL (EDB)**  
```bash
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql -h <host> -U <username> -d zabbix
```

**Breakdown:**  
- `zcat`: Unzips the SQL script.  
- `sudo -u zabbix`: Runs command as `zabbix` user.  
- `psql -h <host> -U <username> -d zabbix`: Connects to the `zabbix` database.  

#### **If Using a Password**  
```bash
export PGPASSWORD=<your_password>
```

---

## **Create Zabbix User & Run Database Initialization**  
```bash
sudo useradd zabbix
id zabbix
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
