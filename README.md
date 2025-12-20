# 🚀 EVE-NG MySQL System Database Recovery

![EVE-NG](https://img.shields.io/badge/EVE--NG-Recovery-blue?style=for-the-badge)
![MySQL](https://img.shields.io/badge/MySQL-Fix-orange?style=for-the-badge)
![Ubuntu](https://img.shields.io/badge/Ubuntu-Server-red?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Fixed-green?style=for-the-badge)

This guide documents a real-world scenario where **MySQL on an EVE-NG server** fails due to a missing or corrupted system database.  
Follow this step-by-step recovery process to restore MySQL and get your EVE-NG lab back online.  

---

## ❗ Problem Overview

After **disk space exhaustion** on Ubuntu-based EVE-NG, MySQL service failed to start. Common errors:

- ❌ MySQL service not running  
- ❌ MySQL system database not found  
- ❌ Missing files under `/var/lib/mysql`  
- ❌ EVE-NG Web UI inaccessible  

> If not fixed, this can **break your entire lab environment**.  

<details>
<summary>📸 Problem Screenshot</summary>
<img src="images/problem/mysql crash on eve-ng.png" alt="MySQL crash on EVE-NG" width="100%"/>
</details>

---

## 🕵️ Root Cause

The **MySQL system database** was corrupted or partially deleted when the disk was full, causing **missing internal tables** required for startup.  

---

## 📌 What This Guide Covers

- ✅ Verify MySQL service and logs  
- ✅ Identify corrupted/missing system databases  
- ✅ Rebuild MySQL system tables safely  
- ✅ Restore MySQL service functionality  
- ✅ Validate EVE-NG Web UI  

---

## 🖥️ Environment

- **EVE-NG** (Community/Pro)  
- Ubuntu Server  
- MySQL Server (v8)  
- LVM-backed storage  

---

## 🔧 Troubleshooting Steps

<details>
<summary>Click to Expand Troubleshooting</summary>

### 1️⃣ Check Disk Space and Initial MySQL Installation

```bash
sudo apt install mysql-server-8
df 
```

 Initial installation failed due to full disk space

  Cleared unnecessary files in /opt/unetlab/addons/qemu

  Verified disk space after cleanup:

<img src="images/problem/checking disk free space-worked.png" alt="Checking Disk Space" width="100%" />

 Reinstalled MySQL:
 ```bash
 sudo apt install mysql-server-8
```
MySQL installed, but service still won’t start

</details>
 ## 🚀 Solution

Follow the steps below in order:
<details>

### 1️⃣ Stop any running MySQL services
```bash
systemctl stop mysql
pkill -i mysqld
```
###2️⃣ Remove the existing MySQL data directory
rm -rf /var/lib/mysql

###3️⃣ Create a new MySQL data directory
mkdir /var/lib/mysql

###4️⃣ Set correct ownership and permissions
chown -R mysql:mysql /var/lib/mysql
chmod 750 /var/lib/mysql

###5️⃣ Reinitialize MySQL system databases
mysqld --initialize-insecure --user=mysql

###6️⃣ Start the MySQL service
sudo systemctl start mysql

###7️⃣ Verify that MySQL is running
sudo systemctl status mysql

<img src="images/solution/fixed mysql server.png" alt="MySQL service running" />

✅ After completing these steps, the MySQL server should be up and running.
</details>

## 🗄️ Restoring the EVE-NG Database

<details>
<summary>Click to Expand Database Restoration</summary>

### 1️⃣ Login to MySQL

```bash
sudo mysql -uroot -p
```
2️⃣ Create database and user
```sql
CREATE DATABASE eve_ng_db;
CREATE USER 'eve_ng'@'localhost' IDENTIFIED BY 'eve_ng';
GRANT ALL PRIVILEGES ON eve_ng_db.* TO 'eve_ng'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
<img src="images/solution/recreating eve-ng database.png" alt="Recreating EVE-NG database" width="100%" /> </details>

## 🌐 Fixing Web Access

<details>
<summary>Click to Expand Web Access Fix</summary>

### 1️⃣ Fix EVE-NG permissions

```bash
unl_wrapper -a fixpermissions
```

## 2️⃣ Restore database
```bash
unl_wrapper -a restoredb
```
## 3️⃣ Check Apache service
```bash
sudo systemctl status apache2
```
## 4️⃣ Verify your lab in the browser
<img src="images/solution/lab up and running.png" alt="EVE-NG lab running" width="100%" />

✅ Your EVE-NG lab is now fully operational!

</details> 

---

## 🏷️ Tags & Mentions

### Companies & Platforms
- [Dobre Technologies](https://www.dobretech.com/)
- [EVE-NG](https://www.eve-ng.net/)
- [MySQL](https://www.mysql.com/)
- [Ubuntu](https://ubuntu.com/)
- [Linux Foundation](https://www.linuxfoundation.org/)
- [Cisco Networking](https://www.cisco.com/)
- [VMware](https://www.vmware.com/)

### Communities & Forums
- [EVE-NG Forum](https://www.eve-ng.net/index.php/forums/)
- [Reddit r/networking](https://www.reddit.com/r/networking/)
- [Reddit r/linuxadmin](https://www.reddit.com/r/linuxadmin/)
- [Stack Overflow](https://stackoverflow.com/)

### Hashtags
`#EVE-NG #NetworkingLab #VirtualLab #SysAdmin #Linux #UbuntuServer #MySQL #LabRecovery #DevOps #NetworkEngineering #CyberSecurity #CloudLab #Infrastructure #TechCommunity #LearningLab #HomeLab #ITAutomation`

### SEO Keywords
`eve-ng-mysql-corruption-fix, eve-ng-mysql-service-recovery, eve-ng-mysql-database-repair, eve-ng-mysql-not-running-fix, eve-ng-mysql-var-lib-mysql-repair`

Repository Topics: `EVE-NG, MySQL, Ubuntu, LabRecovery, VirtualLab, NetworkLab, SysAdmin`

