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

If you are using **firewalld** (the default), then Option 1 is the recommended approach. If you prefer to use `ufw`, then Option 2 will allow you to install and configure it.
