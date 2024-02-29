---
title: "Log File"
date: 2020-12-16T11:14:17+07:00
draft: false
weight: 19
pre: "11. "
---

Log file adalah file hasil rekaman dari kegiatan yang terjadi di sistem operasi atau perangkat lunak. Dengan menganalisa log fie seorang sysadmin dapat mendapatkan petunjuk jika terjadi kesalahan di dalam sistem atau aplikasi. Secara default lokasi log file berada di /var/log.

## auth.log - Authorization Log

File auth.log adalah log file yang merekam aktivitas authorization seperti perintah sudo dan remote login SSH.

```
sudo su
tail -f /var/log/auth.log
```

Menyaring log file, mencari pesan invalid.

```
cat /var/log/auth.log | grep "Invalid"
```

## lastlog - Login Terakhir

lastlog adalah log file yang merekam aktivitas login terakhir. Log file ini dapat ditampilkan dengan menggunakan perintah lastlog.

```
lastlog
```

## Apache Log File

Apache memiliki 2 log file, yang pertama access.log yang merekam setiap aktivitas request dari client. Yang kedua error.log yang merekam setiap error yang terjadi. Jika aplikasi web tidak berjalan sebagaimana mestinya periksalah error.log. Setiap virtual host dapat memiliki log file tersendiri, terpisah dengan log file utama dan virtual host lainnya.

```
tail -f /var/log/apache2/access.log
tail -f /var/log/apache2/domain.com_access.log
tail -f /var/log/apache2/error.log
tail -f /var/log/apache2/domain.com_error.log

```

## Nginx Log File

Sama seperti Apache, Nginx juga memiliki log file untuk access dan error, serta log file masing-masing server block.

```
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/domain.com_access.log
tail -f /var/log/nginx/error.log
tail -f /var/log/nginx/domain.com_error.log

```

