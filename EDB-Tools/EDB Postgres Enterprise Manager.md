## Local Repo

```
yum install edb-as15-server-sslutils-1.3-1.el8.x86_64.rpm --disablerepo="*" --enablerepo="localrepo" -y
yum clean all
yum update -y
dnf repolist
rpm -ivh edb-as15-server-sslutils-1.3-1.el8.x86_64.rpm
```
- vi /etc/yum.repos.d/local.repo
```
[localrepo]
name=Local Repo
baseurl=file:///opt/edb
gpgcheck=0
enabled=1
```
---

### **Issue: YUM `localrepo` Metadata Missing, Createrepo Install Nahi Ho Raha**  
Aapka `yum` command `localrepo` se package install karne ki koshish kar raha hai, par `localrepo` abhi properly configured nahi hai. Aapko pehle **`localrepo` disable karke** `createrepo` install karna hoga.

---

### **✅ Fix Step-by-Step**  

#### **Step 1: Local Repo Temporarily Disable Karo**  
YUM `localrepo` ko use karne ki koshish kar raha hai jo abhi properly configured nahi hai. Isko **temporarily disable** karke install karein:  
```bash
yum install -y createrepo --disablerepo=localrepo
```

---

#### **Step 2: Repodata Generate Karo**  
Ab `/opt/edb` ke andar `repodata` generate karo:  
```bash
createrepo /opt/edb
```

Yeh command `/opt/edb/repodata/repomd.xml` generate karegi.

---

#### **Step 3: YUM Cache Clean & Refresh Karo**  
```bash
yum clean all
yum makecache
```

---

#### **Step 4: Ab Package Install Karo**
```bash
yum install edb-as15-server-sslutils-1.3-1.el8.x86_64.rpm --enablerepo=localrepo
```

---

### **⚡ Extra Debugging Agar Fir Bhi Error Aaye**  
✅ **Check Karo `repodata` generate hua ya nahi:**  
```bash
ls -l /opt/edb/repodata/
```

✅ **Check Karo `localrepo` configuration sahi hai ya nahi:**  
```bash
cat /etc/yum.repos.d/local.repo
```

✅ **SELinux issue to nahi hai:**  
```bash
getenforce  # Agar `Enforcing` hai to disable karo: setenforce 0
```

✅ **File Permissions sahi hain ya nahi:**  
```bash
chmod -R 755 /opt/edb
```

---

> # To set up a **local repository** in **RHEL-based Linux** (Red Hat, CentOS, Rocky Linux, AlmaLinux), follow these steps:

---

### **1. Mount the DVD or ISO (If using a local disk)**
If you're using an **ISO image**, mount it to `/mnt`:

```bash
mkdir -p /mnt/cdrom
mount /dev/cdrom /mnt/cdrom
```

If using an **ISO file**, mount it like this:

```bash
mkdir -p /mnt/iso
mount -o loop /path/to/your.iso /mnt/iso
```

---

### **2. Copy Repo Files (Optional)**
If you want to copy the repo locally for permanent usage:

```bash
mkdir -p /repo
cp -r /mnt/cdrom/BaseOS /repo/
cp -r /mnt/cdrom/AppStream /repo/
```

---

### **3. Create a Local Repo File**
Edit or create the repo file:

```bash
vi /etc/yum.repos.d/local.repo
```

Add the following content:

```
[redhat]
name=Local Repository
baseurl=file:///repo/AppStream
gpgcheck=0
enabled=1
```

If you are using an **ISO mount**, change the `baseurl`:

```
[localrepo]
name=Local Repo
baseurl=file:///mnt/iso/AppStream
gpgcheck=0
enabled=1
```

---

### **4. Clear & Verify Repo**
Run these commands to clear old metadata and verify the repo:

```bash
dnf clean all
dnf repolist
```

---

### **5. Install Packages from Local Repo**
Now, you can install packages using:

```bash
dnf install <package-name> --disablerepo="*" --enablerepo="redhat"
```
