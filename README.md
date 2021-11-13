# Laporan Resmi Praktikum Jarkom Modul 3

![Lambang ITS](https://www.its.ac.id/wp-content/uploads/2020/07/Lambang-ITS-2-320x320.png)

Kelompok C03 :

- Junaedi Akbar 05111940000041
- Zydhan Linnar P. 05111940000118
- M. Fajri Davyza Chaniago 05111940000180

## Daftar Isi

- [Laporan Resmi Praktikum Jarkom Modul 3](#laporan-resmi-praktikum-jarkom-modul-3)
  - [Daftar Isi](#daftar-isi)
  - [Soal Nomor 1](#soal-nomor-1)
  - [Jawaban Nomor 1](#jawaban-nomor-1)
    - [Setup DNS](#setup-dns)
    - [Setup DHCP Server](#setup-dhcp-server)
    - [DHCP Server interface pada ```/etc/default/isc-dhcp-server```](#dhcp-server-interface-pada-etcdefaultisc-dhcp-server)
    - [Setup Proxy Server](#setup-proxy-server)
  - [Soal Nomor 2](#soal-nomor-2)
  - [Jawaban Nomor 2](#jawaban-nomor-2)
    - [Setup DHCP Relay](#setup-dhcp-relay)
    - [Pasang DHCP Relay ```/etc/default/isc-dhcp-relay```](#pasang-dhcp-relay-etcdefaultisc-dhcp-relay)
  - [Soal Nomor 3](#soal-nomor-3)
  - [Jawaban Nomor 3](#jawaban-nomor-3)
    - [Konfigurasi IP Range DHCP untuk switch 1](#konfigurasi-ip-range-dhcp-untuk-switch-1)
  - [Soal Nomor 4](#soal-nomor-4)
  - [Jawaban Nomor 4](#jawaban-nomor-4)
    - [Konfigurasi IP Range DHCP untuk switch 3](#konfigurasi-ip-range-dhcp-untuk-switch-3)
  - [Soal Nomor 5](#soal-nomor-5)
  - [Jawaban Nomor 5](#jawaban-nomor-5)
    - [Menambahkan forwarders pada `/etc/bind/named.conf.options` pada EnniesLoby](#menambahkan-forwarders-pada-etcbindnamedconfoptions-pada-enniesloby)
  - [Soal Nomor 6](#soal-nomor-6)
  - [Jawaban Nomor 6](#jawaban-nomor-6)
    - [Lalu pada network `192.185.1.0` tambahkan `default-lease-time 360;` untuk mengatur waktu minimumnya yaitu 6 menit atau 360 detik](#lalu-pada-network-19218510-tambahkan-default-lease-time-360-untuk-mengatur-waktu-minimumnya-yaitu-6-menit-atau-360-detik)
    - [Default Lease Time](#default-lease-time)
    - [Maximum Lease Time](#maximum-lease-time)
  - [Soal Nomor 7](#soal-nomor-7)
  - [Jawaban Nomor 7](#jawaban-nomor-7)
    - [Konfigurasi MAC Skypie](#konfigurasi-mac-skypie)
    - [Konfigurasi Fix IP pada Jipangu](#konfigurasi-fix-ip-pada-jipangu)
  - [Soal Nomor 8](#soal-nomor-8)
  - [Jawaban Nomor 8](#jawaban-nomor-8)
    - [Pembuatan DNS Zone](#pembuatan-dns-zone)
    - [Konfigurasi A Record](#konfigurasi-a-record)
    - [Konfigurasi Hostname Squid](#konfigurasi-hostname-squid)
  - [Soal Nomor 9](#soal-nomor-9)
  - [Jawaban Nomor 9](#jawaban-nomor-9)
    - [Pembuatan Kata Sandi](#pembuatan-kata-sandi)
    - [Konfigurasi kata sandi pada squid.conf](#konfigurasi-kata-sandi-pada-squidconf)
  - [Soal Nomor 10](#soal-nomor-10)
  - [Jawaban Nomor 10](#jawaban-nomor-10)
    - [Penambahan ACL Waktu](#penambahan-acl-waktu)
    - [Konfigurasi Akses Berdasarkan ACL Waktu](#konfigurasi-akses-berdasarkan-acl-waktu)
  - [Soal Nomor 11](#soal-nomor-11)
  - [Jawaban Nomor 11](#jawaban-nomor-11)
    - [Pembuatan ACL google.com](#pembuatan-acl-googlecom)
    - [Konfigurasi Menolak ACL dan Mengarahkan ke `http://super.franky.c03.com`](#konfigurasi-menolak-acl-dan-mengarahkan-ke-httpsuperfrankyc03com)
  - [Soal Nomor 12](#soal-nomor-12)
  - [Jawaban Nomor 12](#jawaban-nomor-12)
    - [Konfigurasi Limit Kecepatan](#konfigurasi-limit-kecepatan)
  - [Soal Nomor 13](#soal-nomor-13)
  - [Jawaban Nomor 13](#jawaban-nomor-13)
    - [Solusi Singkat](#solusi-singkat)
  - [Kendala](#kendala)

## Soal Nomor 1

Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server

## Jawaban Nomor 1

### Setup DNS

```bash
apt-get update
apt-get install bind9 -y
service bind9 start
```

### Setup DHCP Server

```bash
apt-get update
apt-get install isc-dhcp-server -y
```

### DHCP Server interface pada ```/etc/default/isc-dhcp-server```

```bash
INTERFACES="eth0"
service isc-dhcp-server start
```

### Setup Proxy Server

```bash
apt-get update
apt-get install squid -y
service squid start
```

## Soal Nomor 2

dan Foosha sebagai DHCP Relay

## Jawaban Nomor 2

Foosha sebagai DHCP Relay

### Setup DHCP Relay

```bash
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start
```

### Pasang DHCP Relay ```/etc/default/isc-dhcp-relay```

```text
SERVERS="192.168.2.4"
INTERFACES="eth1 eth2 eth3"
OPTIONS=
```

## Soal Nomor 3

Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169

## Jawaban Nomor 3

### Konfigurasi IP Range DHCP untuk switch 1

```text
subnet  192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.20  192.168.1.99;
    range  192.168.1.150 1 192.168.1.169;
    option routers  192.168.1.1;
    option broadcast-address  192.168.1.255;
}
```

## Soal Nomor 4

Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50

## Jawaban Nomor 4

### Konfigurasi IP Range DHCP untuk switch 3

```text
subnet 192.168.3.0 netmask 255.255.255.0 {
    range  192.168.3.30  192.168.3.50;
    option routers  192.168.3.1;
    option broadcast-address  192.168.3.255;
}
```

## Soal Nomor 5

Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

## Jawaban Nomor 5

### Menambahkan forwarders pada `/etc/bind/named.conf.options` pada EnniesLoby

```text
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

## Soal Nomor 6

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

## Jawaban Nomor 6

Pada **Jipangu** Buka`/etc/dhcp/dhcpd.conf`

### Lalu pada network `192.185.1.0` tambahkan `default-lease-time 360;` untuk mengatur waktu minimumnya yaitu 6 menit atau 360 detik

```text
subnet 192.185.1.0 netmask 255.255.255.0 {
  range 192.185.1.20 192.185.1.99;
  range 192.185.1.150 192.185.1.169;
  option routers 192.185.1.1;
  option broadcast-address 192.185.1.255;
  option domain-name-servers 192.185.2.2;
  default-lease-time 360;
}
```

### Default Lease Time

Kemudian pada network `192.185.3.0` tambahkan `default-lease-time 720;` untuk mengatur waktu minimumnya yaitu 12 menit atau 720 detik

### Maximum Lease Time

Untuk mengubah default max timenya 120 menit atau 7200 detik tambahkan `max-lease-time 7200;` pada konfigurasi global

```text
ddns-update-style none;
default-lease-time 600;
max-lease-time 7200;
log-facility local7;
subnet 192.185.2.0 netmask 255.255.255.0 {}
```

## Soal Nomor 7

Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69

## Jawaban Nomor 7

### Konfigurasi MAC Skypie

Pada **Skypie** kita perlu menambahkan `hwaddress` dari **Skypie** . Dapat dicari dengan perintah `ip a` sehingga diperoleh `f6:03:7a:6e:c1:4e`, tambahkan address tersebut ke konfigurasi IP **Skypie**

```text
auto eth0
iface eth0 inet dhcp
hwaddress ether f6:03:7a:6e:c1:4e
```

### Konfigurasi Fix IP pada Jipangu

Pada **Jipangu** sebagai DHCP server kita perlu menambahkan host skypienya

```bash
echo 'Host Skypie {
  hardware ethernet f6:03:7a:6e:c1:4e;
  fixed-address 192.185.3.69;
}' >> /etc/dhcp/dhcpd.conf
```

## Soal Nomor 8

Loguetown digunakan sebagai client Proxy agar transaksi jual beli dapat terjamin keamanannya, juga untuk mencegah kebocoran data transaksi. Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000

## Jawaban Nomor 8

### Pembuatan DNS Zone

Pada EniesLobby, buat DNS zone baru pada `/etc/bind/named.conf.local` yang berisi:

```conf
zone "jualbelikapal.c03.com" {
  type master;
  file "/etc/bind/jarkom/jualbelikapal.c03.com";
};
```

### Konfigurasi A Record

Setelah itu buat file `/etc/bind/jarkom/jualbelikapal.c03.com` yang berisi:

```text
$TTL 604800
@ IN SOA jualbelikapal.c03.com. root.jualbelikapal.c03.com. (
                20211108 ; Serial
                604800 ; Refresh
                86400 ; Retry
                2419200 ; Expire
                604800 ) ; Negative Cache TTL
;
@ IN NS jualbelikapal.c03.com.
@ IN A 192.185.2.3
@ IN AAAA ::1
www IN CNAME jualbelikapal.c03.com.
```

### Konfigurasi Hostname Squid

Pada Water7, ganti isi `/etc/squid/squid.conf` dengan:

```conf
http_port 5000
visible_hostname jualbelikapal.c03.com
http_access allow all
```

Restart squid

```bash
service squid restart
```

## Soal Nomor 9

Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy

## Jawaban Nomor 9

### Pembuatan Kata Sandi

Pada Water7, eksekusi perintah dibawah untuk membuat password

```bash
htpasswd -c -m -b /etc/squid/passwd luffybelikapalc03 luffy_c03
htpasswd -m -b /etc/squid/passwd zorobelikapalc03 zoro_c03
```

### Konfigurasi kata sandi pada squid.conf
  
Berikutnya, hapus `http_access allow all` pada `/etc/squid/squid.conf` dan tambahkan potongan konfigurasi berikut:

```text
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS

http_access deny all
```

## Soal Nomor 10

Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00)

## Jawaban Nomor 10

### Penambahan ACL Waktu

Pada Water7, buat file `/etc/squid/acl.conf` dan isikan potongan teks berikut:

```text
acl time1 time MTWH 07:00-11:00
acl time2 time TWHF 17:00-24:00
acl time3 time WHFA 00:00-03:00
```

### Konfigurasi Akses Berdasarkan ACL Waktu

Berikutnya, tambahkan `include /etc/squid/acl.conf` pada baris pertama `/etc/squid/squid.conf` dan ganti `http_access allow USERS` dengan:

```text
http_access allow time1 USERS
http_access allow time2 USERS
http_access allow time3 USERS
```

![Akses diblokir](https://media.discordapp.net/attachments/769183322147389460/909068607068192788/unknown.png)

## Soal Nomor 11

Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie

## Jawaban Nomor 11

### Pembuatan ACL google.com

Pada Water7, tambahkan potongan konfigurasi berikut pada `/etc/squid/acl.conf`:

```text
acl lan src 192.185.0.0/16
acl badsites dstdomain .google.com
```

### Konfigurasi Menolak ACL dan Mengarahkan ke `http://super.franky.c03.com`

Berikutnya tambahkan potongan berikut setelah `http_port 5000` pada `/etc/squid/squid.conf`

```text
deny_info http://super.franky.c03.com lan
http_reply_access deny badsites lan
```

**Note:** Deployment mengikuti modul sebelumnya sehingga tidak perlu dijelaskan mendetail

## Soal Nomor 12

Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps

## Jawaban Nomor 12

### Konfigurasi Limit Kecepatan

Pada Water7, tambahkan potongan konfigurasi berikut pada `/etc/squid/squid.conf` sebelum `http_access deny all`:

```text
acl multimedia url_regex -i \.png$ \.jpg$
acl bar proxy_auth luffybelikapalc03

delay_pools 1
delay_class 1 1
delay_parameters 1 1250/1250
delay_access 1 allow bar multimedia
delay_access 1 deny all
```

![Kecepatan dibatasi 10 kbps / 1.25 KBps](https://media.discordapp.net/attachments/769183322147389460/909068106364755988/unknown.png)

## Soal Nomor 13

Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya

## Jawaban Nomor 13

### Solusi Singkat

Tidak perlu menambahkan apapun dimanapun.

![Pengunduhan sangat cepat](https://media.discordapp.net/attachments/769183322147389460/909068426352402442/unknown.png)

## Kendala

- Cukup susah dalam koordinasi mengingat project GNS kurang ramah untuk kolaborasi
