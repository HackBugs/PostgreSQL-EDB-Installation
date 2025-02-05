> # [Zabbix Download](https://www.zabbix.com/download?zabbix=6.0&os_distribution=red_hat_enterprise_linux&os_version=9&components=server_frontend_agent&db=pgsql&ws=apache)

```
yum search zabbix
```

### Troubleshooting Steps:

#### 1. **Ensure your system is up to date**:
Make sure all repositories and packages are up to date to avoid version mismatches:
```bash
sudo dnf update
```

#### 2. **Check and enable required repositories**:
Ensure that the required repositories for OpenSSL and LDAP libraries are enabled on your system.

- **For OpenSSL and LDAP**:
  Install the missing dependencies manually:
  ```bash
  sudo dnf install openssl-libs openldap
  ```

- **For GLIBC**:
  If `libc.so.6(GLIBC_2.34)` is missing, ensure your system is running the appropriate version of glibc. If not, you may need to upgrade or find the appropriate package for your distribution version.

#### 3. **Try using the `--nobest` option**:
The error suggests trying the `--nobest` flag, which can allow `dnf` to install a version of the package with different (but compatible) dependencies:
```bash
sudo dnf install zabbix-server-pgsql zabbix-web-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent --nobest
```

#### 4. **Check if libraries are installed**:
Verify if the required libraries (`libssl.so.3`, `libcrypto.so.3`, `libldap.so.2`, etc.) are installed:
```bash
find /usr/lib /usr/lib64 -name libssl.so.3
find /usr/lib /usr/lib64 -name libldap.so.2
```

#### 5. **Install missing dependencies manually**:
If libraries are missing, install the specific versions:
- **For OpenSSL**:
  ```bash
  sudo dnf install openssl3
  ```

- **For OpenLDAP**:
  ```bash
  sudo dnf install openldap
  ```

---

### Additional Steps to Resolve Issues:

#### 6. **Check the enabled repositories**:
Ensure you have the correct repositories enabled. You might need to enable additional repositories like EPEL (Extra Packages for Enterprise Linux):
```bash
dnf install epel-release
dnf update
```
After that, try installing the Zabbix packages again.

#### 7. **Check the Zabbix repository**:
Ensure you're using the correct Zabbix repository. If it's a third-party or external repo, check for compatibility with your RHEL version:
```bash
dnf repolist
```

#### 8. **Check the compatibility of RHEL version**:
Ensure your RHEL version matches the required dependencies for Zabbix. If using an older version of RHEL, you might need to update or upgrade to a version that supports the required dependencies (GLIBC 2.34, OpenSSL 3.0.0, etc.).

#### 9. **Try skipping the broken packages**:
As the error suggests, you can also try installing the packages while skipping the problematic ones:
```bash
dnf install --skip-broken zabbix-server-pgsql zabbix-web-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent
```

<hr>


> # In PostgreSQL (EDB), the syntax you provided for granting privileges and creating a user is not quite accurate. PostgreSQL has a different approach for both granting privileges and user creation.

Here’s the corrected query for your request:

1. **Grant all privileges on all tables in the schema `zabbix` to the user `zabbixuser`:**

```sql
GRANT ALL PRIVILEGES ON SCHEMA zabbix TO zabbixuser;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA zabbix TO zabbixuser;
```

2. **Create the user `zabbixuser` (if not already created), and set their password:**

```sql
CREATE USER zabbixuser WITH PASSWORD '@#123';
```

3. **Allow `zabbixuser` to connect from `localhost`:**

```sql
ALTER USER zabbixuser SET default_transaction_isolation = 'read committed';
```

In PostgreSQL, you don’t specify `@localhost` when creating a user for specific hosts (like MySQL). Instead, access is controlled through pg_hba.conf file configuration and roles. If you want to limit access to localhost, make sure your `pg_hba.conf` file is set up to allow connections from `localhost` for `zabbixuser`.

You can also specify connection privileges using:

```sql
GRANT CONNECT ON DATABASE your_database TO zabbixuser;
```

<hr>

The error `ERROR: schema "zabbix" does not exist` means that PostgreSQL could not find the schema `zabbix` in the current database. To resolve this, follow these steps:

1. **Check if the schema exists:**

Run this query to list all the schemas in the current database:

```sql
SELECT schema_name
FROM information_schema.schemata;
```

If `zabbix` is not listed, it means the schema doesn't exist.

2. **Create the schema (if needed):**

If the `zabbix` schema does not exist, you can create it by running:

```sql
CREATE SCHEMA zabbix;
```

3. **Grant privileges after ensuring the schema exists:**

Once the `zabbix` schema exists, you can grant privileges to the user:

```sql
GRANT ALL PRIVILEGES ON SCHEMA zabbix TO zabbixuser;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA zabbix TO zabbixuser;
```

This ensures that the schema and privileges are set up correctly.

4. **Double-check the user creation (if needed):**

If you haven’t created the `zabbixuser` yet, use this query to create it:

```sql
CREATE USER zabbixuser WITH PASSWORD '@#123';
```

<hr>

To modify the command to work with an EDB (EnterpriseDB) PostgreSQL database, you can use the following approach:

1. Ensure that the EDB PostgreSQL client tools are installed and that you can connect to the database using `psql`.
2. Update the command to ensure it's pointing to the correct database (`zabbix` in your case) and user.

The adjusted command would look like this:

```bash
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql -h <host> -U <username> -d zabbix
```

### Breakdown of the command:
- `zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz`: Unzips and outputs the content of the `server.sql.gz` script.
- `sudo -u zabbix`: Runs the command as the `zabbix` user (assuming this user has appropriate permissions).
- `psql -h <host> -U <username> -d zabbix`: Connects to the PostgreSQL database `zabbix` (replace `<host>` with the appropriate hostname and `<username>` with the PostgreSQL user that has permissions to run the SQL script).

If your EDB PostgreSQL database uses a specific user or password, you may also need to set the `PGPASSWORD` environment variable for authentication, like this:

```bash
export PGPASSWORD=<your_password>
```

And ensure you're using the correct username and database.

Let me know if you need further clarification!
