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

* Setup DNS
```
apt-get update
apt-get install bind9 -y
service bind9 start
```
* Setup DHCP Server
```
apt-get update
apt-get install isc-dhcp-server -y
```
* DHCP Server interface pada ```/etc/default/isc-dhcp-server```
```
INTERFACES="eth0"
service isc-dhcp-server start
```
* Setup Proxy Server
```
apt-get update
apt-get install squid -y
service squid start
```
### Nomor 2

dan Foosha sebagai DHCP Relay

#### Jawab:

Foosha sebagai DHCP Relay
* Setup DHCP Realy
```
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start
```
* Pasang DHCP Relay ```/etc/default/isc-dhcp-relay```
```
SERVERS="192.168.2.4"
INTERFACES="eth1 eth2 eth3"
OPTIONS=
```
### Nomor 3

Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169

#### Jawab:

* Konfigurasi IP Range DHCP untuk switch 1
```
subnet  192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.20  192.168.1.99;
    range  192.168.1.150 1 192.168.1.169;
    option routers  192.168.1.1;
    option broadcast-address  192.168.1.255;
}
```
### Nomor 4

Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50

#### Jawab:

* Konfigurasi IP Range DHCP untuk switch 3
```
subnet 192.168.3.0 netmask 255.255.255.0 {
    range  192.168.3.30  192.168.3.50;
    option routers  192.168.3.1;
    option broadcast-address  192.168.3.255;
}
```

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

Pada EniesLobby, buat DNS zone baru pada `/etc/bind/named.conf.local` yang berisi:

```conf
zone "jualbelikapal.c03.com" {
  type master;
  file "/etc/bind/jarkom/jualbelikapal.c03.com";
};
```

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

Pada Water7, ganti isi `/etc/squid/squid.conf` dengan:

```conf
http_port 5000
visible_hostname jualbelikapal.c03.com
http_access allow all
```

Lalu restart squid

```bash
service squid restart
```

### Nomor 9

Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy

#### Jawab:

Pada Water7, eksekusi perintah dibawah untuk membuat password

```bash
htpasswd -c -m -b /etc/squid/passwd luffybelikapalc03 luffy_c03
htpasswd -m -b /etc/squid/passwd zorobelikapalc03 zoro_c03
```

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

### Nomor 10

Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00)

#### Jawab:

Pada Water7, buat file `/etc/squid/acl.conf` dan isikan potongan teks berikut:

```text
acl time1 time MTWH 07:00-11:00
acl time2 time TWHF 17:00-24:00
acl time3 time WHFA 00:00-03:00
```

Berikutnya, tambahkan `include /etc/squid/acl.conf` pada baris pertama `/etc/squid/squid.conf` dan ganti `http_access allow USERS` dengan:

```text
http_access allow time1 USERS
http_access allow time2 USERS
http_access allow time3 USERS
```

![Akses diblokir](https://media.discordapp.net/attachments/769183322147389460/909068607068192788/unknown.png)

### Nomor 11

Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie

#### Jawab:

Pada Water7, tambahkan potongan konfigurasi berikut pada `/etc/squid/acl.conf`:

```text
acl lan src 192.185.0.0/16
acl badsites dstdomain .google.com
```

Berikutnya tambahkan potongan berikut setelah `http_port 5000` pada `/etc/squid/squid.conf`

```text
deny_info http://super.franky.c03.com lan
http_reply_access deny badsites lan
```

**Note:** Deployment mengikuti modul sebelumnya sehingga tidak perlu dijelaskan mendetail

### Nomor 12

Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps

#### Jawab:

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

### Nomor 13

Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya

#### Jawab:

Tidak perlu menambahkan apapun dimanapun.

![Pengunduhan sangat cepat](https://media.discordapp.net/attachments/769183322147389460/909068426352402442/unknown.png)

## Kendala
