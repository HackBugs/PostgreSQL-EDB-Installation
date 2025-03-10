Here’s a well-organized version with a title and proper headings:  

---

# **JBoss Service Configuration and Management on Linux**

## **1. Create and Configure the JBoss Service**  

1. Navigate to the systemd directory:  
   ```bash
   cd /etc/systemd/system/
   ```
2. Create a service file for JBoss:  
   ```bash
   touch jboss.service
   ```
3. Edit the `jboss.service` file:  
   ```bash
   vi jboss.service
   ```
4. Check java location cmd
   ```bash
   which java
   ``` 
5. Add the following configuration:  

```
[Unit]
Description=JBoss Application Server
After=network.target

[Service]
User=jboss
Group=jboss
Environment="JAVA_HOME=/opt/jdk-23.0.1"
Environment="JBOSS_HOME=/opt/jboss/wildfly-34.0.1.Final"
WorkingDirectory=/opt/jboss/wildfly-34.0.1.Final
ExecStart=/opt/jboss/wildfly-34.0.1.Final/bin/standalone.sh -b 0.0.0.0
ExecStop=/opt/jboss/wildfly-34.0.1.Final/bin/jboss-cli.sh --connect --command=:shutdown
TimeoutStartSec=100
TimeoutStopSec=100
Restart=on-failure
Environment="JBOSS_HOME=/opt/jboss/wildfly-34.0.1.Final"
#Environment="JAVA_HOME=/path/to/java"   # Make sure this points to the correct Java installation


[Install]
WantedBy=multi-user.target
```

---

## **2. Create a User and Group for JBoss**  

1. Create a `jboss` group:  
   ```bash
   sudo groupadd jboss
   ```
2. Create a `jboss` user and add it to the `jboss` group:  
   ```bash
   sudo useradd -g jboss -m jboss
   ```
3. Verify the user details:  
   ```bash
   id jboss
   ```
4. Set proper permissions for the JBoss directory:  
   ```bash
   sudo chown -R jboss:jboss /opt/jboss/wildfly-34.0.1.Final
   ```

---

## **3. Start and Manage the JBoss Service**  

1. Reload the systemd daemon:  
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable jboss
   sudo systemctl start jboss
   systemctl status jboss
   ```
2. Restart JBoss if needed:  
   ```bash
   sudo systemctl restart jboss
   ```

---

## **4. Troubleshooting JBoss Startup Issues**  

If JBoss fails to start, check the error logs using:  
```bash
sudo journalctl -u jboss -f
```

---

## **5. Important JBoss Directories**  

- **Binary files:** `/opt/jboss/wildfly-34.0.1.Final/bin`  
- **Deployment directory:** `/opt/jboss/wildfly-34.0.1.Final/standalone/deployments`  
- **Log files:** `/opt/jboss/wildfly-34.0.1.Final/standalone/log`

---

## **6. Additional Commands**  

- Change the default shell for the `jboss` user:  
  ```bash
  sudo usermod -s /bin/bash jboss
  ```
