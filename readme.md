# Panduan Ubuntu Server 20.04 LTS

#### Halo teman-teman superuser

Panduan Ubuntu Server 20.04 LTS adalah panduan bagaimana cara menggunakan Ubuntu sebagai sistem operasi server untuk membangun sebuah web server. Aplikasi server yang dibahas yaitu Apache dan Nginx web server, MariaDB database, PHP, dan SSL dengan Let's Encrypt. Selain itu, dibahas juga bagaimana cara install WordPress, sekaligus menjadi bahan pengujian untuk menjalankan website berbasis PHP dan MariaDB database. Panduan ini menggunakan Virtual Private Server (VPS) sehingga tidak ada pembahasan tentang bagaimana cara instalasi Ubuntu, karena VPS sudah terpasang sistem operasi.

## Keterampilan dan Pengetahuan Pendukung
Hal-hal yang dapat mendukung dalam memahami langkah-langkah konfigurasi server dalam panduan ini:

- Pengetahuan jaringan komputer
- Paham struktur direktori di Linux
- Paham atribut file dan hak akses di Linux
- Dapat menggunakan perintah dasar Linux
- Dapat menggunakan text editor berbasis CLI (Command Line Interface)

## Perangkat yang Dibutuhkan
Sebelum mempraktikkan panduan ini, siapkan terlebih dahulu perangkat yang dibutuhkan, yaitu:

1. VPS dengan sistem operasi Ubuntu 20.04 LTS
1. Nama domain internet, memiliki akses DNS Manager
1. Aplikasi SSH client
   * Windows: [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) atau [Xshell](https://www.netsarang.com/en/free-for-home-school/) 
   * Linux/macOS: OpenSSH client (tersedia default berbasis command line)
1. Aplikasi SFTP client
   * Windows: [Xftp](https://www.netsarang.com/en/free-for-home-school/) atau [FileZilla FTP client](https://filezilla-project.org/)
   * Linux/macOS: [FileZilla FTP client](https://filezilla-project.org/)
1. Web Browser: Mozilla Firefox atau Google Chrome
1. Code Editor: [Sublime Text](https://www.sublimetext.com/)
1. Jaringan internet yang stabil

## Penulis

- Nama Penulis: Musa Amin
- Blog: [musaamin.web.id](https://musaamin.web.id)
- YouTube: [youtube.com/musaamin](https://www.youtube.com/musaamin)
- Twitter: [@musaamin](https://www.twitter.com/musaamin)
- Facebook: [facebook.com/musaaminwebid](https://www.facebook.com/musaaminwebid)
- GitHub: [musaamin](https://github.com/musaamin)

## Penulis

- Nama Penulis: Musa Amin
- Blog: [musaamin.web.id/blog](https://musaamin.web.id/blog)
- YouTube: [youtube.com/musaamin](https://www.youtube.com/musaamin)
- Twitter: [@musaamin](https://www.twitter.com/musaamin)
- Facebook: [facebook.com/musaaminwebid](https://www.facebook.com/musaaminwebid)
- GitHub: [musaamin](https://github.com/musaamin)

## Donasi

Dukung penulisan ebook melalui donasi di [Saweria https://saweria.co/musaamin](https://saweria.co/musaamin)
