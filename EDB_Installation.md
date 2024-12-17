---

> # Download and Install EDB  
[Official Download Link](https://www.enterprisedb.com/software-downloads-postgres)

### File to Download
```
el_8_x86_64.tar.gz
```

### Move the File to `/opt` Folder and Extract
```
mkdir edb && tar -zxvf el_8_x86_64.tar.gz -C edb
```

### Count the Number of Files in the Directory
```
ls | wc -l
```

### Check EDB Version and Related Files
```
ls | grep edb
```

### Install EDB Files  
#### Search for the Required RPM Files:
```
ls | grep edb-as15-server
ls | grep edb-as16-server
ls | grep edb-as17-server
```

#### For Installation the EDB run this RPM Package:
```
yum install edb-as17-server-17.2.0-1.el8.x86_64.rpm
```

If you encounter conflicts during installation, run:
```
ps -ef | grep "conflict filename"
sudo yum install "all conflict filenames separated by spaces" -y
```

---

### Configure EDB Data Directories  

```
export PGDATA=/pgdata/data
export PGDATABASE=edb
export PGLOG=/var/log/edb/as15
export PATH=/usr/edb/as15/bin:\$PATH
```

```
cd - /var/lib/edb
```

#### Create Data and WAL Directories:
```
mkdir /pgdata/data/
mkdir /pgwaldata
```

#### Initialize the Database with Data Checksums:
```
/usr/edb/as15/bin/initdb --data-checksums --auth-local=peer --encoding=UTF-8 --waldir=/pgwaldata/wal
```

#### Modify the Service Configuration:
```
cp /usr/lib/systemd/system/edb-as-15.service /etc/systemd/system/edb-as-15.service
sed -i 's#/var/lib/edb/as15/data#/pgdata/data#g' /etc/systemd/system/edb-as-15.service
```

#### Reload and Start the Service:
```
systemctl daemon-reload
systemctl enable edb-as-15.service
systemctl start edb-as-15.service
systemctl status edb-as-15.service
```

#### Login as `enterprisedb` User:
```
su - enterprisedb
logout
cd $PGDATA
psql
```

<hr>

> # Useful Commands

### Find Resources by Name:
```
sudo find / -name httpd
find /opt/EDB -name psql
sudo systemctl list-units --type=service | grep edb
```

### Download Required Files:
```
jdk-23_linux-x64_bin.tar.gz
wildfly-34.0.1.Final.tar.gz
el_8_x86_64.tar.gz (EDB File)
```

### Install EDB Offline (RPM Files):  
1. Copy the RPM Files to Your System:
```
scp /path/to/edb-postgresql*.rpm user@rhel-system:/path/to/destination/
```

2. Install the RPM Locally:
```
sudo yum localinstall /path/to/edb-postgresql*.rpm
```

3. Alternatively, Use `rpm` for Manual Dependency Resolution:
```
sudo rpm -ivh /path/to/edb-postgresql*.rpm
```

4. Start and Enable PostgreSQL Service:
```
sudo systemctl start edb-as
sudo systemctl enable edb-as
```

5. Verify Installation:
```
psql --version
```

6. Login to PostgreSQL:
```
sudo -i -u postgres
psql
```

<hr>

> # Download and Install JBoss Wildfly  
[Official Download Link](https://www.wildfly.org/downloads/)

#### Extract Files:
```
mkdir EDB && tar -zxvf your_file.tar.gz -C EDB
```

### Necessary charging on this Locations:
1. **Deploy EAR Files**:  
   `/opt/jboss/wildfly-34.0.1.Final/standalone/deployments/`
2. **Server Configuration**:  
   `/opt/jboss/wildfly-34.0.1.Final/standalone/configuration/standalone.xml`
3. **Start Wildfly JBoss**:  
   `/opt/jboss/wildfly-34.0.1.Final/bin/add-user.sh`  
   `/opt/jboss/wildfly-34.0.1.Final/bin/standalone.sh`

#### Export path
```
export JAVA_HOME=/opt/jdk-23.0.1
export PATH=$JAVA_HOME/bin:$PATH

export JBOSS_HOME=/opt/jboss/wildfly-34.0.1.Final
export PATH=$JBOSS_HOME/bin:$PATH
```

#### Update Environment Variables:
```
vim /etc/profile
vim ~/.bashrc

source /etc/profile
source ~/.bashrc
```

### Run JBoss in the Background:
```
./standalone.sh &
```

#### Check or Kill Running Instances:
```
ps aux | grep standalone.sh
pgrep -f standalone.sh
kill -9 <PID>
```

#### Reattach to a Screen Session:
```
screen -r jboss
```


### Start pgAdmin:
```
systemctl start httpd
```

### Useful Links:
- [Video Tutorial 1](https://youtu.be/OnYyh6hVFTA?si=qm4tkzGsDZ7A-h7H)  - [Video Tutorial 2](https://youtu.be/1f6hROjDScw?si=7F0e5U2xiPUte4uA)  - [Video Tutorial 3](https://youtu.be/ZKzzK1CsN6I?si=N0u0OobujtVrAlZG)  - [Video Tutorial 4](https://youtu.be/BF0pbJ2nP1I?si=dhwvoedmAapZPvvJ)

--- 


<!---
> # Download and install EDB - [Link](https://www.enterprisedb.com/software-downloads-postgres)

### This File will download to this link
```
el_8_x86_64.tar.gz
```

### Move this file in /opt - folder and unzip use this cmd
```
mkdir edb && tar -zxvf el_8_x86_64.tar.gz -C edb
```
### Use cmd to count how many file have in directries 
```
ls | wc -l
```
## Check EDB version and files
```
ls | grep edb -as
```


###  First install first file name look like this use this cmd to search this file - `ls | grep edb-as15-server` - `edb-as17-server-17.2.0-1.el8.x86_64.rpm`
#### User this cmd find EDB which version you have and install all file use `yum install` - installation rpm will conflict EDB which file will conflict install that yum file 

```
ls | grep edb-as15-server
ls | grep edb-as16-server
ls | grep edb-as17-server
```

```
yum install edb-as17-server-17.2.0-1.el8.x86_64.rpm
After running this cmd you will get all conflict name install of that also
ps -ef |grep "conflict filename"
sudo yum install "all conflict file name with space" -y 
```

### Make Mount point for EDB data
```
vi .bash_profile
```

/usr/edb/as15/initdb

mkdir /pgdata/data/
mkdir /pgwaldata

```
/usr/edb/as15/bin/initdb --data-checksums --auth-local=peer --encoding=UTF-8 --waldir=/pgwaldata/wal
cp /usr/lib/systemd/system/edb-as-15.service /etc/systemd/system/edb-as-15.service
sed -i 's#/var/lib/edb/as15/data#/pgdata/data#g' /etc/systemd/system/edb-as-15.service

systemctl daemon-reload
systemctl enable edb-as-15.service
 
/etc/systemd/system/multi-user.target.wants/edb-as-15.service → /etc/systemd/system/edb-as-15.service.
 
systemctl start edb-as-15.service
systemctl status edb-as-15.service
```
```
su - enterprisedb
logout
cd $PGDATA
psql
```

<hr>

> # Download and install Jboss wildfly [Link](https://www.wildfly.org/downloads/)

### find anything by name

```
sudo find / -name httpd
find /opt/EDB -name psql
sudo systemctl list-units --type=service | grep edb
```
### Downlaod 
```
jdk-23_linux-x64_bin.tar.gz
wildfly-34.0.1.Final.tar.gz

el_8_x86_64.tar.gz - EDB File
```
### EDB installation 
```
sudo yum install -y ./*.rpm | tee installation_log.txt
sudo yum install -y /path/to/edb-postgresql*.rpm
psql --version
rpm -qa | grep -i edb
```

### Jboss run in Background
```
./standalone.sh &

Check  and kill
ps aux | grep standalone.sh
pgrep -f standalone.sh
kill -9 <PID>

screen -r jboss
```

### Commands 
```
tar -zxvf
mkdir EDB && tar -zxvf your_file.tar.gz -C EDB
```

```
vim /etc/profile
vim ~/.bashrc

source /etc/profile
source ~/.bashrc
```
```
export JAVA_HOME=/opt/jdk-23.0.1
export PATH=$JAVA_HOME/bin:$PATH

export JBOSS_HOME=/opt/jboss/wildfly-34.0.1.Final
export PATH=$JBOSS_HOME/bin:$PATH
```

```
sudo systemctl list-units --type=service --state=running
```

### Start pgAdmin
```
systemctl start httpd
```

### Important locations
```
- Deploy EAR file - /opt/jboss/wildfly-34.0.1.Final/standalone/deployments/
- Eanter IP Address of you Server Machince - /opt/jboss/wildfly-34.0.1.Final/standalone/configuration/standalone.xml
- Start Wildfly Jboss - /opt/jboss/wildfly-34.0.1.Final/bin/add-user.sh
- Start Wildfly Jboss - /opt/jboss/wildfly-34.0.1.Final/bin/standalone.sh
```

- [Link-1](https://youtu.be/OnYyh6hVFTA?si=qm4tkzGsDZ7A-h7H) - [Link-2](https://youtu.be/1f6hROjDScw?si=7F0e5U2xiPUte4uA) - [Link-3](https://youtu.be/ZKzzK1CsN6I?si=N0u0OobujtVrAlZG) - [Link-4](https://youtu.be/BF0pbJ2nP1I?si=dhwvoedmAapZPvvJ)

<hr>

### [EDB installation offline .rpm files](https://www.enterprisedb.com/docs/epas/latest/installing/linux_x86_64/epas_rhel_8/)

```
# Step 1: .rpm files ko system par copy karna (example ke liye scp)
scp /path/to/edb-postgresql*.rpm user@rhel-system:/path/to/destination/

# Step 2: .rpm files ko local system par install karna
sudo yum localinstall /path/to/edb-postgresql*.rpm

# Alternatively, using rpm (agar dependencies manually resolve karni hain)
sudo rpm -ivh /path/to/edb-postgresql*.rpm

# Step 3: PostgreSQL service start karna
sudo systemctl start edb-as
sudo systemctl enable edb-as

# Step 4: Installation verify karna
psql --version

# Step 5: PostgreSQL configuration ke liye login karna
sudo -i -u postgres
psql
```
<hr>
--->

