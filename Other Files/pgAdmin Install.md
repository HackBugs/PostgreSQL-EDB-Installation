
# 1. **Install pgAdmin** - [Insrallation link](https://www.pgadmin.org/download/pgadmin-4-apt/)

1. Create file - 1.sh
2. Paste this code

```
#
# Setup the repository
#

# Install the public key for the repository (if not done previously):
curl -fsS https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo gpg --dearmor -o /usr/share/keyrings/packages-pgadmin-org.gpg

# Create the repository configuration file:
sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/packages-pgadmin-org.gpg] https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'

#
# Install pgAdmin
#

# Install for both desktop and web modes:
sudo apt install pgadmin4

# Install for desktop mode only:
sudo apt install pgadmin4-desktop

# Install for web mode only: 
sudo apt install pgadmin4-web 

# Configure the webserver, if you installed pgadmin4-web:
sudo /usr/pgadmin4/bin/setup-web.sh
```

<hr>

```
sudo systemctl status pgadmin4
```

### 1. pgAdmin 4 ko open karne ke liye terminal mein simply type karen:

```
pgadmin4
```

### 2. Agar aapne web mode install kiya hai, toh aapko pehle web server ko configure karna hoga. Aapne jo command run ki thi:

```
sudo /usr/pgadmin4/bin/setup-web.sh
```

### 3. Setup ke baad, aap pgAdmin ko web browser ke through access kar sakte hain. Aapko browser mein yeh URL type karna hoga:

```
http://127.0.0.1/pgadmin4
```

- Example
```
http://192.168.1.72/pgadmin4/login?next=/pgadmin4/browser/
```

```
http://localhost/pgadmin4
```

<hr>

# Insatll karne ka baad ye cmd use kare aur jo email id and password use kiye hai webiste pe o enter kare fir aap koe pkg install kar kakte hai `dnf` cmd se

[Install - pgAdmin in REHL](https://www.tecmint.com/install-postgresql-and-pgadmin-in-rhel-8/)

Aapko jo errors mil rahe hain unka matlab hai ki **python3-devel**, **libxml2-devel**, aur **libxslt-devel** packages aapke system ke repository mein nahi mil rahe hain. Yeh typically tab hota hai jab **Red Hat subscription** active nahi hota ya **Red Hat repositories** properly configure nahi hoti.

Is issue ko solve karne ke liye, aapko kuch steps follow karne honge:

### Step 1: **Register Your System with Red Hat Subscription**

Sabse pehle, aapko apne system ko Red Hat subscription ke saath register karna hoga, jisse Red Hat ke official repositories aapke system pe available ho sakein.

```bash
sudo subscription-manager register
```

Is command ke through aapko apna **Red Hat account** aur **subscription key** dena hoga. Agar aapke paas Red Hat subscription nahi hai, to aap **Developer Subscription** use kar sakte hain, jo free hai.

Agar aap already registered hain, to subscription ko attach karne ke liye yeh command use karein:

```bash
sudo subscription-manager attach --auto
```

### Step 2: **Enable Required Repositories**

Red Hat ke repositories ko enable karna hoga taaki aapko required packages mil sakein. Sabse pehle **AppStream** aur **BaseOS** repositories ko enable karein:

```bash
sudo subscription-manager repos --enable=codeready-builder-for-rhel-8-x86_64-rpms
sudo subscription-manager repos --enable=rhel-8-for-x86_64-appstream-rpms
sudo subscription-manager repos --enable=rhel-8-for-x86_64-baseos-rpms
```

### Step 3: **Install Missing Dependencies**

Ab, jab Red Hat ke repositories enable ho gaye hain, aap easily missing packages ko install kar sakte hain. Phir se dependencies install karne ki koshish karein:

```bash
sudo dnf install python3-devel libxml2-devel libxslt-devel -y
```

Is step se **python3-devel**, **libxml2-devel**, aur **libxslt-devel** packages install ho jaayenge.

### Step 4: **Install pgAdmin**

Agar dependencies successfully install ho gayi hain, to aap **pgAdmin** ko install kar sakte hain using the steps mentioned earlier.

1. **pgAdmin repository add karein** (agar abhi tak nahi kiya):

```bash
sudo nano /etc/yum.repos.d/pgadmin.repo
```

Add the following content:

```bash
[pgAdmin]
name=pgAdmin Repository
baseurl=https://yum.postgresql.org/pgadmin4/rhel/8/x86_64/
enabled=1
gpgcheck=1
gpgkey=https://yum.postgresql.org/RPM-GPG-KEY-PGDG
```

2. **pgAdmin install karein**:

```bash
sudo dnf install pgadmin4 -y
```

### Step 5: **Start pgAdmin**

1. **Desktop mode (GUI)**:

```bash
pgadmin4
```

2. **Web mode**:

```bash
sudo systemctl enable pgadmin4
sudo systemctl start pgadmin4
```

Ab aap pgAdmin ko **http://<your-server-ip>/pgadmin4** pe access kar sakte hain.

---

### Agar Subscription Manager ka Use Nahi Kar Rahe

Agar aapke paas Red Hat subscription nahi hai, ya aapko **subscription-manager** ka use nahi karna chahte, to aap **EPEL (Extra Packages for Enterprise Linux)** repository ko enable kar sakte hain, jo additional packages provide karta hai. Yeh steps follow karein:

1. **EPEL repository ko enable karein**:

```bash
sudo dnf install epel-release -y
```

2. **Missing dependencies ko install karein**:

```bash
sudo dnf install python3-devel libxml2-devel libxslt-devel -y
```

---

### Conclusion:
Agar aapke paas valid **Red Hat subscription** hai, to sabse pehle system ko register karen aur required repositories enable karein. Agar aapke paas subscription nahi hai, to EPEL repository se missing dependencies install karne ki koshish karein. Uske baad, pgAdmin ko install kar sakte hain.

## Install pgAdmin in REHL Script

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

<hr>

> ### Step-by-Step Process: PostgreSQL Database Install Karna

1. **System ko Update Karna:**
   Sabse pehle apne system ke package list ko update karein:
   ```bash
   sudo apt update
   ```

2. **PostgreSQL Install Karna:**
   Ab PostgreSQL aur uske required tools ko install karen:
   ```bash
   sudo apt install postgresql postgresql-contrib
   ```

   - **postgresql-contrib** package me kuch extra extensions aur tools hote hain jo PostgreSQL ke sath aate hain, jaise ki `pg_stat_statements` aur `pg_upgrade`.

3. **PostgreSQL Service ko Start Karna:**
   Installation ke baad, PostgreSQL service ko start karen:
   ```bash
   sudo systemctl start postgresql
   ```

4. **PostgreSQL Service ka Status Check Karna:**
   Ensure karen ki service successfully run ho rahi hai:
   ```bash
   sudo systemctl status postgresql
   ```

   Agar service running hai, toh output kuch is tarah dikhega:
   ```
   ● postgresql.service - PostgreSQL RDBMS
      Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
      Active: active (running) since Thu 2024-12-12 10:15:00 UTC; 1h 30min ago
   ```

5. **PostgreSQL Command Line Interface (CLI) ko Access Karna:**
   PostgreSQL installation ke baad, `postgres` user ko access karne ke liye aapko `psql` command line tool ka use karna hoga. `postgres` user ke under login karne ke liye:
   ```bash
   sudo -i -u postgres
   ```

   Ab aap PostgreSQL shell (`psql`) me login kar sakte hain:
   ```bash
   psql
   ```

   Ye aapko PostgreSQL shell me le jayega jahan aap SQL queries run kar sakte hain.

6. **PostgreSQL User aur Database Create Karna:**
   - **New User Banana:**
     ```bash
     createuser --interactive
     ```
     Is command ke through aap naya user create kar sakte hain.
   
   - **New Database Banana:**
     ```bash
     createdb mydatabase
     ```

   - **PostgreSQL Shell Se Exit Karna:**
     Agar aap PostgreSQL shell se bahar nikalna chahte hain, toh simply `exit` type karen.

7. **PostgreSQL ko Restart Karna (Agar zarurat ho):**
   Agar aapko configuration changes ke baad PostgreSQL ko restart karna ho:
   ```bash
   sudo systemctl restart postgresql
   ```

### PostgreSQL ko pgAdmin Se Access Karna:
Aapne jo **pgAdmin 4** install kiya hai, usse aap apne PostgreSQL server ko manage kar sakte hain:
1. **pgAdmin 4 ko open karein** (ya toh terminal se `pgadmin4` type karen, ya web mode ke liye `http://127.0.0.1/pgadmin4` browser me open karein).
2. **Server add karein**: Jab pgAdmin open ho, aapko apne PostgreSQL server ko add karna hoga (server ko connect karne ke liye `localhost` ko host ke roop mein set karein aur username `postgres` aur password set karein).
   
### Summary:
- Aapne **pgAdmin 4** install kiya hai jo ek GUI tool hai PostgreSQL ko manage karne ke liye.
- Aapko **PostgreSQL database server** alag se install karna padega (jo maine upar step-by-step bataya hai).
- Jab PostgreSQL server install ho jayega, tab aap usse pgAdmin ya `psql` command line tool se manage kar sakte hain.

<hr>

# 2. **REHL Minimal Install Without GUI**


