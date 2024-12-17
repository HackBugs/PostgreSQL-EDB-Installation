## Script Install pgAdmin on REHL
```
#!/bin/bash

# Print message for each step
echo "Starting pgAdmin4 installation process..."

# Step 1: Install EPEL Repository
echo "Installing EPEL Repository..."
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm

# Step 2: Install pgAdmin Repository
echo "Installing pgAdmin4 repository..."
sudo dnf install -y https://ftp.postgresql.org/pub/pgadmin/pgadmin4/yum/pgadmin4-redhat-repo-2-1.noarch.rpm

# Step 3: Update the repository cache
echo "Making repository cache..."
sudo dnf makecache

# Step 4: Install pgAdmin4
echo "Installing pgAdmin4..."
sudo dnf install -y pgadmin4

# Step 5: Verify pgAdmin4 installation
echo "Verifying pgAdmin4 installation..."
rpm -qi pgadmin4

# Step 6: Start and enable the httpd service (web server)
echo "Starting and enabling the httpd service..."
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd

# Step 7: Set up the pgAdmin4 web service
echo "Running pgAdmin4 web setup..."
sudo /usr/pgadmin4/bin/setup-web.sh

# Step 8: Install policycoreutils for SELinux management (if necessary)
echo "Checking for semanage command..."
sudo dnf provides /usr/sbin/semanage

# Step 9: Install policycoreutils-python-utils package (for SELinux policies)
echo "Installing policycoreutils-python-utils..."
sudo dnf install -y policycoreutils-python-utils

# Step 10: Running the pgAdmin4 web setup script again
echo "Running pgAdmin4 web setup again..."
sudo /usr/pgadmin4/bin/setup-web.sh

echo "pgAdmin4 installation and setup complete!"
```

## Script Jboss
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

<hr>

> ## Download Link

 ### **Recommended Download Options:**

1. **Compressed Archive (Manual Installation):**  
   Agar aap zip file se manually install karna chahte hain, to ye download karein:
   - **File:** `jdk-23_linux-x64_bin.tar.gz`  
   - **Link:** [Download Compressed Archive](https://download.oracle.com/java/23/latest/jdk-23_linux-x64_bin.tar.gz)  

2. **RPM Package (Simpler Installation):**  
   Agar aap RPM package prefer karte hain (jo ki RHEL aur CentOS ke liye asaan hai), to ye download karein:
   - **File:** `jdk-23_linux-x64_bin.rpm`  
   - **Link:** [Download RPM Package](https://download.oracle.com/java/23/latest/jdk-23_linux-x64_bin.rpm)
   
3.  **JBoss (WildFly) Download Options** 
- **Compressed Archive (Manual Installation)**:  
   - **File:** `wildfly-<version>.Final.zip`  
   - **Link:** [Download WildFly (JBoss) Compressed Archive](https://www.wildfly.org/downloads/)
 
<hr>
