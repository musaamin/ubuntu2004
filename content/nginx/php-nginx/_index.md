---
title: "PHP dengan Nginx"
date: 2020-12-16T09:09:53+07:00
draft: false
weight: 13
pre: ""
---

Nginx web server bisa menjalankan script PHP dengan menggunakan PHP-FPM.

## Install PHP-FPM
Di Ubuntu 20.04, versi PHP yang tersedia secara default yaitu PHP 7.4. Untuk memasang PHP versi yang lain, kita harus memasang repository tambahan dari PPA. 

```
sudo add-apt-repository ppa:ondrej/php
```

Install PHP-FPM dan extension yang umumnya dibutuhkan.

```
sudo apt install php7.4 php7.4-fpm php7.4-common php7.4-cli php7.4-mbstring php7.4-gd php7.4-intl php7.4-xml php7.4-mysql php7.4-zip php7.4-json
```

Menguji hasil install dengan mengecek versi PHP yang terpasang.

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

PHP-FPM ini memiliki service yang harus aktif agar script PHP bisa dijalankan. 

Cek status PHP-FPM service.

```
sudo systemctl status php7.4-fpm
```

## Konfigurasi Server Block untuk PHP

Buka file konfigurasi server block.

```
cd /etc/nginx/conf.d
sudo nano domain.com.conf
```

Yang diubah adalah opsi **index**, **location /**, dan ditambah dengan **location ~\.php$**. 

Ubah konfigurasi server block menjadi seperti di bawah ini. 

```
server {
    listen 80;
    server_name www.domain.com domain.com;
    root /var/www/domain.com;
    index index.php index.html index.htm;

    location / {
      try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
      try_files $fastcgi_script_name =404;
      include fastcgi_params;
      fastcgi_pass    unix:/run/php/php7.4-fpm.sock;
      fastcgi_index   index.php;
      fastcgi_param DOCUMENT_ROOT    $realpath_root;
      fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name; 
    }

    access_log /var/log/nginx/domain.com_access.log;
    error_log /var/log/nginx/domain.com_error.log;
}
```

Menguji konfigurasi lalu restart Nginx service.

```
sudo nginx -t
sudo systemctl restart nginx
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

## Konfigurasi PHP untuk PHP-FPM

Beberapa konfigurasi umum yang perlu dilakukan di PHP. Nilai dari opsi konfigurasi disesuaikan dengan kebutuhan.

Buka file konfigurasi php.ini.

```
sudo nano /etc/php/7.4/fpm/php.ini
```

Opsi konfigurasi yang diubah yaitu batasan file upload, durasi eksekusi script dalam satuan detik, dan batasan memory yang digunakan oleh sebuah script.

```
upload_max_filesize = 10M
post_max_size = 10M
max_execution_time = 300
max_input_time = 300
memory_limit = 128M
```

Restart PHP-FPM service, lalu browse kembali halaman phpinfo untuk melihat apakah konfigurasi PHP sudah berubah.