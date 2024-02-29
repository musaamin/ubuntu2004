---
title: "Virtual Private Server"
date: 2020-12-14T20:56:40+07:00
draft: false
weight: 1
pre: "1. "
---

Virtual Private Server (VPS) adalah server yang dibangun dengan menggunakan hypervisor, perangkat lunak yang dapat membagi mesin fisik menjadi beberapa mesin virtual. VPS bisa didapatkan dengan menyewa dari perusahaan penyedia server dengan perhitungan biaya sewa minimum per bulan atau per jam.

## Kapan menggunakan VPS?
- Sumber daya yang dimiliki shared hosting sudah tidak mencukupi untuk menjalankan aplikasi (kelebihan beban)
- Shared hosting tidak mendukung untuk menjalankan aplikasi
- Server ingin dikontrol secara penuh
- Menginginkan keamanan dan privasi data, tidak tergabung dengan website lain

Yang harus dipersiapkan sebelum menggunakan VPS adalah tersedianya sumber daya manusia yang memiliki keterampilan sebagai seorang system administrator, khususnya administrasi sistem operasi server berbasis Linux, karena umumnya VPS tersedia dengan sistem operasi berbasis Linux. Selain itu harus menyiapkan biaya lebih dibandingkan dengan shared hosting.

## Menyewa dan Membuat VPS

Provider VPS di Indonesia umumnya menyewakan VPS dengan biaya sewa per bulan. Ada juga provider VPS dengan model cloud, menyewakan sesuai dengan pemakaian, minimum sewa per jam tapi harus deposit terlebih dahulu.

Cara membuat VPS sangat mudah, umumnya cukup dengan 5 langkah saja:

1. Memilih lokasi data center, jika memiliki banyak lokasi data center
1. Memilih spesifikasi server
1. Memilih sistem operasi
1. Memilih metode login SSH, memakai password atau key
1. Mengisi hostname


## Menghubungkan Domain ke VPS
Menghubungkan nama domain dengan VPS dapat dilakukan dengan melakukan konfigurasi pada DNS Record melalui DNS Management yang terdapat di client area penyedia nama domain.

**Tabel DNS Records**

| No | Type  | Hostname        | Value           |
|----|-------|-----------------|-----------------|
| 1  | A     | domain.com      | 64.20.51.164    |
| 2  | CNAME | www.domain.com  | domain.com      |
| 3  | CNAME | blog.domain.com | domain.com      |
| 4  | A     | app.domain.com  | 216.158.228.175 |

1. Type A merupakan nama domain domain.com yang mengarah ke server dengan Public IP 64.20.51.164. 
1. Type CNAME dengan hostname www.domain.com yang merupakan alias dari domain.com, artinya www.domain.com mengarah ke server yang sama dengan hostname domain.com. Dengan alias, dengan www atau tanpa www website tetap bisa diakses dan menampilkan halaman yang sama.
1. Sama dengan No.2 menggunakan CNAME mengarah ke server yang sama, tetapi bukan sebagai alias tetapi sebagai sub-domain.
1. Juga sebagai sub-domain, tetapi menggunakan type A karena berada di server yang berbeda, harus memasukkan IP address. 

### Free Credit Cloud Provider
Dapatkan free credit dari penyedia VPS untuk akun baru:
- [DigitalOcean](https://musaamin.web.id/go/digitalocean) free credit $100 berlaku selama 60 hari 
- [Vultr](https://musaamin.web.id/go/vultr) free credit $100 berlaku selama 14 hari 
- [UpCloud](https://musaamin.web.id/go/upcloud) free credit $25 berlaku selamanya