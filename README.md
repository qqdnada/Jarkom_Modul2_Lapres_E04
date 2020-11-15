# Jarkom_Modul2_Lapres_E04
Repository untuk Laporan Resmi Praktikum Modul 2 Jarkom 2020 - E04

* Michael Ricky
  0511184000078
  
* Qatrunada Qori Darwati
  05111840000059


### Soal No. 1
Membuat domain **semerue04.pw** 

Karena MALANG digunakan sebagai DNS Server Master, maka pada MALANG jalankan perintah ```apt-get update``` untuk update package lists. Setelah melakukan update, install aplikasi bind9 pada MALANG dengan perintah ```apt-get install bind9 -y```.

Untuk membuat domain **semerue04.pw**, lakukan perintah ```nano etc/bind/named.conf.local``` pada MALANG lalu isi konfigurasi domain tersebut dengan sintaks di bawah ini

```
  zone “semerue04.pw” {
    type master;
    file “etc/bind/jarkom/semerue04.pw”;
  };
  
```

Kemudian pada file semerue04.pw tambahkan konfigurasi berikut

```
  @ IN  NS  semerue04.pw
  @ IN  A   10.151.71.44
```

Lalu restart bind9 dengan perintah ```service bind9 restart```.

Terakhir jangan lupa untuk mengarahkan nameserver menuju IP MALANG pada client GRESIK dan SIDOARJO dengan menambahkan konfigurasi berikut pada file resolv.conf.

```
  nameserver 10.151.71.42 #IP MALANG
```

### Soal No. 2
Membuat alias **www.semerue04.pw**

Untuk membuat nama alias dan mengarahkan domain ke domain yang lain, pada MALANG di file semerue04.pw tambahkan konfigurasi berikut

```
  www	IN  CNAME	semerue04.pw.
```

Kemudian restart bind9 dengan perintah ```service bind9 restart```.

### Soal No. 3
Membuat subdomain **penanjakan.semerue04.pw** yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO

Tambahkan konfigurasi berikut pada file semerue04.pw di MALANG.

```
  penanjakan	IN	A	10.151.71.44
```

Lalu restart bind9 dengan perintah ```service bind9 restart```.

### Soal No. 4
Membuat reverse domain untuk domain utama.

Tambahkan konfigurasi berikut pada file etc/bind/named.conf.local di MALANG.

```
  zone "71.151.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/71.151.10.in-addr.arpa";
  };
```

Lalu pada file 71.151.10.in-addr.arpa tambahkan konfigurasi di bawah ini

```
  71.151.10.in-addr.arpa. IN	NS	semerue04.pw.
  44                      IN	PTR	semerue04.pw.
```

Kemudian restart bind9 dengan perintah ```service bind9 restart```.

### Soal No. 5
Membuat DNS Server Slave pada MOJOKERTO

Pada MALANG, buka file etc/bind/named.conf.local lalu sesuaikan dengan konfigurasi berikut.

```
  zone "semerue04.pw" {
    type master;
    notify yes;
    also-notify { 10.151.71.43 };
    allow-transfer { 10.151.71.43 };
    file "/etc/bind/jarkom/semerue04.pw";
  };
```

Lalu restart bind9 dengan perintah ```service bind9 restart```.

Sedangkan pada MOJOKERTO, buka file etc/bind/named.conf.local lalu tambahkan dengan konfigurasi berikut.

```
  zone "semerue04.pw" {
    type slave;
    masters { 10.151.71.42; }; 
    file "/var/lib/bind/semerue04.pw";
  };
```

Lalu restart bind9 dengan perintah ```service bind9 restart```.

Terakhir jangan lupa untuk mengarahkan nameserver menuju IP MOJOKERTO pada client GRESIK dan SIDOARJO dengan menambahkan konfigurasi berikut pada file resolv.conf.

```
  nameserver 10.151.71.43 #IP MOJOKERTO
```

### Soal No. 6
Membuat subdomain **gunung.semerue04.pw** yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO.

Tambahkan konfigurasi berikut pada file semerue04.pw di MALANG.

```
  ns1	    IN	A   10.151.71.43
  gunung  IN	NS  ns1
```

Kemudian restart bind9 dengan perintah ```service bind9 restart```.

Berikutnya pada MOJOKERTO di file etc/bind/named.conf.local tambahkan dengan konfigurasi di bawah ini.

```
  zone "gunung.semerue04.pw" {
    type master;
    file "/etc/bind/delegasi/gunung.semerue04.pw";
    allow-transfer{ any; };
  };
```

Lalu pada file gunung.semerue04.pw tambahkan konfigurasi berikut.

```
  @	IN	NS	gunung.semerue04.pw.
  @	IN	A 	10.151.71.44
```

Kemudian restart bind9 dengan perintah ```service bind9 restart```.

### Soal No. 7
Membuat subdomain **naik.gunung.semerue04.pw** yang mengarah ke IP Server PROBOLINGGO.

Tambahkan konfigurasi berikut pada file gunung.semerue04.pw di MOJOKERTO.

```
  naik	IN	A	10.151.71.44
```

Lalu restart bind9 dengan perintah ```service bind9 restart```.

### Soal No. 8
Domain **http://semeruyyy.pw** memiliki DocumentRoot pada /var/www/semeruyyy.pw. Awalnya web dapat diakses menggunakan alamat **http://semeruyyy.pw/index.php/home**.

### Soal No. 9
Mengaktifkan mod rewrite agar urlnya menjadi **http://semeruyyy.pw/home**.

### Soal No. 10
Web **http://penanjakan.semeruyyy.pw** akan digunakan untuk menyimpan assets file yang memiliki DocumentRoot pada /var/www/penanjakan.semeruyyy.pw dan memiliki struktur
folder sebagai berikut:

/var/www/penanjakan.semeruyyy.pw/public/javascripts

/var/www/penanjakan.semeruyyy.pw/public/css

/var/www/penanjakan.semeruyyy.pw/public/images

/var/www/penanjakan.semeruyyy.pw/errors

Buat direktori di atas dengan menggunakan perintah di bawah ini

```
mkdir /var/www/penanjakan.semeruyyy.pw/public
mkdir /var/www/penanjakan.semeruyyy.pw/public/javascripts
mkdir /var/www/penanjakan.semeruyyy.pw/public/css
mkdir /var/www/penanjakan.semeruyyy.pw/public/images
mkdir /var/www/penanjakan.semeruyyy.pw/errors
```

### Soal No. 11
Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan.

Lakukan mengaktifan directory listing pada /public dengan menambahkan

```
<Directory /var/www/penanjakan.semerue04.pw/public>
     Options +Indexes
</Directory>
```

Serta matikan directory listing pada folder di dalam /public dengan menambahkan

```
<Directory /var/www/penanjakan.semerue04.pw/public/css>
     Options -Indexes
</Directory>

<Directory /var/www/penanjakan.semerue04.pw/public/images>
     Options -Indexes
</Directory>

<Directory /var/www/penanjakan.semerue04.pw/public/javascripts>
     Options -Indexes
</Directory>
```

Kemudian restart apache dengan perintah ```service apache2 restart```.

### Soal No. 12
Untuk mengatasi HTTP Error code 404, disediakan file 404.html pada folder /errors untuk mengganti error default 404 dari Apache.



### Soal No. 13
Untuk mengakses file assets javascript awalnya harus menggunakan url **http://penanjakan.semeruyyy.pw/public/javascripts**. Karena terlalu panjang maka dibuatkan konfigurasi virtual host agar ketika mengakses file assets menjadi **http://penanjakan.semeruyyy.pw/js**.

Untuk membuat alias dapat ditambahkan konfigurasi berikut.

```
Alias "/js" "/var/www/penanjakan.semerue04.pw/public/javascripts"
```

Lalu restart apache dengan perintah ```service apache2 restart```.

### Soal No. 14
Membuat web **http://naik.gunung.semeruyyy.pw** diakses hanya dengan menggunakan port 8888.

### Soal No. 15
Membuat web http://naik.gunung.semeruyyy.pw agar diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung”.

### Soal No. 16
Ketika mengunjungi IP PROBOLINGGO akan dialihkan secara otomatis ke **http://semeruyyy.pw**.


### Soal No. 17
Semua request gambar yang memiliki substring “semeru” pada **/var/www/penanjakan.semeruyyy.pw/public/images** akan diarahkan menuju semeru.jpg.
