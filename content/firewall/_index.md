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
sudo su
apt install iptables-persistent
```

Menampilkan rules iptables.

```
iptables -L
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
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -p icmp -j ACCEPT
iptables -A INPUT -p tcp --dport 50000 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
iptables -P INPUT DROP
```

Rules di atas hanya mengijinkan trafik pada protokol ICMP, port 80 (HTTP), port 443 (HTTPS), dan port  50000 (Custom port SSH). Selain yang tertulis dalam rules semuanya diblokir.

Menampilkan kembali rules untuk melihat rules yang telah dimasukkan.

```
iptables -L

Chain INPUT (policy DROP)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     icmp --  anywhere             anywhere            
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:50000 ctstate NEW,ESTABLISHED
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http ctstate NEW,ESTABLISHED
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:https ctstate NEW,ESTABLISHED

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

Save dan Reload rules iptables.

```
netfilter-persistent save
netfilter-persistent reload
```

Restart server dan cek kembali rules iptables, apakah rules yang dimasukkan sebelumnya masih ada.

```
reboot
sudo su
iptables -L
```

Jika ingin menghapus semua rules, gunakan opsi -F, tapi sebelumnya ubah kembali policy INPUT menjadi ACCEPT karena kalau masih DROP, otomatis koneksi SSH akan terputus ketika semua rules telah dihapus.

```
iptables -P INPUT ACCEPT
iptables -F
netfilter-persistent save
netfilter-persistent reload
```