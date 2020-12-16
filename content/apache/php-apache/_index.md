---
title: "PHP dengan Apache"
date: 2020-12-15T22:00:56+07:00
draft: false
weight: 6
pre: ""
---

PHP adalah salah satu bahasa pemrograman paling populer di dunia untuk membangun aplikasi web. Agar bisa menjalankan aplikasi PHP, kita harus memasang PHP engine beserta modul Apache untuk membaca PHP.

## Install PHP

Install modul Apache, PHP, dan extension PHP yang sering digunakan.

```
sudo apt install php libapache2-mod-php php-cli php-common php-mbstring php-gd php-intl php-xml php-mysql php-zip php-json
```

Memverifikasi hasil instalasi PHP dengan menampilkan versi PHP.

```
php -v
```

Hasil perintah di atas.

```
PHP 7.4.3 (cli) (built: Oct  6 2020 15:47:56) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.3, Copyright (c), by Zend Technologies

```

## Pengujian PHP

Pengujian dilakukan dengan membuat file .php yang berisi fungsi phpinfo() untuk menampilkan informasi konfigurasi PHP yang terpasang. 

Membuat file info.php.

```
nano /var/www/domain.com/info.php
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