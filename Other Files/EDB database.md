## Note Study
- [Doc](https://www.enterprisedb.com/docs/)
- [EDB database](https://www.enterprisedb.com/)

- Systemctl start dbs-as-15
- Location - var/lib/edb/as15/data on this location have log file
- Jboss standalone configuration

<hr>

> ## Here are links to the official documentation and resources for the **25 EDB tools** mentioned. These resources provide details about installation, usage, and features of the tools. 

1. **[EDB Backup and Recovery Tool (BART)](https://www.enterprisedb.com/edb-docs/p/edb-backup-and-recovery-tool)**: Centralized backup solution for EDB Postgres databases. [More details](https://knowledge.enterprisedb.com/hc/en-us/articles/15475319178780).

2. **[EDB Failover Manager](https://www.enterprisedb.com/edb-docs/p/edb-failover-manager)**: Ensures high availability with automated failover.

3. **[EDB LDAP Sync](https://www.enterprisedb.com/edb-docs/p/edb-ldap-sync)**: Syncs LDAP user roles with the database.

4. **[EDB LiveCompare](https://www.enterprisedb.com/edb-docs/p/edb-livecompare)**: Simplifies database comparison and migrations.

5. **[EDB Migration Toolkit](https://www.enterprisedb.com/edb-docs/p/edb-migration-toolkit)**: Assists in database migrations from Oracle or other systems.

6. **[EDB Postgres Enterprise Manager (PEM)](https://www.enterprisedb.com/edb-docs/p/edb-postgres-enterprise-manager)**: Monitors and manages Postgres environments.

7. **[EDB Postgres Extended Server](https://www.enterprisedb.com/edb-docs/p/edb-postgres-extended-server)**: Enhanced version of PostgreSQL for enterprises.

8. **[EDB Postgres for Kubernetes Operator](https://www.enterprisedb.com/edb-docs/p/edb-postgres-kubernetes-operator)**: Simplifies Kubernetes-based database deployments.

9. **[EDB Replication Server](https://www.enterprisedb.com/edb-docs/p/edb-replication-server)**: Manages multi-master and single-master replication.

10. **[EDB Postgres Advanced Server](https://www.enterprisedb.com/edb-docs/p/edb-postgres-advanced-server)**: An Oracle-compatible Postgres database.

11. **[Postgres Advanced Server Container Image](https://www.enterprisedb.com/edb-docs/p/edb-containers)**: For container-based deployments.

12. **[EDB PgBouncer](https://www.enterprisedb.com/edb-docs/p/edb-pgbouncer)**: Connection pooling for Postgres.

13. **[EDB Pgpool](https://www.enterprisedb.com/edb-docs/p/edb-pgpool)**: Enhances connection pooling and load balancing.

14. **[EDB Language Pack](https://www.enterprisedb.com/edb-docs/p/edb-language-pack)**: Prepares PL/SQL languages for Postgres.

15. **[EDB Slony](https://www.enterprisedb.com/edb-docs/p/edb-slony)**: An open-source replication tool.

16. **[EDB*Plus](https://www.enterprisedb.com/edb-docs/p/edb-plus)**: Command-line interface for database administration.

17. **[EDB .NET Connector](https://www.enterprisedb.com/edb-docs/p/edb-net-connector)**: Connects Postgres to .NET applications.

18. **[EDB JDBC Connector](https://www.enterprisedb.com/edb-docs/p/edb-jdbc-connector)**: Connects Java applications to Postgres.

19. **[EDB ODBC Connector](https://www.enterprisedb.com/edb-docs/p/edb-odbc-connector)**: Enables connectivity with ODBC-supported applications.

20. **[EDB Postgres Workload Reports (PWR)](https://www.enterprisedb.com/edb-docs/p/edb-workload-reports)**: Provides insights into database performance.

21. **[EDB OCI Connector](https://www.enterprisedb.com/edb-docs/p/edb-oci-connector)**: Integration with Oracle Cloud Infrastructure.

22. **[EDB StackBuilder Plus](https://www.enterprisedb.com/edb-docs/p/edb-stackbuilder)**: Installs add-ons and enhancements for EDB Postgres.

23. **[EDB Postgres Distributed](https://www.enterprisedb.com/edb-docs/p/edb-postgres-distributed)**: Advanced distributed database solution.

24. **[EDB High Availability Routing for Postgres](https://www.enterprisedb.com/edb-docs/p/edb-high-availability-routing)**: Provides intelligent routing for high availability.

If you need additional specific details about any of these tools, feel free to ask!

<hr>

> ## Yahan main aapko har **EDB tool** ke bare mein simple Roman Hindi mein samjhaunga — iska purpose, kab use kiya jata hai, aur kaise install karte hain.

---

### 1. **EDB Backup and Recovery Tool (BART)**  
**Kyu use karte hain?**  
Database ka automated backup aur recovery ke liye. Agar kuch galat ho jaye ya data loss ho, to isse easily data restore kiya ja sakta hai.  
**Kab use hota hai?**  
Jab aapko large Postgres databases ka backup schedule karna ho.  
**Kaise install karein?**  
1. `yum install edb-bart` (CentOS/RedHat).  
2. Configuration file edit karke backup aur recovery setup karein.  

---

### 2. **EDB Failover Manager (EFM)**  
**Kyu use karte hain?**  
High availability ke liye. Agar ek server fail ho jaye, to ye automatically failover kar deta hai.  
**Kab use hota hai?**  
Jab aapka database mission-critical ho aur downtime avoid karna ho.  
**Kaise install karein?**  
1. `yum install edb-efm`  
2. Cluster configuration ke liye XML file configure karein.  

---

### 3. **EDB LDAP Sync**  
**Kyu use karte hain?**  
LDAP users aur roles ko database ke saath sync karne ke liye.  
**Kab use hota hai?**  
Jab organization ke paas centralized user management ho.  
**Kaise install karein?**  
1. Installer EDB website se download karein.  
2. LDAP settings ke hisaab se sync tool configure karein.

---

### 4. **EDB LiveCompare**  
**Kyu use karte hain?**  
Databases compare karne ke liye, jaise schema aur data differences dikhana.  
**Kab use hota hai?**  
Database migration ke waqt.  
**Kaise install karein?**  
Installer EDB website se leke `java` based commands run karein.

---

### 5. **EDB Migration Toolkit (MTK)**  
**Kyu use karte hain?**  
Oracle aur other databases ko Postgres me migrate karne ke liye.  
**Kab use hota hai?**  
Migration projects me jab aapko Oracle se cost bachana ho.  
**Kaise install karein?**  
1. `yum install edb-migration-toolkit`  
2. Command line se `migration-toolkit.properties` configure karein.

---

### 6. **EDB Postgres Enterprise Manager (PEM)**  
**Kyu use karte hain?**  
Database ka monitoring aur management ke liye.  
**Kab use hota hai?**  
Jab multiple Postgres servers ko manage karna ho.  
**Kaise install karein?**  
1. PEM installer download karein.  
2. `./pem_server` aur `pem_agent` setup karein.

---

### 7. **EDB Postgres Extended Server**  
**Kyu use karte hain?**  
Postgres ka enterprise-level version jo security aur performance ke liye optimized hai.  
**Kab use hota hai?**  
Enterprise production environments me.  
**Kaise install karein?**  
EDB yum repository add karke package install karein.

---

### 8. **EDB Postgres for Kubernetes Operator**  
**Kyu use karte hain?**  
Postgres databases ko Kubernetes pe deploy aur manage karne ke liye.  
**Kab use hota hai?**  
Jab aap Kubernetes based infrastructure use karte ho.  
**Kaise install karein?**  
1. Helm chart ya `kubectl apply` se operator deploy karein.  

---

### 9. **EDB Replication Server**  
**Kyu use karte hain?**  
Data replication ke liye, jaise data sync karna multiple servers me.  
**Kab use hota hai?**  
Disaster recovery ya read scaling ke liye.  
**Kaise install karein?**  
Replication server ko `yum install` se setup karein aur replication tasks configure karein.  

---

### 10. **EDB Postgres Advanced Server**  
**Kyu use karte hain?**  
Oracle compatibility aur advanced features ke liye.  
**Kab use hota hai?**  
Jab Oracle se migrate karna ho.  
**Kaise install karein?**  
1. EDB repository add karein.  
2. `yum install edb-as` run karein.

---

### 11. **Postgres Advanced Server Container Image**  
**Kyu use karte hain?**  
Docker me Postgres deploy karne ke liye.  
**Kab use hota hai?**  
Jab lightweight aur portable Postgres environment chahiye.  
**Kaise install karein?**  
1. Docker image pull karein: `docker pull enterprisedb/edb-as`.  

---

### 12. **EDB PgBouncer**  
**Kyu use karte hain?**  
Connection pooling ke liye, jo database ki load handle karta hai.  
**Kab use hota hai?**  
Jab multiple users se connection requests ho.  
**Kaise install karein?**  
`yum install edb-pgbouncer` aur configuration file setup karein.  

---

### 13. **EDB Pgpool**  
**Kyu use karte hain?**  
Load balancing aur query caching ke liye.  
**Kab use hota hai?**  
Jab multiple Postgres servers ke beech load balance karna ho.  
**Kaise install karein?**  
Pgpool installer se configure karein aur start karein.  

---

### 14. **EDB Language Pack**  
**Kyu use karte hain?**  
Postgres ke liye additional language support (jaise PL/Perl, PL/Python) provide karta hai.  
**Kab use hota hai?**  
Jab aapko stored procedures aur triggers me advanced languages use karni ho.  
**Kaise install karein?**  
1. EDB yum repo se `yum install edb-language-pack` run karein.  
2. Required language configure karein.  

---

### 15. **EDB Slony**  
**Kyu use karte hain?**  
Master-slave replication setup karne ke liye.  
**Kab use hota hai?**  
Jab aapko asynchronous data replication chahiye.  
**Kaise install karein?**  
1. Slony installer download karein.  
2. Configuration script run karke nodes setup karein.  

---

### 16. **EDB*Plus**  
**Kyu use karte hain?**  
Oracle ke SQL*Plus jaisa interface Postgres ke liye provide karta hai.  
**Kab use hota hai?**  
Jab Oracle-like commands Postgres me run karni ho.  
**Kaise install karein?**  
EDB Advanced Server ke sath default install hota hai. Bas binary run karein.  

---

### 17. **EDB .NET Connector**  
**Kyu use karte hain?**  
.NET applications ke liye Postgres database connectivity ke liye.  
**Kab use hota hai?**  
Jab aapko .NET applications ko Postgres ke saath integrate karna ho.  
**Kaise install karein?**  
EDB ki site se .NET driver download karein aur Visual Studio me configure karein.  

---

### 18. **EDB JDBC Connector**  
**Kyu use karte hain?**  
Java applications ko Postgres ke saath connect karne ke liye.  
**Kab use hota hai?**  
Jab Java applications Postgres database use karte ho.  
**Kaise install karein?**  
1. JDBC jar file download karein.  
2. Java project me add karein aur connection string configure karein.  

---

### 19. **EDB ODBC Connector**  
**Kyu use karte hain?**  
ODBC-based tools (jaise MS Access, Tableau) ko Postgres se connect karne ke liye.  
**Kab use hota hai?**  
Jab reporting ya data analysis tools use ho rahe ho.  
**Kaise install karein?**  
ODBC driver EDB ki website se download karein aur Windows ODBC Manager me configure karein.  

---

### 20. **EDB Postgres Workload Reports (PWR)**  
**Kyu use karte hain?**  
Database workload aur performance monitor karne ke liye.  
**Kab use hota hai?**  
Jab aapko slow queries aur bottlenecks find karne ho.  
**Kaise install karein?**  
PEM ke sath pre-installed hota hai, PWR scripts ko enable karein.  

---

### 21. **EDB OCL Connector**  
**Kyu use karte hain?**  
Object-oriented applications ke liye Postgres ke saath connectivity provide karta hai.  
**Kab use hota hai?**  
Jab applications me object-relational mapping ho.  
**Kaise install karein?**  
OCL Connector ko download karke application settings me integrate karein.  

---

### 22. **EDB StackBuilder Plus**  
**Kyu use karte hain?**  
Database add-ons aur extensions (jaise PgBouncer, Slony) install karne ke liye.  
**Kab use hota hai?**  
Jab aapko additional tools setup karna ho.  
**Kaise install karein?**  
Postgres installation ke baad StackBuilder GUI se extensions install karein.  

---

### 23. **EDB Postgres Distributed**  
**Kyu use karte hain?**  
Geo-distributed databases setup karne ke liye.  
**Kab use hota hai?**  
Jab aap globally distributed systems use karte ho.  
**Kaise install karein?**  
EDB tools ke sath replication aur distribution configure karein.  

---

### 24. **EDB High Availability Routing (HAR)**  
**Kyu use karte hain?**  
Database servers ke beech automatic routing aur failover ke liye.  
**Kab use hota hai?**  
Jab multiple database nodes ho aur high availability chahiye.  
**Kaise install karein?**  
`yum install edb-har` aur routing rules setup karein.  

---

### Summary: Installation Tips  
1. EDB tools install karne ke liye `yum` ya `apt` repository setup karein.  
2. Tools ki documentation ko follow karke configuration complete karein.  
3. Enterprise level environments ke liye specific tools like PEM, Failover Manager, aur PgBouncer ka use karein.

Agar kisi tool ke baare me aur detailed setup guide chahiye ho, to mujhe bataiye!

