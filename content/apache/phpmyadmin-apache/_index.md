---
title: "phpMyAdmin dengan Apache"
date: 2020-12-16T05:23:58+07:00
draft: false
weight: 9
pre: ""
---

phpMyAdmin adalah aplikasi manajemen database MySQL/MariaDB. 

## Install phpMyAdmin

Download phpMyAdmin terbaru dari phpmyadmin.net.

```
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.4/phpMyAdmin-5.0.4-english.tar.gz
```

Extract filenya phpMyAdmin\*.tar.gz

```
tar xzvf phpMyAdmin-5.0.4-english.tar.gz
```

Pindahkan folder hasil extract dan buat symbolic link ke /var/www/html.

```
sudo mv phpMyAdmin-5.0.4-english /usr/share/phpmyadmin
sudo ln -s /usr/share/phpmyadmin /var/www/html
```

Browse phpMyadmin di http://serverIP/phpmyadmin.

## Disable root login

Membuat file config phpMyAdmin.

```
cd /usr/share/phpmyadmin
sudo cp config.sample.inc.php config.inc.php
sudo nano config.inc.php
```

Menonaktifkan user root agar tidak bisa login di phpMyAdmin. 

Tambahkan opsi di bawah ini setelah **/\* Server parameters \*/**.

```
$cfg['Servers'][$i]['AllowRoot'] = FALSE;
```