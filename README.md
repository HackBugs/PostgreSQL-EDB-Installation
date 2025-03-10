## IP Allow server side with particular port 

```
pem - https://192.168.219.142/pem/browser/
id - enterprisedb
Password - admin@123

id - alam
Password - alam

Jboss - http://192.168.219.142:8081/
id - jboss
password -  admin

Zabbix - http://192.168.219.142:8080/zabbix.php?action=dashboard.view&dashboardid=1
id - Admin
Password - zabbix

phpMyAdmin - http://192.168.219.142:8081/
id - root
Passwaod - root_password


export JBOSS_HOME=/opt/jboss/wildfly-34.0.1.Final
./standalone.sh &
/opt/jboss/wildfly-34.0.1.Final/bin
/opt/jboss/wildfly-34.0.1.Final/standalone/deployments
/opt/jboss/wildfly-34.0.1.Final/standalone/log
```
----------------------------------------------------------------

sudo ufw allow from <Your_IP> to any port 8834

telnet <Your_IP> 8834

Check allowlist
sudo ufw status
```

## Apache
1. Configuration file open karein
- (e.g., /etc/apache2/sites-available/your-site.conf).

## Ngnix
2. Configuration file open karein
- (e.g., /etc/nginx/sites-available/default).

## EDB Database 
3. /var/lib/edb/as<version>/data/pg_hba.conf
- nano /var/lib/edb/as<version>/data/postgresql.conf
- /var/log/edb/as<version>/postgresql.log
- /etc/edb/efm-<version>/efm.properties
- systemctl restart edb-as<version>

> # COMMANDS

check all running services on Linux, formatted in a code box:

```bash
# For Systemd (modern Linux distributions)
systemctl list-units --type=service --state=running

# To list all services (running, stopped, or failed)
systemctl list-units --type=service

# To check detailed status of a specific service
systemctl status <service_name>

# For SysVinit (older Linux distributions)
service --status-all | grep running

# To check the status of a specific service
service <service_name> status

# For Upstart (used in some older Ubuntu versions)
initctl list

# To check the status of a specific service
initctl status <service_name>
``` 

Replace `<service_name>` with the name of the service you want to inspect.

<hr>

To allow ports `9990` and `8080` through the firewall in Linux, use the appropriate commands depending on your firewall management tool. Below are examples for **UFW**, **firewalld**, and **iptables**:

### For UFW (Uncomplicated Firewall)
```bash
# Allow port 9990
sudo ufw allow 9990

# Allow port 8080
sudo ufw allow 8080

# Reload the firewall to apply changes (if necessary)
sudo ufw reload

# Check the status to confirm
sudo ufw status
```

---

### For firewalld
```bash
# Allow port 9990
sudo firewall-cmd --permanent --add-port=9990/tcp

# Allow port 8080
sudo firewall-cmd --permanent --add-port=8080/tcp

# Reload the firewall to apply changes
sudo firewall-cmd --reload

# Check the status to confirm
sudo firewall-cmd --list-ports
```

---

### For iptables
```bash
# Allow port 9990
sudo iptables -A INPUT -p tcp --dport 9990 -j ACCEPT

# Allow port 8080
sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT

# Save the changes (varies by distribution)
sudo iptables-save > /etc/iptables/rules.v4

# Check the current rules
sudo iptables -L -n -v
```
### Networking CMD
```sh
Check which program is using this port
sudo lsof -i :9043
sudo netstat -tulnp | grep 9043
sudo ss -tulnp | grep 9043
sudo fuser 9043/tcp

sudo ufw allow 9090/tcp - allows firewall traffic on port 9090:
cmd - netstat and ss and
cmd - sudo ethtool enp0s3
./prometheus --web.listen-address=":9043" &

sudo ufw status
sudo ufw enable
sudo ufw allow 9043/tcp
sudo ufw allow 9043/udp
sudo ufw deny 9043/tcp

## How many packages installed 
dpkg -l | wc -l

## All packages list count
apt list | wc -l

netstat -atpune | grep pgbouncer
find / -type f -name "filename

```

<hr>

> ## All file hyperlinks

[Commands.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/Commands.md)  
[DBeaver Tool.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/DBeaver%20Tool.md)  
[DevOps Tools.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/DevOps%20Tools.md)  
[EDB - Oracle aur PostgreSQL ke queries.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/EDB%20-%20Oracle%20aur%20PostgreSQL%20ke%20queries.md)  
[EDB Subscription Software.png](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/EDB%20Subscription%20Software.png)  
[EDB database.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/EDB%20database.md)  
[JBoss (WildFly) Installation Script.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/JBoss%20(WildFly)%20Installation%20Script.md)  
[PostgreSQL database.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/PostgreSQL%20database.md)  
[Red_Hat_JBoss_Enterprise_Application_Platform-7.0-Installation_Guide-en-US.pdf](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/Red_Hat_JBoss_Enterprise_Application_Platform-7.0-Installation_Guide-en-US.pdf)  
[TIBCO Software.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/TIBCO%20Software.md)  
[Tools-Ports-and-IP.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/Tools-Ports-and-IP.md)  
[UFW-Allow.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/UFW-Allow.md)  
[apt-dnf-yum.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/apt-dnf-yum.md)  
[pgAdmin Install.md](https://github.com/HackBugs/PostgreSQL-EDB-Installation/blob/main/Other%20Files/pgAdmin%20Install.md)  


---

> ## Option 2: Postgres aur EDB Ek Saath
- Dono install kar sakte ho agar alag ports use karo (e.g., Postgres port 5432 aur EDB port 5444).
- Iska fayda hoga ki aap dono ka comparison aur testing kar sakte ho.

<hr>

## TIBCO Software

## EDB database
> EnterpriseDB (EDB Advanced Server) Installation
[EDB database](https://www.enterprisedb.com/)

## PostgreSQL database
```
1. pgAdmin - PostgreSQL Tools
```

## Tools
```
1. DBeaver Tool 
2. WinSCP
3. MobaXterm 
4. FileZilla
5. Putty
6. IP Messenger
```

## 1. DBeaver Tool 
```
DBeaver is a SQL client software application and a database administration tool. For relational databases it
uses the JDBC application programming interface to interact with databases via a JDBC driver. For other
databases it uses proprietary database drivers.
```

## TDMS RailServer:
```
TDMS (Track Data Management System) RailServer is a specialized system used in the railway industry to manage, analyze, 
and store data related to railway track maintenance and operations. It is often used by rail operators and engineers 
for track condition monitoring, defect analysis, and planning maintenance activities to ensure the safety and efficiency 
of railway operations.
```

<hr>

## **EDB Subscription Software** 

---
 
- EDB Backup and Recovery Tool  
- EDB Failover Manager  
- EDB LDAP Sync  
- EDB LiveCompare  
- EDB Migration Toolkit  
- EDB Postgres Enterprise Manager  
- EDB Postgres Extended Server  
- EDB Postgres for Kubernetes Operator  
- EDB Replication Server  
- EDB Postgres Advanced Server  
- Postgres Advanced Server Container Image  
- EDB PgBouncer  
- EDB Pgpool  
- EDB Language Pack  
- EDB Slony  
- EDB*Plus  
- EDB .net Connector  
- EDB JDBC Connector  
- EDB ODBC Connector  
- EDB Postgres Workload Reports (PWR)  
- EDB OCL Connector  
- EDB StackBuilder Plus  
- EDB Postgres Distributed  
- EDB High Availability Routing for Postgres  

<hr>

> **JBoss (WildFly)** aur **TIBCO** dono hi enterprise software platforms hain, lekin inka kaam aur use case alag hai. Dono ke beech ka **antar** inke purpose, architecture, aur applications ke base par samjha ja sakta hai. Chaliye, dono ko detail mein compare karte hain:  

---

### **1. Purpose (Kaam aur Use Case)**  
| Feature              | **JBoss (WildFly)**                                  | **TIBCO**                                                                 |
|----------------------|----------------------------------------------------|---------------------------------------------------------------------------|
| **Primary Use**      | Java EE (Enterprise Edition) application server.   | Integration, middleware, and data exchange between applications.          |
| **Use Case**         | Web applications, APIs, and enterprise Java apps.  | Enterprise application integration (EAI), messaging, and data workflows.  |
| **Target Audience**  | Java developers for running Java EE applications.  | Enterprises with diverse tech stacks needing system integration.          |

---

### **2. Technology Stack**  
| Feature              | **JBoss (WildFly)**                                  | **TIBCO**                                                                 |
|----------------------|----------------------------------------------------|---------------------------------------------------------------------------|
| **Language Support** | Primarily Java (Java EE APIs).                     | Multi-language (Java, .NET, Python, etc.) for integration.                |
| **Core Technology**  | Java-based application server for web and APIs.    | Middleware focusing on **ESB (Enterprise Service Bus)** and messaging.    |

---

### **3. Key Features**  
| **JBoss (WildFly)**                                                                 | **TIBCO**                                                                 |
|------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| Application server for deploying Java EE apps.                                     | Enterprise Service Bus (ESB) for connecting applications.                 |
| Provides services like Servlets, JSP, JPA, EJB, and JMS.                           | Focused on **integration** using TIBCO BusinessWorks and ActiveMatrix.    |
| Open-source and community-driven.                                                  | Proprietary and commercial product.                                       |
| Highly scalable for Java-based microservices and monolithic applications.          | Built for **messaging**, **data transformation**, and **workflow orchestration**. |
| Lightweight, modular, and cloud-friendly.                                          | Includes tools like TIBCO EMS (Messaging) and Spotfire (Analytics).       |

---

### **4. Deployment and Usage**  
| Feature              | **JBoss (WildFly)**                                  | **TIBCO**                                                                 |
|----------------------|----------------------------------------------------|---------------------------------------------------------------------------|
| **Deployment Model** | Runs on servers (standalone or clustered).         | Centralized deployment for middleware and messaging.                      |
| **Cloud Readiness**  | Fully cloud-compatible (supports containers, etc.).| Cloud integration but traditionally server-heavy.                         |
| **Ease of Use**      | Developer-friendly, requires knowledge of Java EE. | Enterprise-centric, requires understanding of workflows and integration.  |

---

### **5. Integration**  
| Feature              | **JBoss (WildFly)**                                  | **TIBCO**                                                                 |
|----------------------|----------------------------------------------------|---------------------------------------------------------------------------|
| **Integration Focus**| Primarily focused on Java-based services and APIs. | Highly focused on integrating diverse systems like SAP, Salesforce, etc. |
| **Messaging**        | Supports JMS for Java-based messaging.             | Provides robust messaging tools (TIBCO EMS, BusinessWorks).              |

---

### **6. Pricing and Licensing**  
| Feature              | **JBoss (WildFly)**                                  | **TIBCO**                                                                 |
|----------------------|----------------------------------------------------|---------------------------------------------------------------------------|
| **Cost**             | Free and open-source.                              | Proprietary, requires licensing (costly for small businesses).            |
| **Community Support**| Strong community-driven updates and plugins.       | Vendor-supported with dedicated customer service.                         |

---

### **7. Real-Life Examples**  
| Feature              | **JBoss (WildFly)**                                  | **TIBCO**                                                                 |
|----------------------|----------------------------------------------------|---------------------------------------------------------------------------|
| **Example Usage**    | Deploying an e-commerce Java application.          | Integrating Salesforce CRM with SAP ERP for real-time data exchange.      |
| **Industries**       | Technology, web applications, startups.            | Banks, logistics, healthcare, large-scale enterprise systems.             |

---

### **Key Differences in Layman Terms**  

- **JBoss (WildFly):** Agar aapko **Java-based web applications** ya APIs deploy karni hain, toh JBoss ek **application server** hai jo inko run karne mein help karta hai.  
- **TIBCO:** Agar aapko **different systems ko connect** karna hai (e.g., SAP aur Salesforce ko ek saath kaam karna), ya **data integration aur messaging** setup karna hai, toh TIBCO ka use hota hai.  

---

Agar aapko **Java applications develop aur deploy** karna hai, JBoss/WildFly sahi hai.  
Lekin agar aapko **applications integrate aur automate** karna hai, TIBCO ka use karein.  

<hr>

> **JBoss (WildFly)** ka use karne ka best tarika ek simple project ke saath samajhna hai.  
Yaha hum ek **Java-based web application** banayenge, jo "Hello World" message display karega. Is project ko **JBoss server** par deploy karenge.  

---

### **Example Project: Deploying "Hello World" Application on JBoss**  
---

### **Step 1: Prerequisites**  
1. **JBoss (WildFly)** installed. [Refer installation guide above]  
2. **Java Development Kit (JDK)** installed. Use OpenJDK or Oracle JDK.  
3. **Apache Maven** for building the project. Install with:  
   ```bash
   sudo apt install maven -y
   ```
4. Any **IDE** like IntelliJ IDEA, Eclipse, or Visual Studio Code.

---

### **Step 2: Create a Simple Java Web Application**  
**1. Initialize Maven Project**  
Run the following command to create a Maven project:  
```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=hello-world-app -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```

This will create a directory structure for your project in `hello-world-app`.

**2. Add Dependencies in `pom.xml`**  
Add the following dependencies in your `pom.xml`:  
```xml
<dependencies>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

---

### **Step 3: Create "Hello World" Servlet**  
1. Navigate to `src/main/java/com/example/`.  
2. Create a Java class named `HelloWorldServlet.java`.  

**Code for Servlet:**  
```java
package com.example;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class HelloWorldServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        response.getWriter().println("<h1>Hello World from JBoss!</h1>");
    }
}
```

---

### **Step 4: Configure `web.xml` (Deployment Descriptor)**  
1. Navigate to `src/main/webapp/WEB-INF/`.  
2. Open or create `web.xml` and add the servlet configuration:  

```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee" version="3.0">
    <servlet>
        <servlet-name>HelloWorldServlet</servlet-name>
        <servlet-class>com.example.HelloWorldServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloWorldServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

---

### **Step 5: Build the Application**  
Navigate to your project directory and build the application:  
```bash
mvn package
```
After this, you will find a `hello-world-app.war` file in the `target/` directory.

---

### **Step 6: Deploy Application to JBoss**  
1. Copy the `hello-world-app.war` file to JBoss's `deployments` directory:  
   ```bash
   sudo cp target/hello-world-app.war /opt/wildfly/standalone/deployments/
   ```
2. Start or Restart JBoss server:  
   ```bash
   sudo systemctl restart wildfly
   ```

---

### **Step 7: Access the Application**  
1. Open your browser and go to:  
   ```
   http://<server-ip>:8080/hello-world-app/hello
   ```
2. You should see:  
   ```
   Hello World from JBoss!
   ```

---

### **Summary of Steps**  
1. Install JBoss and prerequisites (JDK, Maven).  
2. Create a Maven web project.  
3. Write a servlet (`HelloWorldServlet`).  
4. Configure `web.xml` to map the servlet.  
5. Build the WAR file using Maven.  
6. Deploy the WAR file to JBoss's `deployments` directory.  
7. Access the app via browser.  

---

**Yeh project aapko real-world deployment samajhne ka ek simple aur practical idea dega.**  

<hr>

> Haan, **JBoss (WildFly)** ek **application server** hai jo **Java EE (Enterprise Edition)** applications ko run karne ke liye use hota hai. Is category mein aur bhi kaafi saari **similar technologies** hain jo alag-alag use cases ke liye kaam aati hain.  

---

### **JBoss ke Similar Technologies (Application Servers)**  

#### 1. **Apache Tomcat**  
   - **Description:**  
     Lightweight server, mainly used for running **Java Servlets** and **JSPs**.  
   - **Use Case:**  
     Simple web applications and APIs. Java EE ka complete support nahi deta.  
   - **Comparison with JBoss:**  
     - JBoss zyada advanced hai (EJB, JPA, JMS ka support).  
     - Tomcat sirf web applications ke liye sufficient hai.  

#### 2. **GlassFish**  
   - **Description:**  
     Official reference implementation for **Java EE**. Sun Microsystems ne develop kiya (ab Oracle ke paas hai).  
   - **Use Case:**  
     Enterprise-level Java applications.  
   - **Comparison with JBoss:**  
     - GlassFish full Java EE compliance deta hai, similar to JBoss.  
     - GlassFish ka community support kam hai, lekin Oracle ka backing hai.  

#### 3. **WebLogic**  
   - **Description:**  
     Oracle ka proprietary application server, enterprise-level deployments ke liye popular.  
   - **Use Case:**  
     High-performance systems like banking or telecom.  
   - **Comparison with JBoss:**  
     - WebLogic commercial hai (costly), JBoss free hai.  
     - WebLogic ka support aur scalability better hai for critical systems.  

#### 4. **WebSphere**  
   - **Description:**  
     IBM ka enterprise-level application server, enterprise Java ke liye famous.  
   - **Use Case:**  
     Large enterprises like healthcare, retail, aur government systems.  
   - **Comparison with JBoss:**  
     - WebSphere ka focus enterprise-grade security aur scalability par hai.  
     - JBoss open-source hai aur lightweight hai in comparison.  

#### 5. **Jetty**  
   - **Description:**  
     Lightweight application server similar to Tomcat.  
   - **Use Case:**  
     Embedded systems ya cloud-based microservices.  
   - **Comparison with JBoss:**  
     - Jetty ka use lightweight services ke liye hota hai.  
     - JBoss heavy-duty enterprise applications ke liye better hai.  

#### 6. **Payara Server**  
   - **Description:**  
     GlassFish ka open-source fork, modern cloud deployments ke liye optimized.  
   - **Use Case:**  
     Microservices aur cloud-native Java applications.  
   - **Comparison with JBoss:**  
     - Payara ka focus cloud-readiness aur modularity par hai.  
     - JBoss traditional aur cloud dono ke liye suitable hai.  

#### 7. **Spring Boot with Embedded Servers**  
   - **Description:**  
     Spring Boot ke saath embedded Tomcat, Jetty, ya Undertow ka use hota hai.  
   - **Use Case:**  
     Microservices architecture aur standalone web applications.  
   - **Comparison with JBoss:**  
     - JBoss ek full-fledged application server hai.  
     - Spring Boot standalone aur lightweight solutions ke liye suitable hai.  

#### 8. **Undertow**  
   - **Description:**  
     Lightweight, high-performance web server (JBoss ke andar bhi use hota hai).  
   - **Use Case:**  
     Microservices aur REST APIs.  
   - **Comparison with JBoss:**  
     - Undertow lightweight aur fast hai, lekin JBoss full Java EE support karta hai.  

---

### **Kaunsa Server Kab Use Karein?**  
| **Server**      | **Use Case**                                                                 |
|------------------|-----------------------------------------------------------------------------|
| **JBoss**        | Full Java EE applications, APIs, aur microservices ke liye.                |
| **Tomcat**       | Lightweight Java web applications ke liye.                                |
| **GlassFish**    | Full Java EE compliance aur enterprise apps ke liye.                      |
| **WebLogic**     | High-performance, mission-critical enterprise systems ke liye.            |
| **WebSphere**    | Secure aur large-scale deployments ke liye.                               |
| **Jetty**        | Lightweight microservices aur embedded systems ke liye.                   |
| **Spring Boot**  | Standalone applications aur microservices architecture ke liye.           |

<hr>

> **Nexus**, **Nginx**, aur **JBoss (WildFly)** tino hi servers hain, lekin unka kaam aur use case alag hai. Ye tino alag-alag problems ko solve karte hain. Chaliye inka comparison simple language aur real-life examples ke saath samajhte hain:

---

### **1. Nexus**  
- **Type:** Repository Manager  
- **Purpose:**  
  - Software dependencies ko manage karta hai.  
  - Artifacts (e.g., JAR files, Docker images, etc.) ko store aur distribute karta hai.  
  - Build tools (Maven, Gradle, npm) ke saath integrate hota hai.  
- **Use Case Example:**  
  Aap ek Java project build kar rahe hain aur aapko dependencies (e.g., Hibernate, Spring) ki zarurat hai. Nexus in dependencies ko manage karega aur build ke baad output artifacts (JAR/WAR) ko store karega.

---

### **2. Nginx**  
- **Type:** Web Server, Reverse Proxy, Load Balancer  
- **Purpose:**  
  - Static content (HTML, CSS, JS, images) serve karna.  
  - Backend servers ke beech load balancing karna.  
  - HTTPS termination aur caching provide karna.  
- **Use Case Example:**  
  Aapki e-commerce website ke static files (e.g., product images) ko Nginx serve karega aur dynamic requests ko backend server (e.g., JBoss) forward karega.

---

### **3. JBoss (WildFly)**  
- **Type:** Application Server  
- **Purpose:**  
  - Java EE-based dynamic applications ko deploy aur run karta hai.  
  - Backend APIs aur database se interact karta hai.  
  - Complex business logic aur enterprise-level applications ke liye use hota hai.  
- **Use Case Example:**  
  Aap ek banking application develop kar rahe hain jo user data aur transactions handle karta hai. JBoss backend par ye sab manage karega.  

---

### **Key Differences:**

| **Feature**               | **Nexus**                                  | **Nginx**                              | **JBoss (WildFly)**                          |
|----------------------------|--------------------------------------------|----------------------------------------|---------------------------------------------|
| **Type**                  | Repository Manager                         | Web Server, Reverse Proxy, Load Balancer | Application Server                          |
| **Primary Function**       | Software artifacts store aur distribute karna. | Static content serve karna aur load balancing. | Java EE applications run karna aur backend logic manage karna. |
| **Focus Area**             | Dependency management aur CI/CD pipelines. | Static content delivery aur request routing. | Backend business logic aur APIs.            |
| **Supported Technologies** | JAR, Docker images, npm packages, etc.     | HTML, CSS, JS, APIs (HTTP/HTTPS).       | Java-based technologies (Servlets, EJBs).   |
| **Role in Architecture**   | Artifact repository                       | Frontend content server                | Backend application server                  |
| **Complexity**             | Simple artifact storage                   | Lightweight and fast                   | Heavyweight enterprise application server   |

---

### **Real-Life Use Cases Together:**

#### **Scenario:**  
Suppose aap ek **DevOps pipeline** setup kar rahe hain:  
1. **Nexus:**  
   Aapka Nexus build tools (e.g., Maven) se dependencies fetch karega aur build output artifacts ko store karega (JAR, WAR).  
2. **Nginx:**  
   Static content serve karega aur load balancing karega backend servers ke beech.  
3. **JBoss:**  
   Backend logic aur dynamic APIs ko handle karega, jo database se connect hongi.

---

### **Kaunsa Kab Use Karein?**

| **Requirement**                 | **Use Technology**              |
|---------------------------------|---------------------------------|
| Artifacts (JAR, WAR, Docker images) ko store aur manage karna. | Nexus                          |
| Static files serve karna aur request routing karna. | Nginx                          |
| Java EE-based application deploy karna. | JBoss                        |

---

<hr>
