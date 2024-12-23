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
