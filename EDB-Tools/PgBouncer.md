

```
sudo vim /etc/edb/pgbouncer1.23/edb-pgbouncer-1.23.ini

SHOW data_directory;
vi /pgdata/data/pg_hba.conf
host    all     all     192.168.237.0/24   md5
host    pgbouncer     pgbouncer     192.168.237.5/32   trust

switch root to enterprisedb
su - enterprisedb
psql -p 5444 -h 192.168.237.5 -U enterprisedb
psql -p 5444 -h 192.168.237.5 -U enterprisedb -d postgres

[database]
edb = host=192.168.237.5 port=5444
postgres = host=192.168.237.5 port=5432
enterprisedb = host=192.168.237.5 port=5432
postgres = host=192.168.237.5 port=5432 dbname=postgres
acctg = host=192.168.237.5 port=5432 user=bob password=admin

[pgbouncer]
logfile = /var/log/pgbouncer/pgbouncer.log
pidfile = /var/run/pgbouncer/pgbouncer.pid
listen_addr = *
listen_port = 6432
auth_type = md5
auth_file = /etc/pgbouncer/userlist.txt
admin_users = enterprisedb
stats_users = enterprisedb
pool_mode = session
server_reset_query = DISCARD ALL
ignore_startup_parameters = application_name
max_client_conn = 100
default_pool_size = 20
```

> # Useful commands for EDB-PgBouncer

```
sudo firewall-cmd --permanent --add-port=6432/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-ports
ping 192.168.237.5

edb=# \password enterprisedb

touch ~/.pgpass
chmod 0600 ~/.pgpass
vi ~/.pgpass

# example
hostname:port:database:username:password
192.168.237.5:6432:pgbouncer:pgbouncer:yourpassword

/var/run/edb/pgbouncer1.23/edb-pgbouncer-1.23.pid
/var/log/edb/pgbouncer1.23/edb-pgbouncer-1.23.log

sudo vim /etc/edb/pgbouncer1.23/edb-pgbouncer-1.23.ini
sudo vim /usr/lib/systemd/system/edb-pgbouncer-1.23.service
Vim /pgdata/data/pg_hba.conf

sudo vi /usr/lib/systemd/system/pgbouncer.service
ExecStart=/usr/bin/pgbouncer /etc/pgbouncer/pgbouncer.ini

vi /pgdata/data/pg_hba.conf
host    all     all     192.168.237.0/24   md5
host    pgbouncer     pgbouncer     192.168.237.5/32   trust

admin_users = enterprisedb
stats_users = enterprisedb

Check log -
tail -f /var/log/edb/pgbouncer1.23/edb-pgbouncer-1.23.log
sudo journalctl -u edb-pgbouncer-1.23.service
journalctl -xe | grep edb-pgbouncer-1.23

pgbouncer -v /etc/pgbouncer/pgbouncer.ini
sudo useradd -r -s /bin/false pgbouncer
sudo chown -R pgbouncer:pgbouncer /etc/pgbouncer /var/log/pgbouncer /var/run/pgbouncer

journalctl -xe -u pgbouncer.service
cat /var/log/pgbouncer/pgbouncer.log

psql -h 192.168.237.5 -p 6432 -U enterprisedb -c "RELOAD;"

pgbench -i my_testdb

\du
edb=# CREATE USER enterprisedb;
edb=# ALTER USER enterprisedb WITH PASSWORD 'admin';
edb=# ALTER USER enterprisedb WITH SUPERUSER;
edb=# edb=# SELECT usename, passwd FROM pg_shadow;

\l
select md5('database_name')
SELECT 'md5' || md5('your_passwordedb')

Example -
echo -n "enterprisedb"mypassword | md5sum
"enterprisedb" "md5c9de1d8a0bd5c9a5e0bd6fcb9e37309f"

edb=# show max_connections;
```

```
ps -ef | grep java
kill -9 "PID"
nohup ./standalone.sh --server-config standlone-ha.xml &
systemctl status edb-as*
systemctl status pgbouncer

ps aux | grep pgbouncer
journalctl -u edb-pgbouncer-1.23
sudo systemctl restart edb-pgbouncer-1.23

systemctl daemon-reload
systemctl status edb-as-15.service
systemctl restart edb-as-15.service

psql -h 192.168.237.5 -p 6432 -U pgbouncer -c "RELOAD;"

sudo firewall-cmd --zone=public --add-port=6432/tcp --permanent
sudo firewall-cmd --reload

ping 192.168.237.5
sudo netstat -tuln | grep 6432

psql -p 6432 -h 192.168.237.5 pgbouncer
psql -U pgbouncer -p 6432 -h 192.168.237.5 pgbouncer

psql -p 6432 -h 192.168.1.71 -U root edb-pgbouncer-1.23
psql -h 192.168.1.71 -U enterprisedb -p 6432 edb-pgbouncer-1.23

auth_type = md5
auth_file = /etc/edb/pgbouncer1.23/userlist.txt
Path - /etc/edb/pgbouncer1.23/edb-pgbouncer-1.23.ini

---------------------------------------------------------------------
su - enterprise 
/usr/edb/as15/bin/psql -p 6432 -h 192.168.237.5 pgbouncer

show clients

/usr/edb/as15/bin/psql -U enterprisedb -d my_testdb -p 6432
password for user enterprisedb:
---------------------------------------------------------------------
## PID file deleted

sudo mkdir -p /var/run/edb/pgbouncer1.23
sudo chown pgbouncer:pgbouncer /var/run/edb/pgbouncer1.23
sudo chmod 755 /var/run/edb/pgbouncer1.23

sudo -u pgbouncer /usr/edb/pgbouncer1.23/bin/pgbouncer /etc/edb/pgbouncer1.23/edb-pgbouncer-1.23.ini

sudo chmod 600 /etc/edb/pgbouncer1.23/userlist.txt
sudo chown enterprisedb:enterprisedb /etc/edb/pgbouncer1.23/userlist.txt

sudo mkdir -p /var/run/edb/pgbouncer
sudo mkdir -p /var/log/edb/pgbouncer
sudo mkdir -p /etc/pgbouncer/userlist.txt
```

<hr>

> # Script uninstallation of edb-pgbouncer-1.23 from your system. 

```
sudo dnf remove edb-pgbouncer*

rpm -qa | grep pgbouncer
sudo rpm -e edb-pgbouncer116-1.16.1.0-2.rhel8.x86_64
sudo dnf clean all
sudo rpm -ivh /path/to/edb-pgbouncer123-1.23.1.0-1.el8.x86_64.rpm
```

```
#!/bin/bash

# Stop the PgBouncer service
echo "Stopping PgBouncer service..."
sudo systemctl stop edb-pgbouncer-1.23

# Disable the PgBouncer service
echo "Disabling PgBouncer service..."
sudo systemctl disable edb-pgbouncer-1.23

# Uninstall using Package Manager (RHEL/CentOS/Fedora)
echo "Uninstalling PgBouncer using package manager..."

# Check if installed with yum or dnf
if command -v yum >/dev/null 2>&1; then
    sudo yum remove -y edb-pgbouncer-1.23
elif command -v dnf >/dev/null 2>&1; then
    sudo dnf remove -y edb-pgbouncer-1.23
else
    echo "Package manager not found. Skipping package removal step."
fi

# Check if installed with rpm
if rpm -qa | grep -q pgbouncer; then
    sudo rpm -e edb-pgbouncer-1.23
else
    echo "PgBouncer package not found in RPM database. Skipping RPM removal."
fi

# Remove residual files
echo "Removing residual files..."
sudo rm -rf /etc/edb/pgbouncer1.23/
sudo rm -rf /var/log/edb/pgbouncer1.23/
sudo rm -rf /usr/edb/pgbouncer1.23/
sudo rm -f /usr/local/bin/pgbouncer
sudo rm -f /usr/local/etc/pgbouncer
sudo rm -f /var/log/edb/pgbouncer1.23/

# Remove systemd service files
echo "Removing systemd service files..."
sudo rm -f /etc/systemd/system/edb-pgbouncer-1.23.service

# Reload systemd configuration
echo "Reloading systemd configuration..."
sudo systemctl daemon-reload

# Verify removal
echo "Verifying PgBouncer removal..."
if ! command -v pgbouncer >/dev/null 2>&1; then
    echo "PgBouncer has been successfully removed."
else
    echo "PgBouncer is still installed. Please check manually."
fi
```
<hr>

> # Installation of PgBouncer [Link](https://www.enterprisedb.com/docs/pgbouncer/latest/02_configuration_and_usage/)

```
sudo yum -y install /opt/edb2/el_8_x86_64/*.rpm

## install EDB

ls | wc -l
ls | grep edb-as15-server

## install PgBouncer

yum install 
ls | grep edb-pgbounce

pgbouncer -V
psql --version

Check PgBouncer Logs - journalctl -u pgbouncer.service -n 50
 
find /etc/systemd/system/ /lib/systemd/system/ -name '*pgbouncer*'
sudo systemctl list-units --type=service | grep pgbouncer
ps aux | grep pgbouncer

systemctl list-units --type=service
sudo systemctl list-units --type=service --state=active
sudo systemctl list-units --type=service --state=active | grep pgbouncer

sudo systemctl start edb-pgbouncer-1.23.service
sudo systemctl status edb-pgbouncer-1.23.service
```
```
psql -h localhost -p 6432 -U username -d mydb
```

> # Here’s the entire process to reset the password for a user in EnterpriseDB if you've forgotten it:

```bash
# Step 1: Switch to the EnterpriseDB superuser
sudo su - enterprisedb

# Step 2: Try logging into the database
psql -U enterprisedb

# If login fails, proceed with the following steps to modify authentication.

# Step 3: Locate and edit the pg_hba.conf file
sudo vim /var/lib/edb/as<version>/data/pg_hba.conf

# Replace <version> with your EDB version, e.g., as13 for version 13.

# Find the line:
# host    all             all             127.0.0.1/32            md5

# Change it to:
# host    all             all             127.0.0.1/32            trust

# Save and exit the file (Ctrl+O, Enter, Ctrl+X).

# Step 4: Restart the EDB server to apply changes
sudo systemctl restart edb-as<version>

# Step 5: Log in without a password and reset the user's password
psql -U enterprisedb
ALTER USER username WITH PASSWORD 'new_password';

# Replace 'username' with the target user and 'new_password' with the desired password.

# Step 6: Revert changes in pg_hba.conf to secure the connection
sudo nano /var/lib/edb/as<version>/data/pg_hba.conf

# Change the line back to:
# host    all             all             127.0.0.1/32            md5

# Save and exit the file.

# Step 7: Restart the server again
sudo systemctl restart edb-as<version>

# Password reset is complete, and the authentication method is secured.
```

Replace `<version>`, `username`, and `new_password` with appropriate values. Let me know if you need further assistance!

<hr>

> # For **EnterpriseDB (EDB)**, the process to change the database user password is quite similar to PostgreSQL. Here's how you can do it:

```bash
# Step 1: Log in to the EDB database as a superuser
psql -U enterprisedb -d edb

# Replace 'enterprisedb' with your superuser name
# Replace 'edb' with the database name if different

# Step 2: Change the user's password
ALTER USER username WITH PASSWORD 'new_password';

# Replace 'username' with the target database user
# Replace 'new_password' with the desired password

# Step 3: Verify the password change by logging in with the updated credentials
psql -U username -h localhost -d database_name

# Replace 'username' and 'database_name' with appropriate values

# Step 4: If using PgBouncer, update the userlist.txt file
sudo nano /etc/pgbouncer/userlist.txt

# Add or update the line with:
# "username" "new_password"

# Step 5: Restart PgBouncer to apply changes
sudo systemctl restart pgbouncer
```

Replace placeholders like `username` and `new_password` with your actual values.

<hr>

> # COMMANDS

check all running services on Linux, formatted in a code box:

```bash
# For Systemd (modern Linux distributions)
systemctl list-units --type=service --state=running

# To list all services (running, stopped, or failed)
systemctl list-units --type=service

# To check detailed status of a specific service
systemctl status <service_name>

# For SysVinit (older Linux distributions)
service --status-all | grep running

# To check the status of a specific service
service <service_name> status

# For Upstart (used in some older Ubuntu versions)
initctl list

# To check the status of a specific service
initctl status <service_name>
``` 

Replace `<service_name>` with the name of the service you want to inspect.

<hr>

To allow ports `9990` and `8080` through the firewall in Linux, use the appropriate commands depending on your firewall management tool. Below are examples for **UFW**, **firewalld**, and **iptables**:

### For UFW (Uncomplicated Firewall)
```bash
# Allow port 9990
sudo ufw allow 9990

# Allow port 8080
sudo ufw allow 8080

# Reload the firewall to apply changes (if necessary)
sudo ufw reload

# Check the status to confirm
sudo ufw status
```

---

### For firewalld
```bash
# Allow port 9990
sudo firewall-cmd --permanent --add-port=9990/tcp

# Allow port 8080
sudo firewall-cmd --permanent --add-port=8080/tcp

# Reload the firewall to apply changes
sudo firewall-cmd --reload

# Check the status to confirm
sudo firewall-cmd --list-ports
```

---

### For iptables
```bash
# Allow port 9990
sudo iptables -A INPUT -p tcp --dport 9990 -j ACCEPT

# Allow port 8080
sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT

# Save the changes (varies by distribution)
sudo iptables-save > /etc/iptables/rules.v4

# Check the current rules
sudo iptables -L -n -v
```

<hr>

> # The `netstat` output confirms that PgBouncer is listening on port `6432` for both IPv4 and IPv6. However, your `psql` connection attempt still fails. Let’s investigate further.

---

### **Next Steps**

#### **1. Verify Network Connectivity**
   - Ensure the client machine (from where you're running `psql`) can reach the PgBouncer server:
     ```bash
     ping 192.168.237.5
     ```
   - If `ping` fails, the network configuration between the client and server needs to be fixed.

#### **2. Verify Firewall Rules**
   - Ensure the firewall is not blocking the connection:
     ```bash
     sudo firewall-cmd --list-ports
     ```
   - If port `6432` is not listed, allow it:
     ```bash
     sudo firewall-cmd --permanent --add-port=6432/tcp
     sudo firewall-cmd --reload
     ```

#### **3. Verify `pgbouncer.ini` Configuration**
   - Check the `pgbouncer.ini` file to ensure the database is configured correctly:
     ```bash
     vi /etc/pgbouncer/pgbouncer.ini
     ```
   - Under the `[databases]` section, verify that the database mapping is correct. For example:
     ```ini
     [databases]
     postgres = host=127.0.0.1 port=5432 dbname=postgres
     ```
     - Ensure the `host` and `port` are correctly pointing to the PostgreSQL server.

   - Save and restart PgBouncer:
     ```bash
     sudo systemctl restart pgbouncer
     ```

#### **4. Verify PostgreSQL Accessibility**
   - Test connecting directly to PostgreSQL from the PgBouncer server:
     ```bash
     psql -h 127.0.0.1 -U enterprisedb -d postgres -p 5432
     ```
   - If this fails, check the PostgreSQL configuration:
     - Verify `pg_hba.conf` allows connections:
       ```bash
       vi /var/lib/pgsql/data/pg_hba.conf
       ```
       Add an entry like:
       ```
       host    all     all     0.0.0.0/0     md5
       ```
     - Ensure PostgreSQL is listening on all interfaces by checking `postgresql.conf`:
       ```bash
       vi /var/lib/pgsql/data/postgresql.conf
       ```
       Update:
       ```ini
       listen_addresses = '*'
       ```
     - Restart PostgreSQL:
       ```bash
       sudo systemctl restart postgresql
       ```

#### **5. Verify PgBouncer Logs**
   - Check the PgBouncer logs for detailed errors:
     ```bash
     tail -f /var/log/pgbouncer/pgbouncer.log
     ```

#### **6. Retest the Connection**
   - Retry the `psql` connection through PgBouncer:
     ```bash
     psql -p 6432 -h 192.168.237.5 -U enterprisedb -d postgres
     ```

---

### **Expected Outcome**
- If everything is configured correctly, the `psql` command should succeed.
- If it still fails, share the relevant error messages from the PgBouncer and PostgreSQL logs for further troubleshooting.
