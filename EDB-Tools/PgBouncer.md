```
vim /etc/edb/pgbouncer1.23/edb-pgbouncer-1.23.ini
admin_users = enterprisedb
stats_users = enterprisedb

Check log -
tail -f /var/log/edb/pgbouncer1.23/edb-pgbouncer-1.23.log

psql -h 192.168.237.5 -p 6432 -U enterprisedb -c "RELOAD;"

\du
edb=# CREATE USER enterprisedb;
edb=# ALTER USER enterprisedb WITH PASSWORD 'admin';
edb=# ALTER USER enterprisedb WITH SUPERUSER;
edb=# edb=# SELECT usename, passwd FROM pg_shadow;

Example -
echo -n "enterprisedb"mypassword | md5sum
"enterprisedb" "md5c9de1d8a0bd5c9a5e0bd6fcb9e37309f"

edb=# show max_connections;
```

```
ps aux | grep pgbouncer
sudo systemctl restart edb-pgbouncer-1.23
psql -h 192.168.237.5 -p 6432 -U pgbouncer -c "RELOAD;"

sudo firewall-cmd --zone=public --add-port=6432/tcp --permanent
sudo firewall-cmd --reload

ping 192.168.237.5
sudo netstat -tuln | grep 6432

psql -p 6432 -h 192.168.237.5 pgbouncer
psql -U pgbouncer -p 6432 -h 192.168.237.5 pgbouncer

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

sudo chmod 600 /etc/edb/pgbouncer1.23/userlist.txt
sudo chown enterprisedb:enterprisedb /etc/edb/pgbouncer1.23/userlist.txt
```

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
systemctl list-units --type=service

sudo systemctl start edb-pgbouncer-1.23.service
sudo systemctl status edb-pgbouncer-1.23.service
```
```
psql -h localhost -p 6432 -U username -d mydb
```

Here’s the entire process to reset the password for a user in EnterpriseDB if you've forgotten it:

```bash
# Step 1: Switch to the EnterpriseDB superuser
sudo su - enterprisedb

# Step 2: Try logging into the database
psql -U enterprisedb

# If login fails, proceed with the following steps to modify authentication.

# Step 3: Locate and edit the pg_hba.conf file
sudo nano /var/lib/edb/as<version>/data/pg_hba.conf

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
