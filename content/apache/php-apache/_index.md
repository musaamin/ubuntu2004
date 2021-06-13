---
title: "PHP dengan Apache"
date: 2020-12-15T22:00:56+07:00
draft: false
weight: 6
pre: ""
---

PHP adalah salah satu bahasa pemrograman paling populer di dunia untuk membangun aplikasi web. Agar bisa menjalankan aplikasi PHP, kita harus memasang PHP engine beserta modul Apache untuk membaca PHP.

## Install PHP
Di Ubuntu 20.04, versi PHP yang tersedia secara default yaitu PHP 7.4. Untuk memasang PHP versi yang lain, kita harus memasang repository tambahan dari PPA. 

```
sudo add-apt-repository ppa:ondrej/php
```

Install modul Apache, PHP, dan extension PHP yang sering digunakan.

```
sudo apt install php7.4 libapache2-mod-php7.4 php7.4-cli php7.4-common php7.4-mbstring php7.4-gd php7.4-intl php7.4-xml php7.4-mysql php7.4-zip php7.4-json
```

Memverifikasi hasil instalasi PHP dengan menampilkan versi PHP.

```
php -v
```

Hasil perintah di atas.

```
PHP 7.4.13 (cli) (built: Nov 28 2020 06:24:59) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.13, Copyright (c), by Zend Technologies

```

## Pengujian PHP

Pengujian dilakukan dengan membuat file .php yang berisi fungsi phpinfo() untuk menampilkan informasi konfigurasi PHP yang terpasang. 

Membuat file info.php.

```
sudo nano /var/www/domain.com/info.php
```

Masukkan script PHP di bawah ini.

```
<?php phpinfo(); ?>
```

Browse http://domain.com/info.php, harus menampilkan halaman phpinfo yang berisi informasi versi PHP beserta extension yang terpasang.

## Konfigurasi PHP

Beberapa konfigurasi umum yang perlu dilakukan di PHP. Nilai dari opsi konfigurasi disesuaikan dengan kebutuhan.

Buka file konfigurasi php.ini.

```
sudo nano /etc/php/7.4/apache2/php.ini
```

Opsi konfigurasi yang diubah yaitu batasan file upload, durasi eksekusi script dalam satuan detik, dan batasan memory yang digunakan oleh sebuah script.

```
upload_max_filesize = 10M
post_max_size = 10M
max_execution_time = 300
max_input_time = 300
memory_limit = 128M
```

Restart apache2 service, lalu browse kembali halaman phpinfo untuk melihat apakah konfigurasi PHP sudah berubah.