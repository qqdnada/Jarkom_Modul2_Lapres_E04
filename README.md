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
  nameserver 10.151.71.42		#IP MALANG
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
  nameserver 10.151.71.43		#IP MOJOKERTO
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
