---
title: "MariaDB Database"
date: 2020-12-16T05:13:01+07:00
draft: false
weight: 8
pre: "7. "
---

MariaDB adalah RDBMS yang merupakan forking dari MySQL yang sekarang sudah dimiliki oleh Oracle. Sudah banyak distro Linux yang telah memasukkan MariaDB ke dalam official repository dan XAMPP pun sudah migrasi dari MySQL ke MariaDB. Website besar yang menggunakan MariaDB yaitu Wikipedia, Google, booking.com, dan Craiglist.

## Install MariaDB

Install MariaDB, lalu cek status mariadb service.

```
sudo apt install mariadb-server
sudo systemctl status mariadb
```

## Mengamankan Instalasi MariaDB

Jalankan tool untuk pengamanan instalasi MariaDB.

```
sudo mysql_secure_installation
```

Lalu jawab pertanyaan yang diajukan seperti di bawah ini.

```
Enter current password for root (enter for none): ENTER
Switch to unix_socket authentication [Y/n] n
Change the root password? [Y/n] y
Remove anonymous users? [Y/n] y
Disallow root login remotely? [Y/n] y
Remove test database and access to it? [Y/n] y	
Reload privilege tables now? [Y/n] y 
```

## Membuat User & Database

Demi keamanan database, setiap database memiliki user tersendiri.

Login ke MariaDB dengan username root.

```
sudo mysql -u root -p
```

Membuat database, user, dan memberikan hak akses database ke user yang baru dibuat.

```
CREATE DATABASE wordpress;
CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'rahasia';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost';
FLUSH PRIVILEGES;
exit
```

Login kembali ke MariaDB dengan user yang baru dibuat untuk mengecek apakah user dan database yang dibuat.

```
mysql -u wordpress -p
SHOW DATABASES;
exit
```

