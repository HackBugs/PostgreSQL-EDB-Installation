> # Cheat sheet in simple language for the mentioned software, explaining what they are used for, their websites, and why they are useful:

| Software           | Used For                                    | Website                         | Why Use It (Layman Language)                        |
|--------------------|---------------------------------------------|---------------------------------|---------------------------------------------------|
| **EDB (EnterpriseDB)** | Database management, like storing and managing data. | [enterprisedb.com](https://www.enterprisedb.com) | Helps manage large amounts of data safely and efficiently. |
| **JBoss**           | Running Java-based applications.             | [jboss.org](https://www.jboss.org) | Makes it easy to run and manage complex Java apps.  |
| **Red Hat Linux**   | Operating system for servers and workstations. | [redhat.com](https://www.redhat.com) | Secure and reliable OS for business environments.   |
| **Ubuntu**          | Free and user-friendly operating system.     | [ubuntu.com](https://www.ubuntu.com) | Easy to use, great for both personal and server use.|
| **Oracle Linux**    | Enterprise-grade operating system.           | [oracle.com](https://www.oracle.com) | Stable and secure OS, often used for Oracle databases.|
| **Kali Linux**      | Cybersecurity and penetration testing.       | [kali.org](https://www.kali.org) | Used by security experts to test system security.   |
| **Zabbix**          | Monitoring servers and networks.             | [zabbix.com](https://www.zabbix.com) | Keeps an eye on systems to avoid downtime or issues.|
| **Kayclock**        | Time-tracking and productivity management.   | N/A                             | Helps track work hours and improve productivity.    |
| **Qlik Sense**      | Data visualization and business analytics.   | [qlik.com](https://www.qlik.com) | Turns raw data into easy-to-understand charts.      |
| **Qlik Admin (QMC)**| Managing Qlik servers and configurations.    | [qlik.com](https://www.qlik.com) | Admin tool for controlling Qlik applications.       |
| **Skyhigh (Networking)** | Cloud security and network management. | [skyhighsecurity.com](https://www.skyhighsecurity.com) | Protects data while using cloud services.           |
| **Redmine**         | Project management and issue tracking.       | [redmine.org](https://www.redmine.org) | Organizes tasks and tracks project progress.        |
| **HCL iNotes**      | Email and collaboration service.             | [hcltech.com](https://www.hcltech.com) | Email platform for businesses to communicate.       |
| **MQTT Client**     | Messaging for IoT devices.                   | [mqtt.org](https://www.mqtt.org) | Connects smart devices for communication.           |
| **WebLogic**        | Hosting and managing Java applications.      | [oracle.com](https://www.oracle.com) | Runs complex enterprise Java apps reliably.         |
| **Tuxedo**          | Managing high-performance applications.      | [oracle.com](https://www.oracle.com) | Handles heavy workloads and transactions securely.  |

<hr>

> # Agar **192.16.14.11:8834** pe page open nahi ho raha aur **Skyhigh** aa raha hai, to yeh kuch cheezein ho sakti hain:

### **1. Skyhigh Security Portal**
- **Skyhigh** ek cloud security service hai jo network traffic ko monitor aur secure karta hai. Agar aapke network mein Skyhigh installed hai, toh woh intercept kar sakta hai aapke HTTP requests ko aur **Skyhigh** ka page show kar sakta hai jab aap URL open karte ho.
- Iska matlab hai ki aapke network ke security layer (jaise firewall ya proxy) ko Skyhigh manage kar raha hai, aur woh aapko **Skyhigh** ka page dikha raha hai.

### **2. Tenable Nessus Essentials** (Agar page open ho raha hai)
- Agar aap **192.16.14.11:8834** pe page open kar pa rahe ho aur **"Tenable Nessus Essentials"** ka page aa raha hai, toh iska matlab hai ki aap successfully **Nessus Essentials** interface ko access kar rahe ho.
- **Nessus Essentials** ka default port 8834 hota hai, aur yeh web-based interface hai jahan se aap vulnerability scans aur reports dekh sakte ho.
  - Jab aap open karte ho, toh **Nessus Essentials** ka login page ya dashboard dikhega, jahan se aap scan setup kar sakte ho, reports dekh sakte ho, aur other security-related tasks perform kar sakte ho.

### **Possible Solutions:**

1. **Skyhigh Security Interception:**
   - Agar Skyhigh security solution ko bypass karna hai, toh aapko apne network administrator se baat karni padegi, jo Skyhigh ko configure karte hain. Woh aapko yeh confirm kar sakte hain ki Skyhigh traffic ko block ya intercept na kare.
   
2. **Nessus Essentials Interface Issue:**
   - Agar aapko **Tenable Nessus Essentials** ka interface dikha raha hai, toh yeh issue nahi hai. Aapko bas **Nessus Essentials** ko properly configure karna hoga.
   - Login karte waqt, apne credentials (agar setup kiye hain) use karein aur phir scanning start kar sakte hain.

### **Summary**:
- Agar Skyhigh aa raha hai, toh iska matlab hai aapke network ke security system ne traffic ko intercept kiya hai.
- Agar **Tenable Nessus Essentials** ka page aa raha hai, toh aap sahi tarike se interface pe pahuch chuke ho. Yahan se aap vulnerability scanning aur reporting ka kaam kar sakte ho.

Agar aapko further help chahiye toh network admin se check karwa lein ke Skyhigh ko bypass kaise karen ya properly configure kaise karein.

<hr>

> # **Skyhigh Security Install:** and **Tenable Nessus Essentials Install:**

### **Skyhigh Security Install:**

**Skyhigh Security** ek cloud-native security solution hai jo network traffic ko monitor karta hai aur security policies enforce karta hai. Yeh typically cloud environments ya enterprise-level security solutions mein use hota hai. Agar aapko apne system pe **Skyhigh** install karna hai, toh aapko generally **Skyhigh Cloud Security** ya **Skyhigh CASB (Cloud Access Security Broker)** ka use karna hoga.

#### **Skyhigh Security Install Steps**:
1. **Sign up and Access Skyhigh**:
   - Sabse pehle aapko Skyhigh ka account banane ke liye unki official website pe jaana hoga.
   - **Skyhigh Cloud Security** ka use karne ke liye aapko unki website par sign up karna padega aur demo ya trial account create karna hoga.

2. **Skyhigh Configuration**:
   - Jab aapka account ready ho, uske baad **Skyhigh** ko aapke network ya system ke liye configure karna hoga.
   - Skyhigh ki configuration mein aapko network traffic ko monitor karne ke liye policies set karni hoti hain.
   
3. **Skyhigh Installation**:
   - **Skyhigh** ko install karne ka process network environment pe depend karta hai (cloud ya on-premise). 
   - Agar aapke paas **on-premise** setup hai, toh aapko Skyhigh ke agents ko apne network devices pe install karna padega, jo traffic ko intercept karenge.

4. **Verify Installation**:
   - Installation complete hone ke baad, Skyhigh ko verify karna hoga. Aapko **Skyhigh Security Dashboard** se logs aur alerts check karne honge.

#### **Skyhigh ka Purpose**:
Skyhigh ko install karne ka main purpose cloud security ko manage karna aur aapke network ke through hone wale data transfers ko secure karna hai.

---

### **Tenable Nessus Essentials Install:**

**Tenable Nessus Essentials** ek free vulnerability scanner hai jo aapko aapke systems ko scan karne aur security vulnerabilities ko identify karne mein madad karta hai. Yeh small networks ke liye perfect hai, especially agar aapko 16 IPs tak scan karna ho.

#### **Steps to Install Tenable Nessus Essentials**:

1. **Download Nessus Essentials**:
   - **Tenable Nessus Essentials** ko download karne ke liye aapko **Tenable** ki official website se installer file download karni hogi.
     - Go to: [Tenable Nessus Downloads](https://www.tenable.com/products/nessus/nessus-essentials) and select your OS (Windows, macOS, Linux).

2. **Install Nessus Essentials**:
   - **Linux**:
     - Agar aap Ubuntu pe install kar rahe ho, toh aap terminal mein yeh commands use kar sakte ho:
       ```bash
       sudo dpkg -i Nessus-<version>.deb
       sudo systemctl start nessusd
       sudo systemctl enable nessusd
       ```
   - **Windows/macOS**: 
     - Installer file ko download karne ke baad, normal installation process follow karein (next, next, install).

3. **Activate Nessus Essentials**:
   - Install hone ke baad, browser mein **`https://localhost:8834`** URL open karen.
   - Aapko **Tenable Account** se activation code milega, jise aapko input karna hoga to activate **Nessus Essentials**.

4. **Configure Nessus Essentials**:
   - Nessus ko configure karne ke liye aapko apne network devices (IPs) ko scan karne ke liye target define karna padega.
   - Scan templates choose karen aur vulnerability scanning start karein.

5. **Running Scans**:
   - Nessus Essentials aapko vulnerabilities ko scan karne aur reports generate karne ka option dega.
   - Aap scan results ko analyze karke prioritize kar sakte hain ki kaunse vulnerabilities ko fix karna hai.

#### **Important Notes for Nessus Essentials**:
- Nessus Essentials ka free version **16 IPs tak scan kar sakta hai**.
- Agar aapko large scale scanning karni ho, toh **Nessus Professional** ki license kharidni padegi.

---

### **Summary**:
- **Skyhigh Security** ko install karne ke liye, aapko unki official website se access lena padega, account create karna padega aur network security ke liye configuration set karni padegi.
- **Tenable Nessus Essentials** ko install karne ke liye aapko Tenable ki website se Nessus Essentials ka installer download karna hoga, usse install karna hoga, aur activation code se activate karna hoga. Uske baad aap system vulnerabilities scan kar sakte ho.

Agar aapko specific configuration ya installation steps mein help chahiye ho, toh bataiye!
