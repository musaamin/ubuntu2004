---
title: "File Transfer"
date: 2020-12-15T20:56:22+07:00
draft: false
weight: 3
pre: "3. "
---

Selain digunakan untuk melakukan remote shell ke server, protokol SSH juga dapat digunakan untuk melakukan file transfer. File transfer melalui protokol SSH ada 2 yaitu Secure File Transfer Protocol (SFTP) dan Secure Copy (SCP).

## SFTP

**SFTP dengan menggunakan aplikasi Xftp di Windows**

1. Buka aplikasi Xshell, lalu login SSH
1. Setelah berhasil login, klik tombol Xftp, maka akan membuka aplikasi Xftp dan langsung login SFTP secara otomatis tanpa harus membuat Session baru

**SFTP dengan menggunakan aplikasi FileZilla di Linux**

Jika authentication SSH memakai key, konversi terlebih dahulu SSH Private Key karena FileZilla hanya mengenal private key format .ppk dan .pem.

```
openssl rsa -in key-server -outform pem > key-server.pem
```

Membuat Koneksi SFTP di FileZilla

1. File → Site Manager (CTRL+S)
1. New Site
1. Host = IP address, Protocol = SFTP, Logon Type = Normal (password), Key File (SSH Key), User = user account di server, Key File = Key format .PEM
1. Connect 

## SCP

**SCP dengan menggunakan scp di Linux**

Jika telah membuat file config untuk SSH client, file transfer dengan SCP bisa menjadi lebih mudah.

File transfer namafile.tar.gz dari lokal ke Host server dan disimpan ke dalam folder /home/user.

```
scp namafile.tar.gz server:~/
```

Jika yang ditransfer adalah folder.

```
scp -rp server:~/
```

Transfer file dari server ke lokal dan disimpan ke dalam folder /home/user.

```
scp server:~/namafile.tar.gz ~/
```

