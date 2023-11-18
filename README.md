# Jarkom-Modul-3-E07-2023
Laporan Resmi Praktikum Modul 3 Jaringan Komputer 2023

### Kelompok E07
| Nama | NRP |
|:----:|:---:|
| [Akmal Sulthon Fathulloh](https://github.com/afsulthon) | 5025211047 |
| [Fadilla Rizky Nurhidayah](https://github.com/fadillaarn) | 5025211110 |

## Topologi Jaringan

![Topologi Jaringan](https://github.com/fadillaarn/Jarkom-Modul-3-E07-2023/assets/107914177/37324315-5d8f-4962-b31b-29debdcd39cf)

## Konfigurasi Jaringan

Aura (Router/DHCP Relay)
```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
    address 10.40.1.10
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.40.2.10
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.40.3.10
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.40.4.10
	netmask 255.255.255.0
```

Himmel (DHCP Server)
```bash
auto eth0
iface eth0 inet static
	address 10.40.1.1
	netmask 255.255.255.0
	gateway 10.40.1.10
```

Heiter (DNS Server)
```bash
auto eth0
iface eth0 inet static
	address 10.40.1.2
	netmask 255.255.255.0
	gateway 10.40.1.10
```

Denken (Database Server)
```bash
auto eth0
iface eth0 inet static
	address 10.40.2.1
	netmask 255.255.255.0
	gateway 10.40.2.10
```

Eisen (Load Balancer)
```bash
auto eth0
iface eth0 inet static
	address 10.40.2.2
	netmask 255.255.255.0
	gateway 10.40.2.10
```

Lugner (PHP Worker 1)
```bash
auto eth0
iface eth0 inet static
	address 10.40.3.1
	netmask 255.255.255.0
	gateway 10.40.3.10
```

Linie (PHP Worker 2)
```bash
auto eth0
iface eth0 inet static
	address 10.40.3.2
	netmask 255.255.255.0
	gateway 10.40.3.10
```

Lawine (PHP Worker 3)
```bash
auto eth0
iface eth0 inet static
	address 10.40.3.3
	netmask 255.255.255.0
	gateway 10.40.3.10
```

Frieren (Laravel Worker 1)
```bash
auto eth0
iface eth0 inet static
	address 10.40.4.1
	netmask 255.255.255.0
	gateway 10.40.4.10
```

Flamme (Laravel Worker 2)
```bash
auto eth0
iface eth0 inet static
	address 10.40.4.2
	netmask 255.255.255.0
	gateway 10.40.4.10
```

Fern (Laravel Worker 3)
```bash
auto eth0
iface eth0 inet static
	address 10.40.4.3
	netmask 255.255.255.0
	gateway 10.40.4.10
```

Richter, Revolte, Sein, Stark (Client)
```bash
auto eth0
iface eth0 inet dhcp
```

## Soal 0

> Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa **riegel.canyon.yyy.com** untuk worker Laravel dan **granz.channel.yyy.com** untuk worker PHP mengarah pada worker yang memiliki IP [prefix IP].x.1.

Setelah mengatur topologi dan konfigurasi di tiap node sedemikian rupa, selanjutnya kita akan menambahkan register domain berupa riegel.canyon.a09.com untuk worker Laravel dan granz.channel.a09.com untuk worker PHP yang mengarah pada worker dengan IP 192.173.x.1. Pada DNS Server Heiter, kita perlu menginstall terlebih dahulu Bind9 untuk meng-setup DNS-nya, lalu jalankan script di bawah ini.

```bash
echo 'zone "riegel.canyon.e07.com" {
    type master;
    file "/etc/bind/jarkom/riegel.canyon.e07.com";
};
 
zone "granz.channel.e07.com" {
    type master;
    file "/etc/bind/jarkom/granz.channel.e07.com";
};' > /etc/bind/named.conf.local
 
mkdir /etc/bind/jarkom
 
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.e07.com. root.riegel.canyon.e07.com. (
                        2023111301      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@               IN      NS      riegel.canyon.e07.com.
@               IN      A       10.40.4.1' > /etc/bind/jarkom/riegel.canyon.e07.com
 
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.e07.com.  root.granz.channel.e07.com.  (
                        2023111301      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@               IN      NS      granz.channel.e07.com.
@               IN      A       10.40.3.1' > /etc/bind/jarkom/granz.channel.e07.com
 
echo 'options {
    directory "/var/cache/bind";
 
    forwarders {
        192.168.122.1;
    };
 
    // dnssec-validation auto;
    allow-query { any; };
    auth-nxdomain no;   # conform to RFC1035
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
 
service bind9 restart
```

Lakukan pengetesan apakah domain sudah diarahkan pada alamat IP yang tepat di sembarang node yang sudah terkoneksi dengan internet menggunakan `dnsutils`.  
![image](https://github.com/fadillaarn/Jarkom-Modul-3-E07-2023/assets/107914177/b5127480-7d1c-4621-bbd3-71d911e123a8)

## Soal 1

> Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

Pastikan topologi dan konfigurasi jaringan tiap node sudah sesuai dengan pengaturan yang di atas.

## Soal 2 - 5

> Kemudian, karena masih banyak spell yang harus dikumpulkan, bantulah para petualang untuk memenuhi kriteria berikut.  
> 1. Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
> 2. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)
> 3. Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)
> 4. Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)
> 5. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)

Setelah semua konfigurasi sesuai, jalankan script berikut pada DHCP server. Pastikan juga bahwa di DHCP server sudah terinstall `isc-dhcp-server`.

```bash
echo 'INTERFACES="eth0"' > /etc/default/isc-dhcp-server

echo 'subnet 10.40.1.0 netmask 255.255.255.0 {
}

subnet 10.40.2.0 netmask 255.255.255.0 {
}

subnet 10.40.3.0 netmask 255.255.255.0 {
    range 10.40.3.16 10.40.3.32;
    range 10.40.3.64 10.40.3.80;
    option routers 10.40.3.10;
    option broadcast-address 10.40.3.255;
    option domain-name-servers 10.40.1.2;
    default-lease-time 180;
    # min-lease-time 180;
    max-lease-time 5760;
}

subnet 10.40.4.0 netmask 255.255.255.0 {
    range 10.40.4.12 10.40.4.20;
    range 10.40.4.160 10.40.4.168;
    option routers 10.40.4.10;
    option broadcast-address 10.40.4.255;
    option domain-name-servers 10.40.1.2;
    default-lease-time 720;
    # min-lease-time 720;
    max-lease-time 5760;
}' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

Script di atas akan mengatur client pada Switch3 agar mendapatkan alamat IP pada rentang `10.40.3.16 - 10.40.3.32` dan `10.40.3.64 - 10.40.3.80`, serta juga akan mengatur client pada Swith4 agar mendapatkan alamat IP pada rentang `10.40.4.12 - 10.40.4.20` dan `10.40.4.160 - 10.40.4.168`.  
```bash
subnet 10.40.3.0 netmask 255.255.255.0 {
    range 10.40.3.16 10.40.3.32;
    range 10.40.3.64 10.40.3.80;
    option routers 10.40.3.10;
}

subnet 10.40.4.0 netmask 255.255.255.0 {
    range 10.40.4.12 10.40.4.20;
    range 10.40.4.160 10.40.4.168;
    option routers 10.40.4.0;
}
```
Beberapa konfigurasi seperti `option broadcast-address` dan `option domain-name-server` juga ditambahkan agar dapat DNS yang telah disiapkan sebelumnya dapat digunakan.  
```bash
subnet 10.40.3.0 netmask 255.255.255.0 {
    option broadcast-address 10.40.3.255;
    option domain-name-servers 10.40.1.2;
}

subnet 10.40.4.0 netmask 255.255.255.0 {
    option broadcast-address 10.40.4.255;
    option domain-name-servers 10.40.1.2;
}
```
Untuk mengatur lama waktu DHCP server meminjamkan alamat IP kepada client, kita gunakan fungsi `default-lease-time` dan `max-lease-team` dalam satuan detik. Karena pada Switch3 dipinjamkan alamat IP selama 3 menit, maka kita set `default-lease-time`-nya adalah 3 menit == 180 detik (180s). Sedangkan Switch4 kita set menjadi 12 menit == 720 detik (720s). Untuk mengatur waktu maksimalnya, maka kita set `max-lease-team` pada kedua switch menjadi 96 menit == 5760 detik (5760s).
```bash
subnet 10.40.3.0 netmask 255.255.255.0 {
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.40.4.0 netmask 255.255.255.0 {
    default-lease-time 720;
    max-lease-time 5760;
}
```

Setelah melakukan konfigurasi pada DHCP server, selanjutnya kita perlu untuk melakukan setup untuk DHCP relay. Jalankan script di bawah ini pada DHCP relay, dan juga pastikan sudah terinstall `isc-dhcp-relay`.

```bash
echo 'SERVERS="10.40.1.1"
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=""' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf

service isc-dhcp-relay restart
```

Script di atas akan mengatur DHCP relay untuk meneruskan requests menuju DHCP server dengan alamat IP `10.40.1.1`, dan juga akan melayani permintaan DHCP dari interface `eth1`, `eth2`, `eth3`, dan `eth4`. Selain itu, script di atas juga mengaktifkan IP forwarding.