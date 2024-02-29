---
title: "Composer"
date: 2020-12-15T22:13:17+07:00
draft: false
weight: 8
pre: "8. "
---

Composer adalah package manager untuk PHP. Fungsinya mirip dengan package manager di distro Linux. Dengan Composer, jika membutuhkan script tertentu, dependensi, dan update cukup menggunakan Composer. Composer akan mengunduh script yang dibutuhkan. Beberapa PHP framework membutuhkan Composer untuk instalasi. 

## Install Composer

Install unzip dan curl.

```
sudo apt install unzip curl -y
```

Download composer dan pindahkan filenya.

```
curl https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

Menguji hasil instalasi composer dengan menampilkan nomor versinya.

```
composer -V
```

Hasil dari perintah di atas.

```
Composer version 2.0.8 2020-12-03 17:20:38
```