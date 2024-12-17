
> ## **disable both `firewalld` and `iptables` (if applicable) on a RHEL system:**

```bash
#!/bin/bash
# Stop and disable firewalld (for RHEL 7+)
sudo systemctl stop firewalld && sudo systemctl disable firewalld
# Verify if firewalld is disabled
sudo systemctl status firewalld
# Check if firewall-cmd is running
sudo firewall-cmd --state || echo "firewalld is not running"
# Optionally remove firewalld
# sudo yum remove -y firewalld
# Stop and disable iptables (for RHEL 6 or if firewalld isn't in use)
sudo systemctl stop iptables && sudo systemctl disable iptables
# Verify if iptables is disabled
sudo systemctl status iptables
```
<hr>

> ## **enable and start the firewall** on a RHEL system (for both `firewalld` and `iptables` if applicable).

```bash
#!/bin/bash
# Start and enable firewalld (for RHEL 7+)
sudo systemctl start firewalld && sudo systemctl enable firewalld
# Verify if firewalld is running
sudo systemctl status firewalld
# Check if firewall-cmd is running
sudo firewall-cmd --state || echo "firewalld is not running"

# Start and enable iptables (for RHEL 6 or if firewalld isn't in use)
sudo systemctl start iptables && sudo systemctl enable iptables
# Verify if iptables is running
sudo systemctl status iptables
```

