
# MySQL Server Hardening 

## Overview

This guide provides step-by-step instructions for each method to enhance the security of your MySQL 
installation.

## Hardening Techniques

### 1. Update MySQL Regularly
- **Step 1:** Check for the latest MySQL version available from the [official MySQL 
website](https://dev.mysql.com/downloads/).
- **Step 2:** Follow the instructions for your operating system to upgrade MySQL.

### 2. Use Strong Passwords
- **Step 1:** Log in to MySQL: `mysql -u root -p`.
- **Step 2:** Change the root password: `ALTER USER 'root'@'localhost' IDENTIFIED BY 
'NewStrongPassword!';`.
- **Step 3:** Update passwords for all user accounts with: `ALTER USER 'username'@'host' IDENTIFIED BY 
'NewStrongPassword!';`.

### 3. Remove Unused Accounts
- **Step 1:** List all MySQL users: `SELECT user, host FROM mysql.user;`.
- **Step 2:** Remove unused accounts: `DROP USER 'username'@'host';`.

### 4. Restrict Remote Access
- **Step 1:** Edit MySQL configuration file (`my.cnf` or `my.ini`).
- **Step 2:** Add `bind-address = 127.0.0.1` to limit access to localhost.
- **Step 3:** Restart MySQL server: `sudo systemctl restart mysql`.

### 5. Disable Remote Root Access
- **Step 1:** Log in to MySQL: `mysql -u root -p`.
- **Step 2:** Update root user to disallow remote login: `UPDATE mysql.user SET Host='localhost' WHERE 
User='root' AND Host='%';`.
- **Step 3:** Flush privileges: `FLUSH PRIVILEGES;`.

### 6. Enable MySQL Firewall
- **Step 1:** Ensure MySQL Enterprise Edition is installed.
- **Step 2:** Configure the MySQL Enterprise Firewall via the MySQL Workbench or command line.

### 7. Use SSL/TLS for Connections
- **Step 1:** Generate SSL certificates (refer to [MySQL SSL/TLS 
documentation](https://dev.mysql.com/doc/refman/8.0/en/creating-ssl-certs.html)).
- **Step 2:** Configure MySQL to use SSL in `my.cnf`:
 ```ini
  [mysqld]
  ssl-ca = /path/to/ca-cert.pem
  ssl-cert = /path/to/server-cert.pem
  ssl-key = /path/to/server-key.pem
```

- **Step 3:** Restart MySQL server: `sudo systemctl restart mysql`.

### 8. Configure the MySQL User Privileges

-   **Step 1:** Log in to MySQL: `mysql -u root -p`.
-   **Step 2:** Use `GRANT` statements to provide specific privileges:
    
    
    ```
    GRANT SELECT, INSERT, UPDATE ON database.* TO 'username'@'host';
    ``` 
    
-   **Step 3:** Flush privileges: `FLUSH PRIVILEGES;`.

### 9. Use the `mysql_secure_installation` Script

-   **Step 1:** Run the script: `sudo mysql_secure_installation`.
-   **Step 2:** Follow the prompts to configure security options.

### 10. Monitor Logs

-   **Step 1:** Enable general and slow query logging in `my.cnf`:
    
    
    ```
    general_log = 1
    slow_query_log = 1
    ``` 
    
-   **Step 2:** Check log files located at `/var/log/mysql/` or configured location.

### 11. Limit Resource Usage

-   **Step 1:** Edit `my.cnf` to set resource limits:
    
    
    ```
    max_connections = 100
    max_allowed_packet = 16M
    ``` 
    
-   **Step 2:** Restart MySQL server: `sudo systemctl restart mysql`.

### 12. Encrypt Data at Rest

-   **Step 1:** Configure data-at-rest encryption in `my.cnf`:
    
    
    ```
    innodb_file_per_table = 1
    innodb_encrypt_tables = 1
    innodb_encrypt_log = 1
    ``` 
    
-   **Step 2:** Restart MySQL server: `sudo systemctl restart mysql`.

### 13. Secure MySQL Configuration Files

-   **Step 1:** Set proper file permissions:
    
    
    ```
    sudo chmod 640 /etc/mysql/my.cnf
    sudo chown mysql:mysql /etc/mysql/my.cnf
    ``` 
    

### 14. Use Application-Level Encryption

-   **Step 1:** Implement encryption in your application code.
-   **Step 2:** Use libraries or services for encryption (e.g., AWS KMS, Azure Key Vault).

### 15. Backup Regularly

-   **Step 1:** Configure backups using `mysqldump` or a backup tool.
-   **Step 2:** Schedule regular backups using `cron` or task scheduler.

### 16. Enable Binary Logging

-   **Step 1:** Edit `my.cnf` to enable binary logging:
    
    
    ```
    log_bin = /var/log/mysql/mysql-bin.log
    ``` 
    
-   **Step 2:** Restart MySQL server: `sudo systemctl restart mysql`.

### 17. Use `innodb_strict_mode`

-   **Step 1:** Edit `my.cnf` to enable strict mode:
    
    
    ```
    innodb_strict_mode = 1
    ``` 
    
-   **Step 2:** Restart MySQL server: `sudo systemctl restart mysql`.

### 18. Disable `LOAD DATA INFILE`

-   **Step 1:** Edit `my.cnf` to disable the feature:
    
    
    ```
    local_infile = 0
    ``` 
    
-   **Step 2:** Restart MySQL server: `sudo systemctl restart mysql`.

### 19. Secure the MySQL Data Directory

-   **Step 1:** Set directory permissions:
    
    
    ```
    sudo chmod 700 /var/lib/mysql
    sudo chown -R mysql:mysql /var/lib/mysql
    ``` 
    

### 20. Implement IP Whitelisting

-   **Step 1:** Configure firewall rules to allow access from specific IPs only.
-   **Step 2:** Use tools like `iptables` or cloud security groups.

### 21. Audit User Activities

-   **Step 1:** Install and configure MySQL Enterprise Audit Plugin.
-   **Step 2:** Review audit logs for suspicious activities.

### 22. Enable General and Slow Query Logging

-   **Step 1:** Edit `my.cnf`
    
    ```
    general_log = 1
    slow_query_log = 1
    slow_query_log_file = /var/log/mysql/mysql-slow.log
    long_query_time = 2
    ``` 
    
-   **Step 2:** Restart MySQL server: `sudo systemctl restart mysql`.

### 23. Use MySQL Enterprise Edition

-   **Step 1:** Purchase and install MySQL Enterprise Edition.
-   **Step 2:** Configure additional security features as needed.

### 24. Limit `max_connections`

-   **Step 1:** Edit `my.cnf` to set the maximum connections:
    
    
    ```
    max_connections = 100
    ``` 
    
-   **Step 2:** Restart MySQL server: `sudo systemctl restart mysql`.

### 25. Disable `information_schema` Access

-   **Step 1:** Restrict access via MySQL user privileges:
    
    
    ```
    REVOKE ALL PRIVILEGES ON information_schema.* FROM 'username'@'host';
    ``` 
    

### 26. Restrict File Privileges

-   **Step 1:** Review and adjust file privileges in MySQL:
   
    
    ```
    REVOKE FILE ON *.* FROM 'username'@'host';
    ``` 
    

### 27. Use `mysqladmin` to Secure the Server

-   **Step 1:** Use `mysqladmin` to perform security tasks:
    
    
    ```
    mysqladmin -u root password 'NewPassword'
    ``` 
    

### 28. Change the Default Port

-   **Step 1:** Edit `my.cnf` to change the port:
   
    
    ```
    port = 3307
    ``` 
    
-   **Step 2:** Restart MySQL server: `sudo systemctl restart mysql`.

### 29. Implement Network Security Groups (NSGs)

-   **Step 1:** Configure NSGs in your cloud provider’s console to restrict traffic.

### 30. Deploy MySQL in a Private Network

-   **Step 1:** Place MySQL server in a private subnet within your cloud infrastructure.

### 31. Enable `validate_password` Plugin

-   **Step 1:** Install and configure the `validate_password` plugin:
    
    
    ```
    INSTALL PLUGIN validate_password SONAME 'validate_password.so';
    ``` 
    
-   **Step 2:** Set password policy:
    
    
    ```
    SET GLOBAL validate_password_policy = 'MEDIUM';
    ``` 
    

### 32. Avoid Using the `root` User for Application Access

-   **Step 1:** Create specific users for application access with limited privileges:
    
    
    ```
    CREATE USER 'appuser'@'localhost' IDENTIFIED BY 'AppPassword!';
    GRANT SELECT, INSERT, UPDATE ON database.* TO 'appuser'@'localhost';
    ``` 
    

### 33. Use Read-Only Replicas

-   **Step 1:** Configure MySQL replication with read-only replicas.
-   **Step 2:** Set `read_only` variable on replicas:
    
    
    ```
    SET GLOBAL read_only = 1;
    ``` 
    

### 34. Limit Access to Administrative Commands

-   **Step 1:** Restrict access to administrative commands by controlling user privileges.

### 35. Monitor and Patch Vulnerabilities

-   **Step 1:** Regularly scan for vulnerabilities.
-   **Step 2:** Apply patches and updates as they become available.

### 36. Use Per-User Encryption Keys

-   **Step 1:** Implement per-user encryption keys as needed in your application.

### 37. Implement Database-Level Firewalls

-   **Step 1:** Use additional firewall layers, such as network firewalls, to protect the database.

### 38. Secure MySQL Connections with VPNs

-   **Step 1:** Configure VPN access for secure connections to MySQL.

### 39. Avoid Storing Sensitive Data in Plaintext

-   **Step 1:** Encrypt sensitive data before storing it in MySQL.

### 40. Secure Backup Files

-   **Step 1:** Encrypt backup files and store them securely.

### 41. Review and Adjust `sql_mode` Settings

-   **Step 1:** Configure `sql_mode` in `my.cnf` to enforce strict SQL standards:
   
    
    ```
    sql_mode = 'STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'
    ``` 
    

### 42. Limit `innodb_file_format` to Barracuda

-   **Step 1:** Edit `my.cnf` to use the Barracuda file format:
    
    
    ```
    innodb_file_format = Barracuda
    ``` 
    

### 43. Enable `innodb_file_per_table`

-   **Step 1:** Edit `my.cnf`:
    ```
    innodb_file_per_table = 1
    ``` 
    

### 44. Avoid `GRANT ALL` Statements

-   **Step 1:** Use specific `GRANT` statements rather than `GRANT ALL`.

### 45. Use Secure User Authentication Plugins

-   **Step 1:** Install and configure secure authentication plugins.

### 46. Implement Connection Limits and Timeouts

-   **Step 1:** Set connection limits and timeouts in `my.cnf`
    
    ```
    max_connections = 100
    wait_timeout = 300
    ``` 
    

### 47. Regularly Review and Update Security Policies

-   **Step 1:** Periodically review and update security policies and practices.

### 48. Secure MySQL Server with Host-Based Firewall

-   **Step 1:** Configure host-based firewall rules to restrict access.

### 49. Avoid Using Deprecated Features

-   **Step 1:** Disable and avoid deprecated MySQL features.

### 50. Educate and Train Administrators

-   **Step 1:** Provide security training and best practices to MySQL administrators.
