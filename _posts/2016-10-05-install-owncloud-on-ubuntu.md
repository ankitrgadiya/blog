---
layout: post
title: Installing Owncloud 9 on Ubuntu 16.04 LTS
---
## Clouds

<br />
Most of the modern users now-a-days store there data on cloud storage. Cloud storages generally give some free space to everyone and they charge for more storages. Common cloud storage providers include Google Drive, One Drive, DropBox. Generally you get around 10-15GB storage for free.

## Owncloud

<br />
Owncloud is kinda replacement for cloud storages which you can host on your own servers. Its free and open-source. Also as you're hosting on your own servers so you don't have to worry about safety. And you can store as much data as your server supports. So, no limitation.

Basically, it is a software which will run on your server and provides a cloud experience. It can be installed on both Windows servers and Linux based servers.

## How to install Owncloud on Ubuntu

<br />
* Install apache and mysql packagges.
```bash
$ sudo apt-get install apache2 mysql-server
```
* Owncloud is not available in Ubuntu repositories but Owncloud maintain a repository for Ubuntu. We have to add the repository.
```bash
$ curl https://download.owncloud.org/download/repositories/
stable/Ubuntu_16.04/Release.key | sudo apt-key add -
$ echo 'deb http://download.owncloud.org/download/repositories/
stable/Ubuntu_16.04/ /' | sudo tee /etc/apt/sources.list.d/
owncloud.list
$ sudo apt-get update
```
* Install Owncloud packages
```bash
$ sudo apt-get install owncloud
```
* Configure MySQL Database
```bash
$ mysql -u root -p
Enter password:  
mysql> CREATE DATABASE owncloud;  
Query OK, 1 row affected(0.00 sec)  
mysql> GRANT ALL ON owncloud.* to 'owncloud'@'localhost'
 IDENTIFIED BY 'password';  
Query OK, 0 rows affected, 1 warning(0.10 sec)  
mysql> FLUSH PRIVILEGES;  
QUERY OK, 0 rows affected(0.00 sec)  
mysql>exit;  
```
* Setup Owncloud

  * Open browser
  * Go to localhost/owncloud
  * Create admin accound
  * Scroll down and choose MySQL database
  * Enter Database information
  * Click Finish

* Done!
<iframe width="640" height="360" src="https://www.youtube.com/embed/BLte3NRYxjI?rel=0" frameborder="0" allowfullscreen></iframe>
