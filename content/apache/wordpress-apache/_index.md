---
title: "WordPress dengan Apache"
date: 2020-12-16T06:06:16+07:00
draft: false
weight: 11
pre: ""
---

WordPress adalah Content Management System (CMS) berbasis PHP yang paling populer di dunia, bisa digunakan sebagai blog, media online, profil perusahaan, profil institusi pendidikan, sampai menjadi toko online.

## Install WordPress

Sebelumnya sudah dibahas tentang Apache web server, MariaDB database, dan PHP. Semua itu dibutuhkan untuk instalasi  atau menjalankan WordPress. Jika semuanya sudah siap, lanjut ke instalasi WordPress.

Download WordPress terbaru.

```
wget -c https://wordpress.org/latest.tar.gz -O wordpress.tar.gz
```

Extract wordpress.tar.gz.

```
tar xzvf wordpress.tar.gz
```

Copy isi folder wordpress ke folder document root domain.com.

```
sudo cp -Rv wordpress/* /var/www/domain.com
```

Mengubah ownership dan permission folder domain.com.

```
sudo chown -R www-data:www-data /var/www/domain.com
sudo chmod -R 755 /var/www/domain.com
```

Browse http://domain.com untuk install WordPress.