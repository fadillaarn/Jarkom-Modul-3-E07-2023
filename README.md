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

## Soal 6

> Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.

Jalankan script berikut pada masing-masing PHP worker. Sebelumnya, pastikan pada setiap PHP worker sudah terinstall package `wget`, `unzip`, `nginx`, `php7.3`, dan `php7.3-fpm`.

```bash
mkdir -p /var/www/granz.channel.e07

wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O /var/www/granz.channel.e07.zip
unzip /var/www/granz.channel.e07.zip -d /var/www/granz.channel.e07
mv /var/www/granz.channel.e07/modul-3 /var/www/granz.channel.e07
rm -rf /var/www/granz.channel.e07.zip

echo nameserver 10.40.1.2 > /etc/resolv.conf

echo 'server {
    listen 80;
    root /var/www/granz.channel.e07;
    index index.php index.html index.htm;
    server_name _;
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
    }
    location ~ /\.ht {
        deny all;
    }
    error_log /var/log/nginx/jarkom_error.log;
    access_log /var/log/nginx/jarkom_access.log;
}' > /etc/nginx/sites-available/granz.channel.e07

echo '<!DOCTYPE html>
<html>
<head>
    <title>Granz Channel Map</title>
    <link rel="stylesheet" type="text/css" href="css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Welcome to Granz Channel</h1>
        <p><?php
            $hostname = gethostname();
            echo "Request ini dihandle oleh: $hostname<br>"; ?> </p>
        <p>Enter your name to validate:</p>
        <form method="POST" action="index.php">
            <input type="text" name="name" id="nameInput">
            <button type="submit" id="submitButton">Submit</button>
        </form>
        <p id="greeting"><?php
            if(isset($_POST['name'])) {
                $name = $_POST['name'];
                echo "Hello, $name!";
            }
        ?></p>
    </div>
    <script src="js/script.js"></script>
</body>' > /var/www/granz.channel.e07/index.php

ln -s /etc/nginx/sites-available/granz.channel.e07 /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service php7.3-fpm restart
service nginx restart
nginx -t
```

Script di atas akan mengunduh repository dari link yang disediakan, dan akan melakukan ekstraksi (unzip).
```bash
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O /var/www/granz.channel.e07.zip
unzip /var/www/granz.channel.e07.zip -d /var/www/granz.channel.e07
mv /var/www/granz.channel.e07/modul-3 /var/www/granz.channel.e07
rm -rf /var/www/granz.channel.e07.zip
```

Setelah mengunduh repositori dan melakukan ekstraksi, kita akan melakukan beberapa konfigurasi pada `nginx` sebagai berikut.

```bash
echo 'server {
    listen 80;
    root /var/www/granz.channel.e07;
    index index.php index.html index.htm;
    server_name _;
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
    }
    location ~ /\.ht {
        deny all;
    }
    error_log /var/log/nginx/jarkom_error.log;
    access_log /var/log/nginx/jarkom_access.log;
}' > /etc/nginx/sites-available/granz.channel.e07

ln -s /etc/nginx/sites-available/granz.channel.e07 /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default
```

Selanjutnya, kita akan sedikit mengedit file `index.php` dalam repositori sebelumnya sesuai ketentuan sebagai berikut.

```bash
echo '<!DOCTYPE html>
<html>
<head>
    <title>Granz Channel Map</title>
    <link rel="stylesheet" type="text/css" href="css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Welcome to Granz Channel</h1>
        <p><?php
            $hostname = gethostname();
            echo "Request ini dihandle oleh: $hostname<br>"; ?> </p>
        <p>Enter your name to validate:</p>
        <form method="POST" action="index.php">
            <input type="text" name="name" id="nameInput">
            <button type="submit" id="submitButton">Submit</button>
        </form>
        <p id="greeting"><?php
            if(isset($_POST['name'])) {
                $name = $_POST['name'];
                echo "Hello, $name!";
            }
        ?></p>
    </div>
    <script src="js/script.js"></script>
</body>' > /var/www/granz.channel.e07/index.php
```

## Soal 7

> Kepala suku dari Bredt Region memberikan resource server sebagai berikut:

> 1. Lawine, 4GB, 2vCPU, dan 80 GB SSD.
> 2. Linie, 2GB, 2vCPU, dan 50 GB SSD.
> 3. Lugner 1GB, 1vCPU, dan 25 GB SSD.  

> Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

Pertama, arahkan domain pada DNS Server agar menuju IP dari Eisen sebagai Load Balancer, atau jalankan script berikut pada node Heiter.

```bash
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
@               IN      A       10.40.2.2' > /etc/bind/jarkom/riegel.canyon.e07.com

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
@               IN      A       10.40.2.2' > /etc/bind/jarkom/granz.channel.e07.com
```
Domain yang sebelumnya mengarah ke worker, sekarang sudah mengarah ke IP Load Balancer. Selanjutnya, kita akan melakukan konfigurasi `nginx` pada node Load Balancer sebagai berikut. Tentu saja pastikan juga bahwa sudah terinstall package `nginx` di Load Balancer.
```bash
echo 'upstream myweb  {
    server 10.40.3.1 weight=128; 
    server 10.40.3.2 weight=40;
    server 10.40.3.3 weight=5;
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    location / {
        proxy_pass http://myweb;
    }
}' > /etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```
Berdasarkan ilustrasi soal, kami menyimpulkan bahwa algoritma load-balancing yang digunakan dalam soal ini adalah algoritma Weighted Round-robin. Hal tersebut dikarenakan setiap server yang digunakan memiliki spesifikasi yang berbeda-beda. Dalam hal ini, kami memberikan weight pada tiap servernya dengan acuan komparasi hasil perkalian resource setiap servernya, hingga diperoleh hasil akhir yang paling sederhana yaitu 128 : 40 : 5.
```bash
upstream myweb  {
    server 10.40.3.1 weight=128; 
    server 10.40.3.2 weight=40;
    server 10.40.3.3 weight=5;
}
```
Kemudian, lakukan benchmarking menggunakan `apache2-utils` pada sembarang node client. Pastikan juga bahwa package tersebut sudah terinstall. Jalankan perintah berikut pada salah satu client.  
```bash
ab -n 1000 -c 100 http://www.granz.channel.e07.com/
```

## Soal 8

> Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
> 1. Nama Algoritma Load Balancer
> 2. Report hasil testing pada Apache Benchmark
> 3. Grafik request per second untuk masing masing algoritma. 
> 4. Analisis

Berikut adalah script yang perlu dijalankan pada node Load Balancer untuk melakukan testing masing-masing algoritma.

1. Algoritma Round-robin  
```bash
echo 'upstream myweb  {
    server 10.40.3.1;
    server 10.40.3.2;
    server 10.40.3.3;
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    location / {
        proxy_pass http://myweb;
    }
}' > /etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```
2. Algoritma Weighted Round-robin
```bash
echo 'upstream myweb  {
    server 10.40.3.1 weight=128;
    server 10.40.3.2 weight=40;
    server 10.40.3.3 weight=5;
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    location / {
        proxy_pass http://myweb;
    }
}' > /etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```
3. Algoritma Least Connection
```bash
echo 'upstream myweb  {
    least_conn;
    server 10.40.3.1;
    server 10.40.3.2;
    server 10.40.3.3;
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    location / {
        proxy_pass http://myweb;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;
    }
}' > /etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```
4. Algoritma IP Hash
```bash
echo 'upstream myweb  {
    least_conn;
    server 10.40.3.1 weight=4; # IP Lugner
    server 10.40.3.2 weight=2; # IP Linie
    server 10.40.3.3 weight=1; # IP Lawine
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    location / {
        proxy_pass http://myweb;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;
    }
}' > /etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```
5. Algoritma Generic Hash
```bash
echo 'upstream myweb  {
    hash $request_uri consistent;
    server 10.40.3.1;
    server 10.40.3.2;
    server 10.40.3.3;
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    location / {
        proxy_pass http://myweb;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;
    }
}' > /etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```
Setelahnya, lakukan benchmarking seperti biasa pada sembarang node client menggunakan `apache2-utils` dan juga `htop` pada masing-masing PHP Worker.

Hasil benchmarking (Grimoire) dari kelompok kami dapat dilihat di link [berikut](https://docs.google.com/document/d/1SQJ5kGBgmNA37KcT3MGDfBn2mwxk-3DGQwyrWkb0qsY/edit?usp=sharing).

## Soal 9

> Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

Caranya serupa dengan [Soal 8](#soal-8), hanya saja pada benchmarking kali ini kita akan melakukannya dalam 3 kondisi, yakni saat menggunakan 3 worker, 2 worker, dan 1 worker. Oleh karena itu, kita perlu mengubah jumlah worker yang digunakan dengan mengedit konfigurasi pada file `/etc/nginx/sites-available/lb-jarkom` di node Load Balancer.
1. 3 Worker
```bash
upstream myweb  {
    server 10.40.3.1;
    server 10.40.3.2;
    server 10.40.3.3;
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    location / {
        proxy_pass http://myweb;
    }
}
```
2. 2 Worker
```bash
upstream myweb  {
    server 10.40.3.1;
    server 10.40.3.2;
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    location / {
        proxy_pass http://myweb;
    }
}
```
3. 1 Worker
```bash
upstream myweb  {
    server 10.40.3.1;
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    location / {
        proxy_pass http://myweb;
    }
}
```
Setelahnya, lakukan benchmarking seperti biasa pada sembarang node client menggunakan `apache2-utils` dan juga `htop` pada masing-masing PHP Worker.

Hasil benchmarking (Grimoire) dari kelompok kami dapat dilihat di link [berikut](https://docs.google.com/document/d/1SQJ5kGBgmNA37KcT3MGDfBn2mwxk-3DGQwyrWkb0qsY/edit?usp=sharing).

## Soal 10

> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

Untuk menambahkan konfigurasi autentikasi, jalankan script di bawah pada node Load Balancer.

```bash
echo 'upstream granz.channel.e07.com  {
    server 10.40.3.1;
    server 10.40.3.2;
    server 10.40.3.3;
}

server {
    listen 80;
    server_name granz.channel.e07.com;
    
    location / {
        proxy_pass http://granz.channel.e07.com;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;

        auth_basic "Administrators Area";
        auth_basic_user_file /etc/nginx/rahasiakita;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' > /etc/nginx/sites-available/lb-jarkom

htpasswd -c -b /etc/nginx/rahasiakita netics ajke07
```