> ## Aapke LAN (Local Area Network) me jitne devices connected hain, unka IP address ek saath dekhne ke liye aapko network scanning tools ya commands ka use karna hoga. Niche steps diye gaye hain jo aapko help karenge:

---
## Install RPM pkg
```
sudo rpm -ivh jdk-17_linux-x64_bin.rpm
```

### **Option 1: `arp -a` Command Ka Use Karein (Windows/Linux/Ubuntu)**
1. **Command Run Karein**:  
   - **Windows**: Command Prompt (CMD) open karein aur type karein:
     ```bash
     arp -a
     ```
   - **Linux/Ubuntu**: Terminal me type karein:
     ```bash
     arp -a
     ```

2. **Output Samajhein**:
   - Ye command aapke network ka ARP (Address Resolution Protocol) table dikhata hai, jisme connected devices ke IP aur MAC addresses listed hote hain.
   - Example output:
     ```
     Interface: 192.168.1.1
     Internet Address      Physical Address      Type
     192.168.1.2          00-14-22-01-23-45     dynamic
     192.168.1.3          00-15-5d-45-67-89     dynamic
     ```

---

### **Option 2: `nmap` Tool Ka Use Karein (Linux/Ubuntu/Windows)**
Agar aapko ek detailed list chahiye to **nmap** tool ka use karein.

#### Step 1: Install `nmap`
- **Ubuntu/Linux**:
  ```bash
  sudo apt install nmap
  ```
- **Windows**: [Nmap Official Website](https://nmap.org/download.html) se download karke install karein.

#### Step 2: Network Scan Karein
1. Apne network ka subnet pata karein:
   - Gateway IP dekhein:
     ```bash
     ip route | grep default
     ```
     Example output: `192.168.1.1`

2. Nmap Command Run Karein:
   ```bash
   nmap -sn 192.168.1.0/24
   ```
   - **`192.168.1.0/24`** me `192.168.1.0` aapke network ka subnet hai. Isse adjust karein agar aapka subnet alag ho.

3. Output Dekhein:
   - Ye connected devices ka IP aur hostname dikhayega:
     ```
     Nmap scan report for 192.168.1.2
     Host is up (0.0020s latency).
     MAC Address: 00:14:22:01:23:45

     Nmap scan report for 192.168.1.3
     Host is up (0.0015s latency).
     MAC Address: 00:15:5d:45:67:89
     ```

---

### **Option 3: Router Interface Ka Use Karein**
1. **Router IP Access Karein**:
   - Apne browser me router ka IP address open karein, jaise: `192.168.1.1` (Ye gateway IP hota hai).
   - Username aur password dalke login karein.

2. **Connected Devices Section Check Karein**:
   - Router ke admin panel me ek section hoga: **"Connected Devices"** ya **"DHCP Clients List"**.
   - Ye list aapko sabhi devices ke IP aur MAC addresses dikhayegi.

---

### **Option 4: Advanced Tools**
Agar aapko GUI chahiye ya aur advanced scanning karni hai:
- **Angry IP Scanner**:
  - [Download Angry IP Scanner](https://angryip.org/download/) aur run karein.
  - Ye tool connected devices ka IP address aur hostname list karega.

- **Advanced IP Scanner (Windows)**:
  - Is tool ko [yahan se download](https://www.advanced-ip-scanner.com/) karke install karein aur scan karein.

---

### Conclusion:
- **Quick Check**: Use `arp -a`.
- **Detailed List**: Use `nmap` ya GUI tools.
- **Router Access**: Connected devices ki list router se dekhein.
