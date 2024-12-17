```
#!/bin/bash

# Print message for each step
echo "Starting WildFly (JBoss) installation process..."

# Step 1: Install Java (required for WildFly)
echo "Installing OpenJDK 17..."
sudo dnf install -y java-17-openjdk

# Step 2: Install necessary dependencies (if required)
echo "Installing dependencies..."
sudo dnf install -y wget unzip

# Step 3: Create WildFly user and group
echo "Creating WildFly user and group..."
sudo groupadd -r wildfly
sudo useradd -r -g wildfly -m -d /opt/wildfly -s /bin/bash wildfly

# Step 4: Download WildFly (latest stable release)
echo "Downloading WildFly..."
wget https://github.com/wildfly/wildfly/releases/download/26.0.1.Final/wildfly-26.0.1.Final.tar.gz -P /tmp

# Step 5: Extract WildFly to the /opt directory
echo "Extracting WildFly..."
sudo tar -xvzf /tmp/wildfly-26.0.1.Final.tar.gz -C /opt

# Step 6: Set permissions for the WildFly directory
echo "Setting permissions for WildFly directory..."
sudo chown -R wildfly:wildfly /opt/wildfly-26.0.1.Final

# Step 7: Create a symbolic link for easy access
echo "Creating symbolic link to WildFly..."
sudo ln -s /opt/wildfly-26.0.1.Final /opt/wildfly

# Step 8: Configure WildFly to run as a service
echo "Configuring WildFly as a systemd service..."

# Create the service file
sudo bash -c 'cat << EOF > /etc/systemd/system/wildfly.service
[Unit]
Description=WildFly Application Server
After=network.target

[Service]
User=wildfly
Group=wildfly
ExecStart=/opt/wildfly/bin/standalone.sh -b 0.0.0.0
ExecStop=/opt/wildfly/bin/jboss-cli.sh --connect --command=:shutdown
Restart=on-failure
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=wildfly

[Install]
WantedBy=multi-user.target
EOF'

# Step 9: Reload systemd and start WildFly service
echo "Reloading systemd and starting WildFly..."
sudo systemctl daemon-reload
sudo systemctl start wildfly
sudo systemctl enable wildfly

# Step 10: Verify WildFly status
echo "Verifying WildFly status..."
sudo systemctl status wildfly

# Step 11: Open WildFly port (default is 8080)
echo "Opening WildFly port 8080..."
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --reload

echo "WildFly (JBoss) installation and setup complete!"
```
