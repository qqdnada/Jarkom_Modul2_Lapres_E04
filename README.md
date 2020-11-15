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
  ns1	  IN	A   10.151.71.43
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
Domain **http://semeruyyy.pw** memiliki DocumentRoot pada /var/www/semeruyyy.pw.

Script berikut digunakan agar semerue04.pw memiliki DocumentRoot pada /var/www/semeruyyy.pw.

```
DocumentRoot /var/www/web-8888
<Directory /var/www/semerue04.pw>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
 </Directory>
 ```

### Soal No. 9
Mengaktifkan mod rewrite agar urlnya menjadi **http://semeruyyy.pw/home**.

Untuk mengaktifkan mod rewrite agar url menjadi **http://semeruyyy.pw/home** dapat menggunakan script berikut pada .htaccess di PROBOLINGGO.

```
RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule ^(.*)\?*$ index.php/$1 [L,QSA]
```

Screenshot Hasil
![9](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/9-semeruhome.png)

### Soal No. 10
Web **http://penanjakan.semeruyyy.pw** akan digunakan untuk menyimpan assets file yang memiliki DocumentRoot pada /var/www/penanjakan.semeruyyy.pw dan memiliki struktur
folder sebagai berikut:

/var/www/penanjakan.semeruyyy.pw/

				public/javascripts

				public/css

				public/images

				errors

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

Halaman ketika mencoba mengakses folder public.
![11-1](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/11-1-penanjakanhome.png)

Ketika mencoba mengakses folder css, images, atau javascripts akan muncul seperti berikut.
![11-2](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/11-2-penanjakancss.png)


### Soal No. 12
Untuk mengatasi HTTP Error code 404, disediakan file 404.html pada folder /errors untuk mengganti error default 404 dari Apache.

Dapat menggunakan script di bawah ini.
![12-1](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/12-1-htaccess_semeru.png)

### Soal No. 13
Untuk mengakses file assets javascript awalnya harus menggunakan url **http://penanjakan.semeruyyy.pw/public/javascripts**. Karena terlalu panjang maka dibuatkan konfigurasi virtual host agar ketika mengakses file assets menjadi **http://penanjakan.semeruyyy.pw/js**.

Untuk membuat alias dapat ditambahkan konfigurasi berikut.

```
Alias "/js" "/var/www/penanjakan.semerue04.pw/public/javascripts"
```

Lalu restart apache dengan perintah ```service apache2 restart```.

![13](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/13-penanjakanjs.png)

### Soal No. 14
Membuat web **http://naik.gunung.semeruyyy.pw** diakses hanya dengan menggunakan port 8888.

Agar naik.gunung.semeru.e04.pw hanya dapat diakses menggunakan port 8888, maka isi pada config perlu diubah menjadi ```<VirtualHost *:8888>``` dan ```DocumentRoot /var/www/web-8888```.

Untuk hasilnya dapat dilihat pada gambar pertama di Soal Nomor 15.

### Soal No. 15
Membuat web **http://naik.gunung.semeruyyy.pw** agar diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung”.

Script untuk autentikasi seperti permintaan soal.
```
<?php
if (!isset($_SERVER['PHP_AUTH_USER'])) {
    header('WWW-Authenticate: Basic realm="My Realm"');
    header('HTTP/1.0 401 Unauthorized');
    echo 'You are not logged in.';
    exit;
} else {
	if($_SERVER['PHP_AUTH_USER'] == 'semeru' || $_SERVER['PHP_AUTH_PW'] == 'kuynaikgunung')
	{
    		echo 'This is the website.';
		destroy();
		exit;
	}
	else
	{
		header('HTTP/1.0 401 Unauthorized');
    		echo 'Wrong credentials. Please try again by reloading.';
		destroy();
    		exit;
	}
}
?>
```

Berikut hasil screenshotnya.
![15-1](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/15-1-naikauth.png)

Jika berhasil akan muncul seperti berikut.
![15-2](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/15-2-authsuccess.png)

Sedangkan jika gagal akan muncul.
![15-3](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/15-3-authwrong.png)

### Soal No. 16
Ketika mengunjungi IP PROBOLINGGO akan dialihkan secara otomatis ke **http://semeruyyy.pw**.

Untuk redirect IP PROBOLINGGO ke **http://semeruyyy.pw** dapat digunakan script berikut ini.
![16-script](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/16-1-php_redirectIP.png)

Adapun ketika mencoba mengakses IP Probolinggo maka halaman http://semerue04.pw yang muncul.
![16-hasil](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/16-2-semerupw.png)

### Soal No. 17
Semua request gambar yang memiliki substring “semeru” pada **/var/www/penanjakan.semeruyyy.pw/public/images** akan diarahkan menuju semeru.jpg.

Script berikut digunakan agar semua request gambar yang memiliki substring “semeru” pada **/var/www/penanjakan.semeruyyy.pw/public/images** diarahkan menuju semeru.jpg.
![17-script](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/17-1-htaccess_penanjakan.png)

Berikut hasil ketika kita meminta gambar yang memiliki substring "semeru".
![17-hasil](https://github.com/qqdnada/Jarkom_Modul2_Lapres_E04/blob/master/images/17-2-penanjakansemeru.png)
