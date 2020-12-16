---
title: "Let's Encrypt untuk Apache"
date: 2020-12-16T05:56:52+07:00
draft: false
weight: 10
pre: ""
---

SSL (Secure Socket Layer) dan versi baru TLS (Transport Layer Security) adalah protokol kriptografi yang berfungsi untuk menyediakan jalur komunikasi yang aman melalui jaringan. Pada website SSL/TLS digunakan untuk mengamankan komunikasi antara web server dengan web browser. Google kini lebih mengutamakan website yang telah menggunakan SSL (HTTPS) di hasil pencarian. Secara default website berjalan pada port 80 dengan protokol HTTP, website yang menggunakan SSL/TLS berjalan pada port 443 dengan protokol HTTPS (HTTP Secure/HTTP over SSL/TLS).

## Install Certbot

Install certbot via snap.

```
sudo snap install --classic certbot
```

Membuat symbolic link untuk certbot.

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

## Install SSL untuk Apache

Domain yang akan dipasangkan SSL harus sudah normal diakses melalui protokol HTTP.

Request SSL untuk domain.com yang terpasang di Apache.

```
sudo certbot --apache -d domain.com -d www.domain.com
```

Untuk menguji apakah SSL sudah terpasang dengan baik, browse http://domain harus secara otomatis redirect ke https://domain.com. Selain itu bisa menggunakan online tools dari https://www.ssllabs.com/ssltest/.

## Memperbarui SSL

SSL dari  Let’s Encrypt hanya berlaku selama 3 bulan, harus diperbarui secara berkala. Untuk memperbarui SSL dari Let’s Encrypt cukup menjalankan certbot dengan parameter renew.

```
sudo certbot renew
```