---
title: "Nginx Web Server"
date: 2020-12-16T08:59:54+07:00
draft: false
weight: 8
pre: "7. "
---

Nginx (dibaca Egine X) adalah web server kedua yang paling banyak digunakan di internet setelah Apache. Nginx lebih hemat dalam penggunaan resource memori. Beberapa perusahaan teknologi yang menggunakan Nginx yaitu OpenDNS, CloudFlare, Adobe, BuzzFeed, ARM, dan WordPress.

## Install Nginx

Install Nginx lalu cek status nginx service.

```
sudo apt install nginx
sudo systemctl status nginx
```

Browse http://serverIP untuk menguji apakah Nginx sudah berjalan dengan baik.

## Server Block

Untuk menghosting banyak domain dalam satu Nginx web server, konfigurasikan Server Block (di Apache dikenal sebagai Virtual Host).

Membuat folder document root dan membuat file test.html

```
cd /var/www
sudo mkdir domain.com
sudo nano domain.com/test.html
```

Masukkan teks ke dalam file test.html.

```
<h1>ini adalah halaman test dari domain.com</h1>
```

Mengubah permission dan ownership folder document root.

```
sudo chown -R www-data:www-data domain.com
sudo chmod -R 755 domain.com
```

Membuat file konfigurasi server block untuk domain.com.

```
cd /etc/nginx/conf.d
sudo nano domain.com.conf
```

Masukkan konfigurasi server block.

```
server {
    listen 80;
    server_name www.domain.com domain.com;
    root /var/www/domain.com;
    index index.html index.htm;

    location / {
    	try_files $uri $uri/ =404;
    }

    access_log /var/log/nginx/domain.com_access.log;
    error_log /var/log/nginx/domain.com_error.log;
}
```

Cek apakah tidak ada kesalahan konfigurasi.

```
sudo nginx -t
```

Pesan yang ditampilkan jika tidak ada kesalahan.

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Restart nginx service dan cek statusnya.

```
sudo systemctl restart nginx
sudo systemctl status nginx
```

Browse http://domain.com/test.html, harus menampilkan teks dari file test.html.