> The error you are encountering indicates that the necessary dependencies for the Zabbix packages (like `libldap.so.2`, `libssl.so.3`, `libcrypto.so.3`, etc.) are not available on your system.

This could happen due to a few reasons:

1. **Missing Repositories**: Your system might be missing the appropriate repositories that provide these dependencies, or they are not enabled by default.
2. **Version Incompatibilities**: The version of libraries required by Zabbix might be incompatible with the libraries available on your system. For instance, OpenSSL 3.0.0 and GLIBC 2.34 are relatively recent and may not be installed.

### Troubleshooting Steps:

1. **Ensure your system is up to date**: Make sure all repositories and packages are up to date to avoid version mismatches.
   ```bash
   sudo dnf update
   ```

2. **Check and enable required repositories**: Ensure that the required repositories for OpenSSL and LDAP libraries are enabled on your system.
   
   - **For OpenSSL and LDAP**:
     You can try installing the missing dependencies manually.
     ```bash
     sudo dnf install openssl-libs openldap
     ```
   
   - **For GLIBC**:
     If `libc.so.6(GLIBC_2.34)` is missing, ensure that your system is running an appropriate version of glibc. If not, you may need to upgrade or find the appropriate package for your distribution version.

3. **Try using `--nobest` option**: The error suggests trying the `--nobest` flag, which can allow dnf to install a version of the package that might have different (but compatible) dependencies.
   ```bash
   sudo dnf install zabbix-server-pgsql zabbix-web-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent --nobest
   ```

4. **Check if libraries are installed**: Verify if the required libraries (`libssl.so.3`, `libcrypto.so.3`, `libldap.so.2`, etc.) are already installed:
   ```bash
   find /usr/lib /usr/lib64 -name libssl.so.3
   find /usr/lib /usr/lib64 -name libldap.so.2
   ```

5. **Install missing dependencies manually**: If the required libraries are still missing, try to install the specific versions:
   - **For OpenSSL**:
     ```bash
     sudo dnf install openssl3
     ```
   - **For OpenLDAP**:
     ```bash
     sudo dnf install openldap
     ```

6. **Check Compatibility**: Make sure that you are using the correct Zabbix repository for your version of RHEL or CentOS. The Zabbix repository should be compatible with your system version (CentOS 8 or RHEL 9 in this case).

Let me know if you need further assistance with any of these steps!
