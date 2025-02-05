> ## **Zabbix Installation & Troubleshooting Guide**  

### **Download Zabbix**  
- > **[Zabbix Download](https://www.zabbix.com/download?zabbix=6.0&os_distribution=red_hat_enterprise_linux&os_version=9&components=server_frontend_agent&db=pgsql&ws=apache)**
- > **[install-zabbix-on-rhel-8/](https://www.tecmint.com/install-zabbix-on-rhel-8/)**
- > **[how-to-install-zabbix-server-on-rhel-8-rhel-9/](https://tekneed.com/how-to-install-zabbix-server-on-rhel-8-rhel-9/)**

> # Install Zabbix ✅

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

   sudo dnf install zabbix-web
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
systemctl enable --now zabbix-agent httpd php-fpm
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

> # Install PHP ✅

To upgrade PHP from version 7.2 to 7.4 (or the latest version) on Red Hat Enterprise Linux (RHEL), you need to enable the appropriate repositories and install the desired PHP version. Here's how you can do it:

---

### **Step 1: Remove Existing PHP (Optional)**
If you want to clean up the existing PHP installation, you can remove it. However, this step is optional, as installing a newer version will typically replace the older one.

```bash
sudo dnf remove php*
```

---

### **Step 2: Enable Remi Repository**
The Remi repository provides the latest versions of PHP for RHEL-based systems. You can enable it as follows:

1. **Install the EPEL repository (if not already installed):**
   ```bash
   sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
   ```

2. **Install the Remi repository:**
   ```bash
   sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
   ```

3. **Enable the Remi repository for PHP 7.4 or later:**
   - For PHP 7.4:
     ```bash
     sudo dnf module reset php
     sudo dnf module enable php:remi-7.4
     ```
   - For the latest PHP version (e.g., PHP 8.x):
     ```bash
     sudo dnf module reset php
     sudo dnf module enable php:remi-8.2
     ```

---

### **Step 3: Install PHP**
Once the repository is enabled, install the desired PHP version:

- For PHP 7.4:
  ```bash
  sudo dnf install php php-cli php-common php-fpm php-mysqlnd php-zip php-gd php-mbstring php-curl php-xml php-bcmath php-json
  ```

- For the latest PHP version (e.g., PHP 8.x):
  ```bash
  sudo dnf install php php-cli php-common php-fpm php-mysqlnd php-zip php-gd php-mbstring php-curl php-xml php-bcmath php-json
  ```

---

### **Step 4: Verify the PHP Version**
After installation, verify the PHP version to ensure the upgrade was successful:

```bash
php -v
```

You should see output similar to:
```
PHP 7.4.x (cli) (built: ...)
```
or
```
PHP 8.x.x (cli) (built: ...)
```

---

### **Step 5: Restart Web Server (if applicable)**
If you're using PHP with a web server like Apache or Nginx, restart the service to apply the changes:

- For Apache:
  ```bash
  sudo systemctl restart httpd
  ```

- For Nginx (if using PHP-FPM):
  ```bash
  sudo systemctl restart php-fpm
  sudo systemctl restart nginx
  ```

---

### **Step 6: Check PHP Modules (Optional)**
You can list the installed PHP modules to ensure all required extensions are installed:

```bash
php -m
```

---

### **Step 7: Update PHP Configuration (Optional)**
If you need to customize PHP settings, edit the `php.ini` file:

```bash
sudo vi /etc/php.ini
```

After making changes, restart the web server or PHP-FPM service.


<hr>

> # Install Zabbix-web and check PHP ✅

The error message indicates that the `zabbix-web` package is available in the repository but is not currently installed on your system. This is likely why you're encountering the `Not Found` error when trying to access the Zabbix web interface.

Let’s resolve this step by step:

---

### **1. Install the `zabbix-web` Package**
Since the package is available but not installed, you need to install it explicitly.

Run the following command to install the `zabbix-web` package:
```bash
sudo dnf install zabbix-web
```

---

### **2. Verify the Installation**
After installation, verify that the Zabbix web files are present in the correct directory:
```bash
ls -l /usr/share/zabbix/
```
You should see a list of files and directories, such as `app`, `include`, `local`, etc.

---

### **3. Check Apache Configuration**
Ensure that the Apache configuration for Zabbix is correctly set up.

- Check the Zabbix Apache configuration file:
  ```bash
  sudo vi /etc/httpd/conf.d/zabbix.conf
  ```
  The file should contain something like this:
  ```apache
  Alias /zabbix /usr/share/zabbix

  <Directory "/usr/share/zabbix">
      Options FollowSymLinks
      AllowOverride None
      Require all granted
  </Directory>
  ```

- If the file is missing, create it manually with the above content.

---

### **4. Restart Apache**
After verifying the configuration, restart Apache to apply the changes:
```bash
sudo systemctl restart httpd
```

---

### **5. Check SELinux (if enabled)**
If SELinux is enabled, it might block access to the Zabbix web files.

- Check SELinux status:
  ```bash
  sestatus
  ```

- Temporarily set SELinux to permissive mode to test:
  ```bash
  sudo setenforce 0
  ```

- If the issue is resolved, adjust SELinux policies for Zabbix:
  ```bash
  sudo chcon -R -t httpd_sys_rw_content_t /usr/share/zabbix/
  ```

---

### **6. Check File Permissions**
Ensure that the Zabbix web files have the correct permissions and are owned by the Apache user.

- Set the correct permissions:
  ```bash
  sudo chmod -R 755 /usr/share/zabbix/
  sudo chown -R apache:apache /usr/share/zabbix/
  ```

---

### **7. Test with a Simple File**
Create a simple test file in the Zabbix web directory to ensure Apache can serve files from that location.

- Create a test file:
  ```bash
  echo "Hello, Zabbix!" | sudo tee /usr/share/zabbix/test.html
  ```

- Access the test file in your browser:
  ```
  http://192.168.1.71/zabbix/test.html
  ```

  If this works, the issue is likely with the Zabbix web interface configuration.

---

### **8. Verify Zabbix Web Interface Configuration**
Ensure that the Zabbix web interface is properly configured.

- Check the Zabbix web configuration file:
  ```bash
  sudo vi /etc/zabbix/web/zabbix.conf.php
  ```

  Ensure the database and Zabbix server settings are correct:
  ```php
  <?php
  $DB['TYPE']     = 'POSTGRESQL';
  $DB['SERVER']   = 'localhost';
  $DB['PORT']     = '0';
  $DB['DATABASE'] = 'zabbix';
  $DB['USER']     = 'zabbix';
  $DB['PASSWORD'] = 'your_password';

  $ZBX_SERVER      = 'localhost';
  $ZBX_SERVER_PORT = '10051';
  $ZBX_SERVER_NAME = 'Zabbix';
  ?>
  ```

---

### **9. Check Logs for Errors**
Check the Apache and Zabbix logs for any errors that might provide more details about the issue.

- Apache error log:
  ```bash
  sudo tail -n 20 /var/log/httpd/error_log
  ```

- Zabbix server log:
  ```bash
  sudo tail -n 20 /var/log/zabbix/zabbix_server.log
  ```

---

### **10. Final Check**
After completing the above steps, try accessing the Zabbix web interface again:
```
http://192.168.1.71/zabbix/
```

---

If the issue persists, please provide the output of the following commands so I can assist further:
1. `ls -l /usr/share/zabbix/`
2. `sudo cat /etc/httpd/conf.d/zabbix.conf`
3. `sudo tail -n 20 /var/log/httpd/error_log`

<hr>

> # Aap **RHEL 8** use kar rahe hain, lekin aap **RHEL 7 ka Remi repository** install kar rahe ho, is wajah se conflict ho raha hai.  
Aapko **RHEL 8 ke liye sahi repository enable** karni padegi.  ✅

---

### **RHEL 8 me PHP 7.4 Install Karne ke Steps**

#### **1. Purana PHP Remove Karo (Agar Installed Hai)**
```bash
sudo yum remove -y php php-cli php-common php-mysql
```

#### **2. EPEL & Remi Repository Enable Karo (RHEL 8)**
```bash
sudo yum install -y epel-release yum-utils
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

#### **3. Remi Repository Se PHP 7.4 Enable Karo**
```bash
sudo yum module reset php
sudo yum module enable php:remi-7.4
```

## Aapka error **"All matches were filtered out by modular filtering"** ka matlab hai ki **RHEL 8 ka default module stream enabled hai**, jo **PHP 7.2** provide karta hai.  
Aapko **PHP module reset karna hoga aur Remi repository ka PHP 7.4 module enable karna hoga**.  

---

### ** Solution: PHP 7.4 Install Karne ke Steps in RHEL 8**
#### **1. PHP Module Reset Karo**
```bash
sudo yum module reset php -y
```

#### **2. PHP 7.4 ke Remi Repository Ko Enable Karo**
```bash
sudo yum module enable php:remi-7.4 -y
```

#### **3. PHP 7.4 Install Karo**
```bash
sudo yum install -y php php-cli php-mysqlnd php-common php-xml php-curl php-mbstring php-zip php-bcmath php-json
```
**⚠️ Note:** `php-mysql` ki jagah **`php-mysqlnd`** use karo, kyunki **RHEL 8 aur PHP 7.4 ke liye `php-mysqlnd` recommended hai**.

#### **4. Verify Karo Ki PHP 7.4 Install Hua Hai**
```bash
php -v
```
Agar output me **PHP 7.4.x** dikhe, to installation successful hai.

#### **5. Apache Restart Karo (Agar Required Hai)**
Agar aap Apache (`httpd`) use kar rahe ho, to:
```bash
sudo systemctl restart httpd
```

---

### **Agar `php -v` Me Abhi Bhi Purana Version Dikhe**
```bash
sudo alternatives --config php
```
Is command se manually **PHP 7.4 select** karo.  

**Ab aapka error solve ho jana chahiye!** 🚀  
Agar koi aur issue aaye to batao. 😊


<hr>
<hr>

> ## **Search for Zabbix Packages** ✅
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
