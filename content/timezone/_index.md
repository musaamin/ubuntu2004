---
title: "Time Zone"
date: 2020-12-15T21:28:07+07:00
draft: false
weight: 4
pre: "4. "
---

Kekeliruan pengaturan waktu atau tanggal pada server dapat mengakibatkan kekeliruan data pada aplikasi, waktu log sistem, waktu penjadwalan otomatis (cron), dan aplikasi lain yang membutuhkan tanggal. Contohnya tanggal yang terdapat di nota pemesanan dan pembelian pada aplikasi e-commerce, atau kesalahan waktu diskon.

Menampilkan semua time zone.

```
sudo timedatectl list-timezones
```

Menampilkan time zone Asia.

```
sudo timedatectl list-timezones | grep Asia
```

Mengubah time zone ke Asia/Jakarta (WIB)

```
sudo timedatectl set-timezone Asia/Jakarta
```

Menampilkan time zone yang terpasang.

```
sudo timedatectl
date
```