---
title: "System Monitoring"
date: 2020-12-16T10:53:05+07:00
draft: false
weight: 18
pre: "10. "
---

System Monitoring adalah pemantauan pemakaian sumber daya server seperti pemakaian CPU, pemakaian RAM, storage, network, dan process. Dengan mengetahui seberapa besar pemakaian sumber daya, dapat menjadi salah satu pertimbangan dalam memutuskan apakah perlu menaikkan atau menurunkan kapasitas sumber daya server.

## htop - Interactive Process Viewer

htop adalah aplikasi berbasis CLI untuk menampilkan process dan sumber daya. 

Install htop.

```
sudo apt install htop
```

Jalankan htop.

```
htop
```

![Gambar htop bagian atas](https://raw.githubusercontent.com/musaamin/ubuntu2004/main/static/images/htop-atas.jpg)

![Gambar htop bagian bawah](https://raw.githubusercontent.com/musaamin/ubuntu2004/main/static/images/htop-bawah.jpg)

## df - Report File System Disk Space Usage

df adalah aplikasi untuk mengetahui disk dan partisi apa saja yang ada di server beserta seberapa besar pemakaiannya.

```
df -hT
```

Contoh hasil perintah df.

```
Filesystem     Type      Size  Used Avail Use% Mounted on
udev           devtmpfs  974M     0  974M   0% /dev
tmpfs          tmpfs     200M  608K  199M   1% /run
/dev/vda1      ext4       30G  3.3G   25G  12% /
tmpfs          tmpfs     996M     0  996M   0% /dev/shm
tmpfs          tmpfs     5.0M     0  5.0M   0% /run/lock
tmpfs          tmpfs     996M     0  996M   0% /sys/fs/cgroup
tmpfs          tmpfs     200M     0  200M   0% /run/user/1001
```

Dari hasil perintah df di atas, terlihat bahwa partisi vda1 menggunakan file system ext4 dengan kapasitas penyimpanan 30GB, telah terpakai sebesar 3.3GB atau 12%, tersisa 25GB, dan termount di / (root).

## Hetrixtools – Uptime Monitoring

[HetrixTools](https://hetrixtools.com/) adalah aplikasi cloud untuk uptime monitoring website dan juga memiliki fitur tambahan untuk system monitoring. HetrixTools menyediakan paket Free yang dapat memantau sampai 15 website/server.

### Uptime Monitoring

Membuat uptime monitoring.

1. Login ke HetrixTools.
1. Klik menu Tools → Uptime Monitors.
1. Add Monitor.
1. Monitor Type = Website Monitor, Monitor Name = Nama Monitor, Website URL = URL dari website, Monitor From = Pilih 4 lokasi server yang melakukan uptime check
1. Add Monitor.

### Monitoring SSL dan Domain

HetrixTools dapat mengirimkan peringatan atau notifikasi ketika mendekati masa habis berlakunya sertifikat SSL dan domain, serta ketika terjadi perubahan nameserver pada domain.
1. Klik nama monitor.
1. SSL Cert. Expiration = pilih kapan mengirimkan peringatan SSL
1. Domain Expiration = pilih kapan mengirimkan peringatan domain
1. Domain NameServers = pilih kapan mengirimkan peringatan nameserver

### Server Monitoring

HetrixTools menyediakan fitur untuk server monitoring.

Install HetrixTools Server Monitoring Agent.

1. Klik  nama monitor.
1. Klik _Click to deploy our Server Monitoring Agent_.
1. Centang _View Running Processes_
1. Centang _Monitor service?_, masukkan nama service yang ingin dipantau, misal apache2, nginx, mariadb, php7.4-fpm, ssh.
1. Centang _Monitor port connections?_, masukkan nomor port yang ingin dipantau, misal 80, 443, 3306, 50000.
1. Copy _Install Code_, lalu jalankan di server sebagai user root.

### Notifikasi ke Telegram

Terdapat 12 channel pengiriman notifikasi, secara default yang aktif adalah email yang digunakan saat mendaftar. 

Mengaktifkan notifikasi ke Telegram.

1. Klik nama account HetrixTools.
1. Klik _Contact Lists_.
1. Klik icon Edit di kolom _Actions_.
1. Klik Telegram.
1. Buka aplikasi Telegram, Cari HetrixTools bot (@hetrixtools_bot), klik _Start_ atau ketik _/start_, HetrixTools bot membalas dengan _Chat ID_. Copy _Chat ID_.
1. Paste di kolom opsi Telegram pada _Edit Contact List_.
1. Klik _Send test notification_ untuk menguji pengiriman notifkasi ke Telegram.
1. Klik _Edit_ untuk simpan.

Menguji notifikasi ke Telegram.

1. Hentikan web server service untuk menguji status uptime dari website.
1. Jika berhasil, HetrixTools akan mengirimkan notifkasi website down ke email dan Telegram. 
1. Jalankan kembali service milik web server, akan dikirimkan notifikasi bahwa website sudah up.

