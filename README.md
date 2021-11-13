# Jarkom-Modul-3-C03-2021

Lapres Praktikum Jarkom Modul 3
kelompok C03 :

- Junaedi Akbar 05111940000041
- Zydhan Linnar P. 05111940000118
- M. Fajri Davyza Chaniago 05111940000180

## **Konten**

- [**Cara Pengerjaan**](#cara-pengerjaan)
- [**Kendala**](#kendala)

## Cara Pengerjaan

### Nomor 1

Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server

#### Jawab:

### Nomor 2

dan Foosha sebagai DHCP Relay

#### Jawab:

### Nomor 3

Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169

#### Jawab:

### Nomor 4

Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50

#### Jawab:

### Nomor 5

Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

#### Jawab :

- Menambahkan forwarders pada `/etc/bind/named.conf.options` pada EnniesLoby

```
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```

### Nomor 6

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

#### Jawab:

Pada **Jipangu** Buka`/etc/dhcp/dhcpd.conf` <br>

- lalu pada network `192.185.1.0 ` tambahkan `default-lease-time 360;` untuk mengatur waktu minimumnya yaitu 6 menit atau 360 detik

```
subnet 192.185.1.0 netmask 255.255.255.0 {
  range 192.185.1.20 192.185.1.99;
  range 192.185.1.150 192.185.1.169;
  option routers 192.185.1.1;
  option broadcast-address 192.185.1.255;
  option domain-name-servers 192.185.2.2;
  default-lease-time 360;
}
```

- kemudian pada network `192.185.3.0 ` tambahkan `default-lease-time 720;` untuk mengatur waktu minimumnya yaitu 12 menit atau 720 detik

- dan untuk mengubah default max timenya 120 menit atau 7200 detik tambahkan `max-lease-time 7200;` pada konfigurasi global

```
ddns-update-style none;
default-lease-time 600;
max-lease-time 7200;
log-facility local7;
subnet 192.185.2.0 netmask 255.255.255.0 {}
```

### Nomor 7

Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69

#### Jawab:

- Pada **Skypie** kita perlu menambahkan `hwaddress` dari **Skypie** . Dapat dicari dengan perintah `ip a` sehingga diperoleh `f6:03:7a:6e:c1:4e`, tambahkan address tersebut ke konfigurasi IP **Skypie**

```
auto eth0
iface eth0 inet dhcp
hwaddress ether f6:03:7a:6e:c1:4e
```

- Pada **Jipangu** sebagai dhcp server kita perlu menambahkan host skypienya

```
echo 'Host Skypie {
  hardware ethernet f6:03:7a:6e:c1:4e;
  fixed-address 192.185.3.69;
}' >> /etc/dhcp/dhcpd.conf
```

### Nomor 8

Loguetown digunakan sebagai client Proxy agar transaksi jual beli dapat terjamin keamanannya, juga untuk mencegah kebocoran data transaksi. Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000

#### Jawab:

### Nomor 9

Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy

#### Jawab:

### Nomor 10

Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00)

#### Jawab:

### Nomor 11

Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie

#### Jawab:

### Nomor 12

Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps

#### Jawab:

### Nomor 13

Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya

#### Jawab:

## Kendala
