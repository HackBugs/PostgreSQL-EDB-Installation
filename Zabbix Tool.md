> # [Zabbix Download](https://www.zabbix.com/download?zabbix=6.0&os_distribution=red_hat_enterprise_linux&os_version=9&components=server_frontend_agent&db=pgsql&ws=apache)

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

---
