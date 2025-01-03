```
JBCS rpm version is available via yum install (yum groupinstall jbcs-httpd24)

yum install httpd
yum install mod_ssl

chmod -R 755 /var/www/html
chown -R apache:apache /var/www/html

sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload

apachectl start
systemctl restart httpd
systemctl stop httpd
systemctl start httpd
systemctl status httpd

systemctl status firewalld.service
systemctl stop firewalld.service

vi /etc/httpd/conf/httpd.conf
cd /var/www/html
tail -n 20 /var/log/httpd/error_log
vi /etc/hosts

ps aux | grep httpd
lsof -i :80
netstat -tuln | grep :80
ss -tuln | grep :80
sudo kill -9 <PID>
```

<hr>

# 1. Scenario

> Prompt

> ## Mere paas ek website hai jo Java pe bani hui hai. Main EAR file deploy karta hoon JBoss par, aur wo do IP addresses pe run karti hai. Ek HTTPD server bhi hai jo alag IP par chal raha hai. Jab website par koi action hota hai, toh sabhi systems par data automatically update ho jata hai. Agar koi user kuch karta hai, toh wo change JBoss pe bhi reflect hota hai. Website jo dikh rahi hai, wo HTTPD server (domain ke IP) se access hoti hai, aur backend mein EDB database use hota hai.

---

### Aapka Setup
1. **Java-based Website**:  
   - Aapki website Java pe likhi gayi hai aur EAR (Enterprise Archive) file ke form me hai.
   - EAR file ko **JBoss Application Server** (jaise WildFly) par deploy kiya jata hai.

2. **Two IP Addresses for JBoss**:  
   - Aapka JBoss server do alag IP addresses pe bind hai, jo shayad clustering ya high availability ke liye use ho raha hai.

3. **HTTPD Server (Apache HTTP Server)**:  
   - Ek alag IP pe HTTPD server run ho raha hai. Ye main entry point hai jisme aapka domain bind hai.
   - HTTPD ko load balancer ya reverse proxy ke tarah use kiya ja raha hai, jo requests ko JBoss servers pe route karta hai.

4. **EDB Database**:  
   - EDB (EnterpriseDB) ek PostgreSQL-based database hai jo aapki website ke data ko manage karta hai.

---

### Aapke Use Case ka Flow
1. **Client Request Handling**:  
   - User ka request HTTPD server ke domain IP pe aata hai. HTTPD server is request ko JBoss servers (do IPs) me se kisi ek par forward karta hai.

2. **Dynamic Data Updates**:  
   - Jab user koi action leta hai (e.g., data update), to request JBoss server pe process hoti hai aur EDB database me changes save hote hain.
   - Kyunki saare JBoss instances ek hi EDB database ke saath linked hain, data sab jagah sync rahta hai.

3. **HTTPD Server as Main Domain**:  
   - HTTPD server ka domain IP sabse main hai aur client se connected hai. Users ko bas HTTPD server ka IP ya domain dikhai deta hai.

---

### Key Challenges aur Solutions

#### 1. **Data Synchronization**:
   - **Challenge**: Sabhi JBoss servers aur HTTPD par ek hi data dikhna chahiye.
   - **Solution**: 
     - EDB database use ho raha hai jo centralized hai, is wajah se data sync ho raha hai.
     - Ensure karo ki **session replication** enable hai agar clustering use kar rahe ho.

#### 2. **Load Balancing**:
   - **Challenge**: Load balance karte waqt user requests ko efficiently distribute karna.
   - **Solution**: 
     - HTTPD me `mod_proxy` ya `mod_jk` module use karke JBoss servers ke beech load balancing configure karo.
     - Example configuration (`httpd.conf`):
       ```apache
       <Proxy balancer://mycluster>
           BalancerMember http://192.168.1.100:8080
           BalancerMember http://192.168.1.101:8080
           ProxySet lbmethod=byrequests
       </Proxy>

       ProxyPass / balancer://mycluster/
       ProxyPassReverse / balancer://mycluster/
       ```

#### 3. **Failover Handling**:
   - **Challenge**: Agar ek JBoss server down ho jaye, to service impact na ho.
   - **Solution**: 
     - HTTPD me failover setup karo.
     - Ensure karo ki JBoss clustering properly configured ho.

#### 4. **Performance Optimization**:
   - **Challenge**: Multiple layers hone ki wajah se latency na aaye.
   - **Solution**:
     - HTTPD me caching enable karo (e.g., `mod_cache`).
     - JBoss me proper thread pool aur memory settings configure karo.

---

### Recommendations
1. **Monitoring and Logging**:
   - HTTPD aur JBoss dono me access logs aur error logs monitor karo.
   - Database ke health checks aur performance monitoring tools (jaise `pgAdmin` ya Prometheus) use karo.

2. **High Availability**:
   - Agar EDB database ek single instance hai, to uske liye bhi clustering setup karo (EDB ke replication features ka use karo).

3. **Scalability**:
   - Future me traffic badhne par aur JBoss instances add karne ke liye infrastructure ready rakho.

4. **SSL/TLS**:
   - HTTPD server pe SSL setup karo taaki domain secure ho (Let's Encrypt ya DigiCert ke certificates use karo).

<hr>

# 2. Scenario

> Real-life example

> ## Mere paas ek website hai jo Java pe bani hui hai. Main EAR file deploy karta hoon JBoss par, aur wo do IP addresses pe run karti hai. Ek HTTPD server bhi hai jo alag IP par chal raha hai. Jab website par koi action hota hai, toh sabhi systems par data automatically update ho jata hai. Agar koi user kuch karta hai, toh wo change JBoss pe bhi reflect hota hai. Website jo dikh rahi hai, wo HTTPD server (domain ke IP) se access hoti hai, aur backend mein EDB database use hota hai.

---

### Scenario:
1. **HTTPD Server**: IP: `192.168.0.10`  
   - **Role**: Ye user ke requests ko handle karega aur JBoss servers par forward karega.
   - Domain: `www.mywebsite.com`

2. **JBoss Servers**:  
   - **JBoss 1**: IP: `192.168.0.20`, Port: `8080`  
   - **JBoss 2**: IP: `192.168.0.30`, Port: `8080`  

3. **EDB Database**: IP: `192.168.0.40`, Port: `5432`  
   - **Role**: Centralized database jo data store karega.

---

### Step-by-Step Implementation

#### 1. **HTTPD Configuration**
- Install HTTPD:
  ```bash
  sudo yum install httpd -y
  sudo systemctl start httpd
  sudo systemctl enable httpd
  ```

- HTTPD ke config file (`/etc/httpd/conf/httpd.conf`) me load balancing aur proxy settings add karein:
  ```apache
  LoadModule proxy_module modules/mod_proxy.so
  LoadModule proxy_balancer_module modules/mod_proxy_balancer.so
  LoadModule proxy_http_module modules/mod_proxy_http.so

  <Proxy balancer://jbosscluster>
      BalancerMember http://192.168.0.20:8080
      BalancerMember http://192.168.0.30:8080
      ProxySet lbmethod=byrequests
  </Proxy>

  ProxyPass / balancer://jbosscluster/
  ProxyPassReverse / balancer://jbosscluster/
  ```

- HTTPD ko restart karein:
  ```bash
  sudo systemctl restart httpd
  ```

---

#### 2. **JBoss Setup**
- **JBoss 1 (192.168.0.20)** aur **JBoss 2 (192.168.0.30)** par EAR file deploy karein:
  1. JBoss ka standalone mode start karein:
     ```bash
     ./standalone.sh -b 192.168.0.20
     ./standalone.sh -b 192.168.0.30
     ```
  2. Admin console ke zariye EAR file upload karein:
     - URL: `http://192.168.0.20:9990` aur `http://192.168.0.30:9990`

---

#### 3. **EDB Database Setup**
- EDB database install karein aur uska ek schema banayein:
  ```bash
  sudo yum install edb-as11-server
  sudo /usr/edb/as11/bin/initdb -D /var/lib/edb/as11/data
  sudo systemctl start edb-as11
  ```

- Database user aur schema create karein:
  ```sql
  CREATE USER myuser WITH PASSWORD 'mypassword';
  CREATE DATABASE mydatabase OWNER myuser;
  ```

- JBoss me database configuration (`standalone.xml`) update karein:
  ```xml
  <datasource jndi-name="java:/jdbc/MyDS" pool-name="MyPool" enabled="true">
      <connection-url>jdbc:edb://192.168.0.40:5432/mydatabase</connection-url>
      <driver>postgresql</driver>
      <security>
          <user-name>myuser</user-name>
          <password>mypassword</password>
      </security>
  </datasource>
  ```

---

#### 4. **Testing**
1. **Access Website**:  
   - URL: `http://192.168.0.10` (HTTPD Server). Ye request ko JBoss servers pe route karega.

2. **Data Update Verification**:  
   - Agar ek user `192.168.0.10` se form submit karta hai, to data EDB me save hoga.
   - Aap kisi bhi JBoss server (`192.168.0.20` ya `192.168.0.30`) se bhi wahi updated data dekhoge.

3. **Logs Check**:
   - HTTPD logs: `/var/log/httpd/access_log` aur `/var/log/httpd/error_log`
   - JBoss logs: `standalone/log/server.log`

---

### Diagram Representation

```
                [ Users ]
                   |
        ------------------------
        |                      |
      HTTPD               www.mywebsite.com
  (192.168.0.10)           (Domain Entry)
                   |
        ------------------------
        |                      |
  [JBoss 1]              [JBoss 2]
  192.168.0.20:8080      192.168.0.30:8080
                   |
         ----------------------
         |                    |
      [EDB Database]
      192.168.0.40:5432
```

---

### Notes
1. **Failover**: HTTPD automatically failover karega agar ek JBoss server down ho jaye.
2. **SSL**: Domain secure karne ke liye HTTPD par SSL setup karo.
3. **Monitoring**: Prometheus aur Grafana ka use kar ke HTTPD, JBoss, aur EDB ko monitor karein.

<hr>

# 3. Scenario

1. **HTTPD Server (Reverse Proxy)**:
   - HTTPD server ek entry point hai, jiska apna alag IP hai. Ye server aapke domain ko handle karta hai, matlab jab koi user aapke domain name (e.g., `www.example.com`) ko access karega, toh request sabse pehle HTTPD server tak pahuchegi.
   - HTTPD server ka kaam yeh hai ki user ki request ko accept karke usse JBoss servers tak route kare. Yeh ek **reverse proxy** ke roop mein kaam karta hai. Matlab, HTTPD ko configure kiya gaya hai ki jab bhi koi request aaye, toh wo request backend servers (JBoss) pe forward ho.
   - Reverse proxy ka fayda yeh hota hai ki user directly JBoss servers se connect nahi hota, balki wo pehle HTTPD server ke through jata hai, jo request ko manage karke JBoss servers tak forward karta hai.

2. **Load Balancer (HTTPD server acting as Load Balancer)**:
   - Agar aapke paas multiple JBoss servers hain (jaise 2 IPs, `192.168.1.10` aur `192.168.1.11`), toh HTTPD server unhe load balance kar sakta hai.
   - Matlab jab ek user request karega, toh HTTPD server decide karega ki us request ko kaunsa JBoss server handle karega. Yeh process **load balancing** kehlaata hai.
   - Example:
     - Agar ek request aayi `http://www.example.com`, toh HTTPD server pehle check karega ki kaunsa JBoss server available hai, phir us request ko us server tak forward karega.
     - Agar JBoss server 1 (`192.168.1.10`) busy ho, toh HTTPD server request ko JBoss server 2 (`192.168.1.11`) pe forward karega.

3. **Example Configuration**:
   - Maan lijiye aapka HTTPD server ka IP `192.168.1.20` hai, aur aapka domain `www.example.com` hai.
   - Aapka JBoss server 1 ka IP `192.168.1.10` hai, aur JBoss server 2 ka IP `192.168.1.11` hai.
   
   HTTPD server ko aise configure kiya jayega:

   ```apache
   <VirtualHost *:80>
       ServerName www.example.com

       # Load balancing configuration
       ProxyPass / http://192.168.1.10/
       ProxyPassReverse / http://192.168.1.10/

       ProxyPass / http://192.168.1.11/
       ProxyPassReverse / http://192.168.1.11/
   </VirtualHost>
   ```

   Is configuration mein:
   - Jab bhi user `www.example.com` ko access karega, HTTPD server pehli request ko JBoss server 1 (`192.168.1.10`) pe forward karega, aur agar wo server busy hai, toh next request ko JBoss server 2 (`192.168.1.11`) pe forward karega.
   - `ProxyPass` aur `ProxyPassReverse` directives ka use hota hai HTTPD ko reverse proxy aur load balancing ke liye configure karne ke liye.

4. **End Result**:
   - Jab user koi action karega, toh uska data JBoss server pe update hoga. JBoss ke dono servers mein data synchronization ka kaam ho sakta hai, jisse dono servers pe latest data available rahe.
   - HTTPD server domain ke through request ko JBoss servers pe route karega, aur response ko user tak forward karega.

Is setup se aapki website high availability aur load balancing ke sath smoothly chal paayegi.
