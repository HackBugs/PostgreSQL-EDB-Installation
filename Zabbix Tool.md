> ## **Zabbix Installation & Troubleshooting Guide**  

### **Download Zabbix**  
- > **[Zabbix Download](https://www.zabbix.com/download?zabbix=6.0&os_distribution=red_hat_enterprise_linux&os_version=9&components=server_frontend_agent&db=pgsql&ws=apache)**
- > **[install-zabbix-on-rhel-8/](https://www.tecmint.com/install-zabbix-on-rhel-8/)**
- > **[how-to-install-zabbix-server-on-rhel-8-rhel-9/](https://tekneed.com/how-to-install-zabbix-server-on-rhel-8-rhel-9/)**

### 1. **Enable the EPEL Repository**
The Extra Packages for Enterprise Linux (EPEL) repository often contains packages that are not available in the default RHEL repositories. You can enable it by running:

```bash
sudo dnf install epel-release
```

### 2. **Enable the Zabbix Repository**
Zabbix provides its own repository for RHEL-based systems. You can enable it by following these steps:

1. **Install the Zabbix repository configuration package:**

   For Zabbix 6.0 (LTS):
   ```bash
   sudo rpm -Uvh https://repo.zabbix.com/zabbix/6.0/rhel/8/x86_64/zabbix-release-6.0-4.el8.noarch.rpm
   ```

   For Zabbix 6.4 (latest stable):
   ```bash
   sudo rpm -Uvh https://repo.zabbix.com/zabbix/6.4/rhel/8/x86_64/zabbix-release-6.4-1.el8.noarch.rpm
   ```

2. **Clean the DNF cache:**
   ```bash
   sudo dnf clean all
   ```

3. **Update the repository metadata:**
   ```bash
   sudo dnf makecache
   ```

### 3. **Install the Zabbix Packages**
After enabling the Zabbix repository, you should be able to install the required packages:

```bash
sudo dnf install zabbix-server-pgsql zabbix-web-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent
```

### 4. **Verify the Installation**
Once the installation is complete, you can verify that the packages are installed correctly:

```bash
rpm -q zabbix-server-pgsql zabbix-agent
```

### 5. **Start and Enable Services**
After installation, you may want to start and enable the Zabbix server and agent services:

```bash
sudo systemctl start zabbix-server zabbix-agent
sudo systemctl enable zabbix-server zabbix-agent
```

### 6. **Check the Status of Services**
You can check the status of the services to ensure they are running correctly:

```bash
sudo systemctl status zabbix-server zabbix-agent
```

### 7. **Configure Firewall (if necessary)**
If you have a firewall enabled, you may need to open the necessary ports for Zabbix:

```bash
sudo firewall-cmd --add-service={http,https} --permanent
sudo firewall-cmd --add-port=10050/tcp --permanent
sudo firewall-cmd --add-port=10051/tcp --permanent
sudo firewall-cmd --reload
```

### 8. **Access Zabbix Web Interface**
Once everything is set up, you can access the Zabbix web interface by navigating to `http://your-server-ip/zabbix` in your web browser.

### Troubleshooting
- **Repository Issues:** If you still encounter issues, ensure that the Zabbix repository is correctly enabled by checking the contents of `/etc/yum.repos.d/zabbix.repo`.
- **Package Versions:** Ensure that the package versions you are trying to install are compatible with your RHEL version.

  
<hr>
<hr>

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
