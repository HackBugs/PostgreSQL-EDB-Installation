> ## In RHEL (Red Hat Enterprise Linux), `ufw` (Uncomplicated Firewall) is not typically used by default. Instead, **firewalld** is the default firewall management tool on RHEL-based systems. However, if you're trying to use `ufw`, you might need to install and configure it first.

### Option 1: Using **firewalld** (Default in RHEL)

To allow port 9043/tcp in **firewalld**, follow these steps:

1. **Check if firewalld is running:**
   ```bash
   sudo systemctl status firewalld
   ```
   If it is not running, start it with:
   ```bash
   sudo systemctl start firewalld
   ```

2. **Allow the port 9043/tcp:**
   ```bash
   sudo firewall-cmd --permanent --add-port=9043/tcp
   ```

3. **Reload firewalld to apply changes:**
   ```bash
   sudo firewall-cmd --reload
   ```

4. **Verify that the port is open:**
   ```bash
   sudo firewall-cmd --list-all
   ```

### Option 2: Installing and Using **ufw** (If you specifically need `ufw`)

1. **Install `ufw` on RHEL:**
   If `ufw` is not installed, you can install it using the following command:
   ```bash
   sudo yum install ufw
   ```

2. **Enable `ufw`:**
   After installing `ufw`, enable and start the service:
   ```bash
   sudo systemctl enable ufw
   sudo systemctl start ufw
   ```

3. **Allow port 9043/tcp:**
   ```bash
   sudo ufw allow 9043/tcp
   ```

4. **Check the status of `ufw`:**
   ```bash
   sudo ufw status
   ```

---

## Using firewalld:

```
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8443/tcp --permanent
sudo firewall-cmd --zone=public --add-port=9990/tcp --permanent
sudo firewall-cmd --reload
```

## Using iptables:

```
sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8443 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 9990 -j ACCEPT
sudo service iptables save
```

## Using ufw:

```
sudo ufw allow 8080/tcp
sudo ufw allow 8443/tcp
sudo ufw allow 9990/tcp
sudo ufw enable
```

