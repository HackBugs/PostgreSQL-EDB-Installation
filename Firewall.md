```
# Step 1: Start the Firewall
sudo systemctl start firewalld

# Enable Firewall to start on boot
sudo systemctl enable firewalld

# Check Firewall status
sudo systemctl status firewalld

# Step 2: Allow Necessary Ports
# Allow PgBouncer port (Default: 6432)
sudo firewall-cmd --add-port=6432/tcp --permanent

# Allow SSH port (Default: 22)
sudo firewall-cmd --add-service=ssh --permanent

# Apply the changes
sudo firewall-cmd --reload

# Verify allowed ports
sudo firewall-cmd --list-ports

# Step 3: Allow Specific IP (Optional)
# Replace "192.168.237.1" with your specific IP
sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="192.168.237.1" port port=6432 protocol=tcp accept' --permanent
sudo firewall-cmd --reload

sudo firewall-cmd --add-port=6432/tcp --permanent
sudo firewall-cmd --reload

# Step 4: Troubleshooting
# View Firewall logs
sudo journalctl -u firewalld

# Test Port Accessibility from Remote
# Replace "192.168.237.5" with your server IP
nc -zv 192.168.237.5 6432

```
