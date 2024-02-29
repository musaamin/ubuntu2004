---
title: "phpMyAdmin dengan Nginx"
date: 2020-12-16T09:30:48+07:00
draft: false
weight: 14
pre: ""
---

phpMyAdmin adalah aplikasi manajemen database MySQL/MariaDB. 

## Install phpMyAdmin

Download phpMyAdmin terbaru dari phpmyadmin.net.

```
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.4/phpMyAdmin-5.0.4-english.tar.gz
```

Extract filenya phpMyAdmin\*.tar.gz

```
tar xzvf phpMyAdmin-5.0.4-english.tar.gz
```

Pindahkan folder hasil extract dan buat symbolic link ke /var/www/html.

```
sudo mv phpMyAdmin-5.0.4-english /usr/share/phpmyadmin
sudo ln -s /usr/share/phpmyadmin /var/www/html
```

Konfigurasi server block default agar bisa menjalankan script PHP saat mengakses IP address di web browser.

```
sudo nano /etc/nginx/sites-available/default
```

Ubah opsi **index** dan tambahkan konfigurasi.

```
index index.php index.html;

location ~ \.php$ {
  try_files $fastcgi_script_name =404;
  include fastcgi_params;
  fastcgi_pass  unix:/run/php/php7.4-fpm.sock;
  fastcgi_index index.php;
  fastcgi_param DOCUMENT_ROOT  $realpath_root;
  fastcgi_param SCRIPT_FILENAME   $realpath_root$fastcgi_script_name; 
}
```

Cek konfigurasi, restart, dan cek status Nginx service.

```
sudo nginx -t
sudo systemctl restart nginx
sudo systemctl status nginx
```

Browse http://serverIP/phpmyadmin.