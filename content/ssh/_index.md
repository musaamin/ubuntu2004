---
title: "Secure Shell (SSH)"
date: 2020-12-14T21:39:50+07:00
draft: false
weight: 2
pre: "2. "
---

Untuk bisa mengakses VPS digunakan protokol SSH (Secure Shell). SSH server secara default telah aktif di VPS agar dapat langsung kita remote. Informasi VPS yang dibutuhkan untuk SSH remote adalah Public IP address, username, password, dan nomor port SSH.

## Mengakses VPS di Windows
Mengakses VPS dari Windows memerlukan aplikasi SSH client salah satunya Xshell. Xshell tersedia dalam  versi berbayar dan gratis (Free License). Cukup mengisi form download, link download terkirim ke email.

**Login SSH Memakai Password di Xshell**
1. Buat session baru
1. Masukkan Name = Nama session, Protocol = SSH, Host = Public IP, Port Number = Nomor Port SSH (default 22)
1. Klik Connect

## Mengakses VPS di Linux
Aplikasi SSH client di Linux menggunakan OpenSSH client yang sudah tersedia secara default di berbagai distro Linux. Aplikasi OpenSSH client ini berbasis Command Line Interface (CLI) yang diakses melalui terminal atau console.

**Login SSH Memakai Password di Linux**
1. Buka terminal
1. Ketik perintah ssh untuk login ke server, misal login SSH ke server IP 64.20.51.164 dengan user root 

```
ssh root@64.20.51.164
```
## Mengamankan SSH Server

**Mengganti Nomor Port SSH**

Secara default nomor port SSH yaitu 22. Mengganti dengan nomor lain dapat mencegah orang-orang atau bot yang ingin mencoba login SSH ke VPS. Gantilah dengan nomor port lain yang belum digunakan. Jika mengikuti standar dari IANA, nomor yang disarankan untuk digunakan yaitu mulai dari 49152 sampai 65535.

Misalnya mengubah port SSH ke 50000. 

Buka file konfigurasi SSH server

```
nano /etc/ssh/sshd_config
```

Cari baris opsi #Port 22, lepas tanda # (komentar), dan ganti nomor 22 ke 50000.

```
Port 50000
```

Lalu restart SSH service.

```
systemctl restart ssh
systemctl status ssh
```

Untuk remote SSH di Xshell jangan lupa ubah Port Number menjadi 50000.

Sementara di Linux, tambahkan parameter -p untuk custom port SSH.

```
ssh root@64.20.51.164 -p 50000
```

**Disable root Login**

Menonaktifkan login SSH dengan user root. Langkah pertama yang harus dilakukan yaitu membuat user baru, diberikan hak ases root.

```
adduser musa 
usermod -aG sudo musa 
```

Ubah kembali konfigurasi SSH server, cari opsi PermitRootLogin dan ubah nilainya menjadi no.

```
PermitRootLogin no
```

**Login SSH Memakai Key**

Login SSH dengan key lebih aman dibandingkan login menggunakan password.

**Membuat SSH Key di Xshell**
1. Tools → User Key Manager.
1. Generate.
1. Next.
1. Key Name = nama key, misal key-getbox.
1. Key Passphrase untuk pengamanan key, sebelum digunakan harus memasukkan Passphrase. Tidak harus diisi.
1. Finish.
1. Buka Properties Key.
1. Copy Public Key yang ditampilkan.

**Memasukkan Public Key dari Windows ke Server**
1. Login SSH ke VPS.
1. Buat folder .ssh di dalam direktori home jika belum ada.
1. Masuk ke folder .ssh.
1. Buat file authorized_keys jika belum ada.
1. Paste SSH key.

```
mkdir ~/.ssh
chmod 700 ~/.ssh
cd ~/.ssh
nano authorized_keys
chmod 600 authorized_keys
```

**Login SSH dengan Key di Xshell**
1. Buka Properties dari Session.
1. Klik menu Connection → Authentication.
1. Pilih Method = Public Key, Username = root, User Key = nama key yang sudah dibuat sebelumnya.
1. Klik Connect.

**Membuat SSH Key di Linux**
1. Jalankan perintah ssh-key untuk generate SSH key.
1. Berikan nama file lengkap dengan path direktori penyimpanan. 
1. Misal namanya key-server, tercipta 2 file yaitu key-server berisi Private Key dan key-server.pub berisi Public Key.

```
ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/musa/.ssh/id_rsa): /home/musa/.ssh/key-server 
```

**Memasukkan Public Key dari Linux ke Server**

Mengirim SSH key ke VPS dengan memakai perintah ssh-copy-id.

```
ssh-copy-id -i ~/.ssh/key-server root@64.20.51.164 -p 50000
```

**Mengaktifkan Login SSH dengan Key**

Buka kembali file konfigurasi SSH dan sesuaikan dengan opsi di bawah ini.

```
PubkeyAuthentication yes	
PasswordAuthentication no 
```

**Koneksi SSH client dengan file config di Linux**

Membuat file konfigurasi agar lebih mudah melakukan koneksi SSH tanpa harus mengetikkan perintah yang cukup panjang.

Membuat file config dengan nano text editor. 

```
nano ~/.ssh/config
```

Isi dengan konfigurasi koneksi SSH

```
Host server
HostName 64.20.51.164 
  IdentityFile /home/musa/.ssh/key-server
  IdentitiesOnly=yes
  Port 22
  User musa
```

Dengan konfigurasi SSH client di atas, kita cukup menjalankan perintah di bawah ini untuk terhubung ke VPS, tanpa harus memasukkan parameter SSH key, username, IP address, dan nomor port.

```
ssh server
```