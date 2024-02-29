---
title: "Let's Encrypt untuk Nginx"
date: 2020-12-16T09:44:22+07:00
draft: false
weight: 15
---

SSL (Secure Socket Layer) dan versi baru TLS (Transport Layer Security) adalah protokol kriptografi yang berfungsi untuk menyediakan jalur komunikasi yang aman melalui jaringan. Pada website SSL/TLS digunakan untuk mengamankan komunikasi antara web server dengan web browser. Google kini lebih mengutamakan website yang telah menggunakan SSL (HTTPS) di hasil pencarian. Secara default website berjalan pada port 80 dengan protokol HTTP, website yang menggunakan SSL/TLS berjalan pada port 443 dengan protokol HTTPS (HTTP Secure/HTTP over SSL/TLS).

## Install Certbot

Install certbot untuk Nginx.

```
sudo apt install certbot python3-certbot-nginx
```

## Install SSL untuk Nginx

Domain yang akan dipasangkan SSL harus sudah dapat diakses melalui protokol HTTP.

Request SSL untuk domain.com yang terpasang di Nginx.

```
sudo certbot --nginx -d domain.com -d www.domain.com
```

Masukkan alamat email untuk menerima notifikasi ketika SSL akan habis masa berlakunya, perlu dilakukan pembaruan SSL.

```
Enter email address (used for urgent renewal and security notices) (Enter 'c' to
cancel): email@domain.com
``` 

Jawab 'A (Agree)' untuk menyetujui Terms of Service.

```
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server at
https://acme-v02.api.letsencrypt.org/directory
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(A)gree/(C)ancel: A
```

Jawab 'Y (Yes)' jika ingin dikirimkan informasi ke email.

```
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about our work
encrypting the web, EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y
```

Jawab '2' agar semua permintaan secara otomatis dialihkan ke protokol HTTPS.

```
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
```

SSL berhasil terpasang.

```
Congratulations! You have successfully enabled https://domain.com and
https://www.domain.com
```

Untuk menguji apakah SSL sudah terpasang dengan baik, browse http://domain harus secara otomatis redirect ke https://domain.com. Selain itu bisa menggunakan online tools dari https://www.ssllabs.com/ssltest/.

## Memperbarui SSL

SSL dari  Let’s Encrypt hanya berlaku selama 3 bulan, harus diperbarui secara berkala. Untuk memperbarui SSL dari Let’s Encrypt cukup menjalankan certbot dengan parameter renew.

```
sudo certbot renew
```