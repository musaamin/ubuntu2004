---
title: "Firewall"
date: 2020-12-16T10:03:17+07:00
draft: false
weight: 17
pre: "9. "
---

Firewall adalah sistem keamanan yang berfungsi untuk mengelola dan memantau trafik jaringan yang masuk dan keluar berdasarkan aturan keamanan (security rules) yang sudah ditentukan. Firewall dapat berupa perangkat keras atau perangkat lunak.

![Gambar Topologi Firewall](https://raw.githubusercontent.com/musaamin/ubuntu2004/main/static/images/topologi-firewall.jpg)

## Cloud Firewall

Beberapa penyedia cloud server memiliki fitur cloud firewall yang dapat dikonfigurasi melalui dashboard atau client area penyedia cloud. Cloud firewall tersebut merupakan sistem yang terpisah dari server, sehingga tidak perlu melakukan konfigurasi secara langsung di server.

Cloud firewall secara default mengijinkan semua trafik jaringan yang berasal dari server menuju keluar jaringan (Output/Outbound/Egress).

![Gambar Firewall Rule Outbund](https://raw.githubusercontent.com/musaamin/ubuntu2004/main/static/images/cloud-firewall-outbound.jpg)

Sedangkan untuk trafik jaringan dari luar menuju ke dalam server (Input/Inbound/Ingress) secara default diblokir semua, kecuali rule yang dituliskan.

![Gambar Firewall Rule Outbund](https://raw.githubusercontent.com/musaamin/ubuntu2004/main/static/images/cloud-firewall-inbound.jpg)

## IPTables

iptables adalah firewall berupa perangkat lunak yang secara default sudah tersedia pada sistem operasi berbasis Linux.

Install iptables-persistent, agar rules yang sudah dimasukkan ke dalam iptables tetap tersimpan meskipun server sudah direstart.

```
sudo apt install iptables-persistent
```

Menampilkan rules iptables.

```
sudo iptables -L
```

Hasilnya jika belum ada rules.

```
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination 
```

Memasukkan rules iptables.

```
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -p icmp -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 50000 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
```

Rules di atas hanya mengijinkan trafik pada protokol ICMP, port 80 (HTTP), port 443 (HTTPS), dan port  50000 (Custom port SSH). Selain yang tertulis dalam rules semuanya diblokir.

Menampilkan kembali rules untuk melihat rules yang telah dimasukkan.

```
sudo iptables -L

Chain INPUT (policy DROP)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     icmp --  anywhere             anywhere            
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:50000
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:https

Chain FORWARD (policy DROP)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

Save dan Reload rules iptables.

```
sudo netfilter-persistent save
sudo netfilter-persistent reload
```

Restart server.

```
sudo reboot
```

Cek kembali rules iptables, apakah rules yang dimasukkan sebelumnya masih ada.

```
sudo iptables -L
```

Jika ingin menghapus semua rules, gunakan opsi -F, tapi sebelumnya ubah kembali policy INPUT menjadi ACCEPT karena kalau masih DROP koneksi SSH akan terputus ketika semua rules telah terhapus.

```
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -F
sudo netfilter-persistent save
sudo netfilter-persistent reload
```

## UFW

UFW (Uncomplicated Firewall) adalah sebuah aplikasi yang merupakan interface dari IPTables dengan tujuan untuk memudahkan dalam konfigurasi IPTables.

Install UFW.

```
sudo apt install ufw
```

Memeriksa status UFW apakah aktif atau tidak.

```
sudo ufw status
```

Mengijinkan port ssh, http, https, dan custom port 50000/tcp.

```
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw allow 50000/tcp
sudo ufw allow 12345/tcp
```

Mengubah default policy incoming menjadi deny

```
sudo ufw default deny incoming 
```

Mengaktifkan UFW dan menampilkan rule-nya.

```
sudo ufw enable
sudo ufw status
sudo ufw status verbose
sudo ufw status numbered
```

Hasil **ufw status numbered**

```
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere                  
[ 2] 80/tcp                     ALLOW IN    Anywhere                  
[ 3] 443/tcp                    ALLOW IN    Anywhere                  
[ 4] 50000/tcp                  ALLOW IN    Anywhere                  
[ 5] 12345/tcp                  ALLOW IN    Anywhere                  
[ 6] 22/tcp (v6)                ALLOW IN    Anywhere (v6)             
[ 7] 80/tcp (v6)                ALLOW IN    Anywhere (v6)             
[ 8] 443/tcp (v6)               ALLOW IN    Anywhere (v6)             
[ 9] 50000/tcp (v6)             ALLOW IN    Anywhere (v6)             
[10] 12345/tcp (v6)             ALLOW IN    Anywhere (v6)
```

Menghapus salah satu rule berdasarkan nomor urut.

```
sudo ufw delete 5
```

Menghapus berdasarkan rule, akan menghapus rule v4 dan v6.

```
sudo ufw delete allow 12345/tcp
```

Jika ingin menghapus semua rule, mengembalikan default policy incoming, dan menonaktifkan UFW.

```
sudo ufw default allow incoming
sudo ufw reset
sudo ufw disable
sudo ufw status
```