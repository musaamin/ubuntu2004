---
title: "Backup Data"
date: 2020-12-16T11:20:15+07:00
draft: false
weight: 20
pre: "12. "
---

Melakukan backup data yang sudah berjalan di server sangat penting, karena hasil backup dapat dikembalikan lagi jika terjadi insiden keamanan yang tidak diinginkan. Contoh insiden yang bisa saja terjadi, website menjadi korban hacking, website terkena malware, atau bahkan kesalahan dari pemilik website sendiri (tidak sengaja menghapus file atau database). Data yang dibackup adalah file website beserta databasenya. Backup data dibuat dalam bash script kemudian dikombinasikan dengan cron untuk penjadwalan backup.

## Bash Script Backup

Langkah pertama yang harus dilakukan yaitu membuat bash script yang berisi perintah backup. Bash script adalah script program yang berisi perintah-perintah bash.

Membuat direktori /backup.

```
sudo mkdir -p /backup/domain.com/www
sudo mkdir /backup/domain.com/database
```

Membuat file bash script dengan nano.

```
sudo nano /backup/backup.sh
```

Masukkan bash script di bawah ini.

```
#!/bin/bash

rsync -avz /var/www/domain.com/ /backup/domain.com/www --delete
mysqldump -uwordpress -prahasia wordpress | gzip > /backup/domain.com/database/wordpress-$(date +%d%m%Y).sql.gz
```

Bash script di atas melakukan sinkronisasi direktori dengan perintah rsync. Kemudian melakukan dump database dan dikompresi agar file dump lebih kecil, selain itu nama file dump dibuat per hari.

Beri atribut execution dan uji menjalankan bash script.

```
cd /backup
sudo chmod 700 backup.sh
sudo ./backup.sh
```

Setelah menjalankan bash script, periksa isi dari direktori backup, apakah bash script berhasil melakukan sinkronisasi direktori dan dump database.

```
ls -l /backup/domain.com/www/
ls -l /backup/domain.com/database
```

## cron – Job Scheduler

cron adalah utility untuk penjadwalan tugas, perintah, atau script berdasarkan waktu dalam  sistem operasi. Bash script backup yang sudah dibuat dijadwalkan pada jam tertentu dengan cron agar bisa berjalan secara otomatis tanpa harus dijalankan lagi secara manual. 

Membuat cron job.

```
sudo crontab -e
```

Tambahkan cron job di bawah ini.

```
0 0 * * * /bin/bash /backup/backup.sh
```

Cron job di atas akan menjalankan bash script backup.sh pada jam 00.00 setiap hari.

Format penulisan waktu di cron.

```
*     *     *     *     *  
-     -     -     -     -
|     |     |     |     |
|     |     |     |     +----- hari (0 - 6) (0=Minggu)
|     |     |     +------- bulan (1 - 12)
|     |     +--------- tanggal (1 - 31)
|     +----------- jam (0 - 23)
+------------- menit (0 – 59)
```

Contoh penulisan waktu di cron.

```
* * * * * <command> #setiap menit
30 * * * * <command> #setiap 30 menit
45 6 * * * <command> #setiap jam 6.45
45 18 * * * <command> #setiap jam 18.45
00 1 * * 0 <command> #setiap jam 1 hari Minggu
00 1 * * 7 <command> #setiap jam 1 hari Minggu
00 1 * * Sun <command> #setiap jam 1 hari Minggu
30 8 1 * * <command> #setiap jam 8.30 tanggal 1 setiap bulan
00 0-23/2 02 07 * <command> #setiap jam pada tanggal 2 Juli
```

Untuk memudahkan dalam penulisan waktu di cron dapat menggunakan crontab editor di [crontab.guru](https://crontab.guru)