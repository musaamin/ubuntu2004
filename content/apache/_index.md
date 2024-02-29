---
title: "Apache Web Server"
date: 2020-12-15T21:31:26+07:00
draft: false
weight: 6
pre: "6. "
---

Apache HTTP Server adalah aplikasi server untuk menyediakan layanan web atau berfungsi sebagai web server (HTTP server). Apache HTTP Server (httpd) terlahir pada tahun 1995 dan telah menjadi web server paling populer sejak April 1996.

## Install Apache

Install paket apache2 dan cek status apache2 service apakah sudah berjalan.

```
sudo apt install apache2
sudo systemctl status apache2
```

Browse http://serverIP untuk menguji apakah Apache sudah berjalan dengan baik.

## Virtual Host

Dalam satu server kita dapat menghosting banyak domain atau subdomain. Yang harus dilakukan adalah pertama melakukan konfigurasi DNS records untuk mendefinisikan di mana IP server dari domain atau subdomain. Yang kedua adalah melakukan konfigurasi Virtual Host di Apache.

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

Ubah konfigurasi default virtual host.

```
sudo nano /etc/apache2/sites-available/000-default.conf
```

Aktifkan opsi ServerName dan pasang server IP address.

```
ServerName 64.20.51.164
```

Membuat konfigurasi virtual host untuk domain.com.

```
cd /etc/apache2/sites-available
sudo nano domain.com.conf
```

Masukkan konfigurasi virtual host.

```
<VirtualHost *:80>
    ServerName www.domain.com
    ServerAlias domain.com
    DocumentRoot /var/www/domain.com
    <Directory /var/www/domain.com>
        Options -Indexes +FollowSymLinks +MultiViews
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog /var/log/apache2/domain.com_error.log
    CustomLog /var/log/apache2/domain.com_access.log combined
</VirtualHost>
```

Cek apakah tidak ada kesalahan konfigurasi.

```
sudo apache2ctl -t
```

Pesan yang ditampilkan jika tidak ada kesalahan.

```
Syntax OK
```

Aktifkan virtual host, modul rewrite, dan restart apache2 service.

```
sudo a2ensite domain.com.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
sudo systemctl status apache2
```

Browse http://domain.com/test.html, harus menampilkan teks dari file test.html.