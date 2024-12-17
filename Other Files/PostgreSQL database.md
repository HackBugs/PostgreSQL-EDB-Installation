
- [Queries PostgresSQL And about EDB](https://github.com/HackBugs/PostgreSQL-EDB-firewall-cmd.md/blob/main/EDB%20-%20Oracle%20aur%20PostgreSQL%20ke%20queries.md)

```
ps -ef |greo psql

find / -name "*postgresql*" -o -name "*pgsql*" 2>/dev/null
sudo systemctl list-units | grep postgres

sudo systemctl stop postgresql-17
sudo systemctl start postgresql-17
sudo systemctl restart postgresql-17
sudo systemctl status postgresql-17

sudo systemctl disable postgresql-17
sudo systemctl enable postgresql-17

- Uninstall PostgreSQL
sudo systemctl stop postgresql-17
sudo yum remove -y postgresql17*

sudo rm -rf /var/lib/pgsql/17/
sudo rm -rf /etc/postgresql/
ps -ef | grep postgresql17
```

> ## CMD Check where have BIN file of PostgrecsSQL - `which psql` also can use `where psql`

## After installation use these cmd to desc database and alter view table details

1. Use this link and select the version and all and after use this 1.sh paste in this file all code for installation script run in one click
2. sudo su - postgres psql
3. sudo -u postgres psql

---
4. create databse "your databse name";
5. ALTER USER postgres WITH PASSWORD 'admin@123';
6. create USER "yourname" WITH PASSWORD 'admin@123';
---

7. \l - check database
8. \du - Check user

## connect to database cmd
- \du - check database
- \c - database name which you want to connect
- \dt - check list of tables

## create table
- create table test123 (sno int, sname varchar(10));
- \dt - check tables
- \d+ - check table details
- \q - exit to database 

---

- [Download](https://www.postgresql.org/)
- [Download-2](https://www.postgresql.org/download/linux/redhat/)

```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo dnf -qy module disable postgresql
sudo dnf install -y postgresql17-server
sudo /usr/pgsql-17/bin/postgresql-17-setup initdb
sudo systemctl enable postgresql-17
sudo systemctl start postgresql-17
```

```
# Install the repository RPM:
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm

# Disable the built-in PostgreSQL module:
sudo dnf -qy module disable postgresql

# Install PostgreSQL:
sudo dnf install -y postgresql17-server

# Optionally initialize the database and enable automatic start:
sudo /usr/pgsql-17/bin/postgresql-17-setup initdb
sudo systemctl enable postgresql-17
sudo systemctl start postgresql-17
```
<hr>


