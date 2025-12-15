# EVE-NG MySQL System Database Recovery

This repository documents a real-world issue encountered on an EVE-NG server where the MySQL service failed to start due to a missing or corrupted system database.

## Problem Description

After disk space exhaustion and cleanup on an Ubuntu-based EVE-NG server, MySQL failed to start and returned errors such as:

- MySQL service not running
- MySQL system database not found
- Missing files under `/var/lib/mysql`
- EVE-NG web UI inaccessible

This issue can completely break an EVE-NG lab environment if not handled correctly.
<img src="images/problem/mysql crash on eve-ng.png" />
## Root Cause

The MySQL system database became corrupted or partially deleted while the disk was full, leading to missing internal MySQL tables required for startup.

## What This Repository Covers

- Verifying MySQL service and error logs
- Identifying corrupted or missing MySQL system databases
- Safely rebuilding MySQL system tables
- Restoring MySQL service functionality
- Validating EVE-NG web UI after recovery

## Environment

- EVE-NG (Community/Pro)
- Ubuntu Server
- MySQL Server
- LVM-backed storage

## Troubleshooting
After getting the /var/lib/mysql not found, I tried installing mysql by running 
<br><br>
<i>sudo apt install mysql-server-8</i>
<br>
- I received a message saying the disk was full.
- i ran the command df and i saw that the filesystem /dev/mapper/ubuntu--vg-ubuntu--lv had Use% 100
- so i resolved this issue by deleting some less important files within the /opt/unetlab/addons/qemu that was less important to create space
- i later ran the df command and i had the result below.
<img src="images/problem/checking disk free space-worked.png"  width="100%" height="900px"/>

<br>
I re-ran 
<br>
<i>sudo apt install mysql-server-8</i> 
<br>
- Mysql server was installed but the problem wasn't solved
- Mysl service still wasn't able to start
<br>

## Solution
 <br>
 
- Ensure to Kill any mysql service running using
  <br>
  <i>systemctl stop mysql</i>
  <i>pkill -i mysqld</i>
  <br>
- Remove the any /var/lib/mysql directory present
  <br>
  <i>rm -rf /var/lib/mysql </i>
  <br>
- Create a new /var/lib/mysql directory
  <br>
  <i>mkdir /var/lib/mysql</i>
  <br>
- Adjust the permission to the newly directory created
  <br>
  <i>chown -R mysql:mysql /var/lib/mysql </i>
  <br>
  <i>chmod 750 /var/lib/mysql </i>
  <br>
- Reinitialize to re-create the mysql database
  <br>
  <i>mysqld --initialize-insecure --user=mysql</i>
  <br>
- Try to start the mysql service
  <br>
  <i>sudo systemctl start mysql</i>
  <br>

- Check the service to ensure its running
 <br>
  <i>sudo systemctl status mysql</i>

<br>
<img src="images/solution/fixed mysql server.png" />
- After doing all that, the mysql server should be up and running !
<br>

## Restoring the database schema

- Login to the mysql server by running sudo mysql -uroot -p
- While logged in, Create the following;
  - create database
  - create the user account
  - grant all privileges to the account
  - Flush privileges
  - exit;
 
<br>
<img src="images/solution/recreating eve-ng database.png" />
<br>
<br>

## Fixed web Access
- Fixed all necessary permission using the command
  unl_wrapper -a fixpermissions
- Restart database
  unl_wrapper -a restoredb
- Check apache service to see if running
  sudo systemctl status apache2
<br>
<img src="images/solution/restarting db.png" />
<br>

  - check to see if your lab is accessible on browser
  
<br>
<br>
     Hurray! 
<br>
<br>
<img src="images/solution/lab up and running.png" />
