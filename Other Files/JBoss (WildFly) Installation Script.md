

1. Create directory
 ```
 mkdir -p /opt/jboss
 ```
2. Download this
 ```
 jdk1.8.0_144
 jdk-8u144-linux-x64.tar.gz
 jboss-eap-7.0.0-installer.jar
 ```
3. Copy this file on this location
 ```
 cp jdk-8u144-linux-x64.tar.gz /opt/jboss/
 cp jboss-eap-7.0.0-installer.jar /opt/jboss/
 ```
4. Set Java Path
 ```
 vi ~/.bash_profile
 ```
 ```
 #!/bin/bash

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
  source ~/.bashrc
fi

# Set the JAVA_HOME environment variable
export JAVA_HOME=/opt/redhat/jdk1.8.0_144

# Update the PATH environment variable
export PATH=$JAVA_HOME/bin:$PATH:$HOME/bin

export PATH
```
5. execute file
```
source ~/.bash_profile
```
6. check java - version
```
java -version
```
7. execute installer use this cmd java -jar
```
java -jar jboss-eap-7.0.0-installer.jar
```
8. during installation save script file for silent install - and than go on jbos folder and run
```
./standalone.sh
```


- [Download jboss](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform)
- [Doc Jboss Installtion](https://docs.redhat.com/en/documentation/red_hat_jboss_enterprise_application_platform/6.4/html/getting_started_guide/sect-download_and_install_jboss_eap_using_the_graphical_installation_program#Run_the_JBoss_EAP_Installation_Program)


```
unzip example.zip -d example
```

- Set Environment variables - `sudo vim /etc/profile` and `source ~/.bashrc`
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk
export PATH=$JAVA_HOME/bin:$PATH

export JAVA_HOME="/usr/lib/jvm/java-11-openjdk-11.0.25.0.9-2.el8.x86_64/"
export PATH=$PATH:$JAVA_HOME/bin
```

<hr>

# **JBoss** application server ko RHEL (Red Hat Enterprise Linux) par install aur configure karne ke liye niche diye gaye steps ko follow karein:

---

### **Step 1: Prerequisites Check Karein**
1. **Java Install Karein**:
   JBoss ko chalane ke liye Java Development Kit (JDK) ka installed aur configured hona zaroori hai. 

   ```bash
   sudo yum install java-11-openjdk-devel
   ```
- After this
   ```
   sudo alternatives --config java
   ```

   Java version confirm karein:
   ```bash
   java -version
   ```

2. **Firewall and Ports Configuration**:
   JBoss default ports (8080, 9990) ko firewall me allow karein:
   ```bash
   sudo firewall-cmd --add-port=8080/tcp --permanent
   sudo firewall-cmd --add-port=9990/tcp --permanent
   sudo firewall-cmd --reload
   ```

---

### **Step 2: JBoss ZIP File Extract Karein**
1. **Download Location Par Navigate Karein**:
   JBoss ZIP file jis folder me downloaded hai, wahan jaayein:
   ```bash
   cd /path/to/downloaded/zip
   ```

2. **Extract ZIP File**:
   ZIP file ko extract karein:
   ```bash
   unzip jboss-eap-*.zip -d /opt/
   ```

   Agar `unzip` command nahi hai, to install karein:
   ```bash
   sudo yum install unzip
   ```

3. **JBoss Folder Rename Karein (Optional)**:
   Extracted folder ko rename karein:
   ```bash
   sudo mv /opt/jboss-eap-* /opt/jboss
   ```

---

### **Step 3: Environment Variable Set Karein**
1. **Java Path Ko Environment Variable Me Add Karein**:
   `/etc/profile` file me edit karein:
   ```bash
   sudo nano /etc/profile
   ```

   Niche lines add karein:
   ```bash
   export JAVA_HOME=/usr/lib/jvm/java-11-openjdk
   export PATH=$JAVA_HOME/bin:$PATH
   ```

2. **JBoss Path Ko Environment Variable Me Add Karein**:
   Us file me ye line bhi add karein:
   ```bash
   export JBOSS_HOME=/opt/jboss
   export PATH=$JBOSS_HOME/bin:$PATH
   ```

3. **Changes Apply Karein**:
   ```bash
   source /etc/profile
   ```

---

### **Step 4: JBoss Start Karein**
1. **Standalone Mode Me Start Karna**:
   Standalone mode ke liye:
   ```bash
   /opt/jboss/bin/standalone.sh
   ```

2. **Domain Mode Me Start Karna**:
   Agar domain mode chahiye:
   ```bash
   /opt/jboss/bin/domain.sh
   ```

3. **JBoss Admin Console Check Karein**:
   - Browser me open karein: `http://<your-server-ip>:9990`.
   - Default admin console available hoga.

---

### **Step 5: User Account Setup Karein (Admin Console Ke Liye)**
1. Admin user create karein:
   ```bash
   /opt/jboss/bin/add-user.sh
   ```

2. Instructions follow karein:
   - User type: `Management User`.
   - Username aur password set karein.
   - Configuration complete hone ke baad, ye admin console ke liye use hoga.

---

### **Step 6: JBoss Ko Background Me Run Karein (Optional)**
1. **SystemD Service Create Karein**:
   Service file banayein:
   ```bash
   sudo nano /etc/systemd/system/jboss.service
   ```

2. Content Add Karein:
   ```bash
   [Unit]
   Description=JBoss Application Server
   After=network.target

   [Service]
   User=root
   ExecStart=/opt/jboss/bin/standalone.sh
   ExecStop=/bin/kill -15 $MAINPID
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

3. **Service Enable Aur Start Karein**:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable jboss
   sudo systemctl start jboss
   ```

4. **Status Check Karein**:
   ```bash
   sudo systemctl status jboss
   ```

---

### **Step 7: Verify Installation**
- Check karein ki JBoss sahi se chal raha hai:
  ```bash
  curl http://localhost:8080
  ```

- Agar response mile to iska matlab setup successful hai.

<hr>

# Aapke system par FirewallD service chal nahi rahi hai, is wajah se `firewall-cmd` commands kaam nahi kar rahi hain. Is issue ko solve karne ke liye aapko FirewallD service ko enable aur start karna hoga, ya fir agar Firewall ka use nahi karna chahte hain, to iptables ya SELinux configurations ko adjust karna padega. Niche steps diye gaye hain:

---

### **Option 1: FirewallD Service Enable Aur Start Karein**
1. **FirewallD Service Ko Enable Karein**:
   ```bash
   sudo systemctl enable firewalld
   ```

2. **FirewallD Service Ko Start Karein**:
   ```bash
   sudo systemctl start firewalld
   ```

3. **Status Check Karein**:
   ```bash
   sudo systemctl status firewalld
   ```
   - Agar service active hai to output me "active (running)" dikhna chahiye.

4. **Ports Add Karein**:
   - Ab ports add karein:
     ```bash
     sudo firewall-cmd --add-port=8080/tcp --permanent
     sudo firewall-cmd --add-port=9990/tcp --permanent
     ```

5. **Firewall Rules Reload Karein**:
   ```bash
   sudo firewall-cmd --reload
   ```

6. **Rules Verify Karein**:
   ```bash
   sudo firewall-cmd --list-ports
   ```

---

### **Option 2: Firewall Band Karna (Agar Zarurat Na Ho)**
Agar aap Firewall service ka use nahi karna chahte hain, to ise permanently disable kar sakte hain:

1. **Firewall Stop Karein**:
   ```bash
   sudo systemctl stop firewalld
   ```

2. **Disable Firewall**:
   ```bash
   sudo systemctl disable firewalld
   ```

3. **Confirm Firewall Disabled Hai**:
   ```bash
   sudo systemctl status firewalld
   ```

---

### **Option 3: Iptables Use Karein (Alternative)**
Agar FirewallD ke bajaye iptables ka use karna chahte hain, to niche commands ka use karein:

1. **Ports Open Karein**:
   ```bash
   sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
   sudo iptables -A INPUT -p tcp --dport 9990 -j ACCEPT
   ```

2. **Iptables Rules Save Karein**:
   ```bash
   sudo service iptables save
   ```

3. **Changes Apply Karein**:
   ```bash
   sudo systemctl restart iptables
   ```

4. **Rules Verify Karein**:
   ```bash
   sudo iptables -L
   ```

---

### **Option 4: SELinux Ko Check Karein (Agar Applicable Ho)**
Agar SELinux enabled hai, to ports ko explicitly allow karna padega:

1. **SEManage Install Karein** (Agar installed nahi hai):
   ```bash
   sudo yum install policycoreutils-python-utils
   ```

2. **Ports Allow Karein**:
   ```bash
   sudo semanage port -a -t http_port_t -p tcp 8080
   sudo semanage port -a -t http_port_t -p tcp 9990
   ```

3. **SELinux Relabel Karein**:
   ```bash
   sudo restorecon -Rv /opt/jboss
   ```

4. **SELinux Status Check Karein**:
   ```bash
   sestatus
   ```

---

### **Recommendation**
Agar aap firewall ka use karte hain, to FirewallD ko start aur configure karein. Agar zarurat na ho, to Firewall disable karke configuration simple rakh sakte hain. 

<hr>

