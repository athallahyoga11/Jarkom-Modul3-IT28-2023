# Jarkom-Modul-3-IT28-2023
**Praktikum Jaringan Komputer Modul 3 Tahun 2023**

## Anggota
| Nama | NRP |
|---------------------------|------------|
|Yoga Hartono| 5027211023| 
|Athallah Narda Wiyoga| 5027211041| 

# Laporan Resmi


## Topologi
![image](https://cdn.discordapp.com/attachments/1067327620938747946/1176045616418279544/Screenshot_2023-11-20_at_13.25.15.png?ex=656d70d4&is=655afbd4&hm=66c5ee565423a85f7a5a71802e23fba1fda7ac2b3b2424df898f19aee40899bc&)

## Config
- **Aura (DHCP Relay)**
```
auto eth0
iface eth0 inet dhcp
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.247.0.0/16

auto eth1
iface eth1 inet static
	address 192.247.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.247.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.247.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.247.4.0
	netmask 255.255.255.0

```
- **Himmel (DHCP Server)**
```
auto eth0
iface eth0 inet static
	address 192.247.1.1
	netmask 255.255.255.0
	gateway 192.247.1.0

```
- **Heiter (DNS Server)**
```
auto eth0
iface eth0 inet static
	address 192.247.1.2
	netmask 255.255.255.0
	gateway 192.247.1.0
```
- **Denken (Database Server)**
```
auto eth0
iface eth0 inet static
	address 192.247.2.1
	netmask 255.255.255.0
	gateway 192.247.2.0
```
- **Eisen (Load Balancer)**
```
auto eth0
iface eth0 inet static
	address 192.247.2.2
	netmask 255.255.255.0
	gateway 192.247.2.0
```
- **Frieren (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.247.4.3
	netmask 255.255.255.0
	gateway 192.247.4.0

hwaddress ether 9e:d5:2c:df:39:32
```
- **Flamme (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.247.4.2
	netmask 255.255.255.0
	gateway 192.247.4.0

hwaddress ether da:cc:d9:29:59:e8
```
- **Fern (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.247.4.1
	netmask 255.255.255.0
	gateway 192.247.4.0

hwaddress ether 8a:31:c6:e6:a2:7b
```
- **Lawine (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.247.3.3
	netmask 255.255.255.0
	gateway 192.247.3.0

hwaddress ether 8e:c1:16:c2:6d:4b
```
- **Linie (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.247.3.2
	netmask 255.255.255.0
	gateway 192.247.3.0

hwaddress ether 7a:e7:1c:d2:ae:44
```
- **Lugner (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.247.3.1
	netmask 255.255.255.0
	gateway 192.247.3.0

hwaddress ether 8a:02:fd:bb:05:be
```
- **Revolte, Richter, Sein, dan Stark (Client)**
```
auto eth0
iface eth0 inet dhcp
```

### Sebelum Memulai 
setiap node, kita inisiasi pada `.bashrc` menggunakan `nano`

- DNS Server
  ```sh
  echo 'nameserver 192.168.122.1' > /etc/resolv.conf
  apt-get update
  apt-get install bind9 -y  
  ```
- DHCP Server
  ```sh
  echo 'nameserver 192.247.1.2' > /etc/resolv.conf   # Pastikan DNS Server sudah berjalan 
  apt-get update
  apt install isc-dhcp-server -y
  ```
- DHCP Relay
  ```sh
  apt-get update
  apt install isc-dhcp-relay -y
  ```
- Database Server
  ```sh
  echo 'nameserver 192.247.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install mariadb-server -y
  service mysql start

  Lalu jangan lupa untuk mengganti [bind-address] pada file /etc/mysql/mariadb.conf.d/50-server.cnf menjadi 0.0.0.0 dan jangan lupa untuk melakukan restart mysql kembali
  ```
- Load Balancer
  ```sh
  echo 'nameserver 192.247.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install apache2-utils -y
  apt-get install nginx -y
  apt-get install lynx -y

  service nginx start
  ```
- Worker PHP
  ```sh
  echo 'nameserver 192.247.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install nginx -y
  apt-get install wget -y
  apt-get install unzip -y
  apt-get install lynx -y
  apt-get install htop -y
  apt-get install apache2-utils -y
  apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y

  service nginx start
  service php7.3-fpm start
  ```
- Worker Laravel
  ```sh
  echo 'nameserver 192.247.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install lynx -y
  apt-get install mariadb-client -y
  # Test connection from worker to database
  # mariadb --host=192.247.2.1 --port=3306   --user=kelompokIT28 --password=passwordIT28 dbkelompokIT28 -e "SHOW DATABASES;"
  apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
  curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
  sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
  apt-get update
  apt-get install php8.0-mbstring php8.0-xml php8.0-cli   php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
  apt-get install nginx -y

  service nginx start
  service php8.0-fpm start
  ```
- Client
  ```sh
  apt update
  apt install lynx -y
  apt install htop -y
  apt install apache2-utils -y
  apt-get install jq -y
  ```

## Soal 1 
>Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

Pertama kita perlu mempersiapkan konfigurasi topologi dan [setup](#sebelum-memulai) seperti aturan diatas. Selanjutnya untuk kebutuhan testing, kita perlu menambahkan register domain berupa ``riegel.canyon.IT28.com`` untuk worker Laravel dan granz.channel.IT28.com untuk worker PHP yang mengarah pada worker dengan IP ``192.247.x.1``. Karena pada konfirgurasi topologi sebelumnya seluruh worker sudah menggunakan DHCP, maka kita perlu modifikasi sedikit pada node ``Lugner`` dan ``Fern`` seperti dibawah ini
- **Lugner (PHP Worker)**
```
auto eth0
iface eth0 inet static
	address 192.247.3.1
	netmask 255.255.255.0
	gateway 192.247.3.0
```
- **Fern (Laravel Worker)**
```
auto eth0
iface eth0 inet static
	address 192.247.4.1
	netmask 255.255.255.0
	gateway 192.247.4.0
```

Selanjutnya pada DNS Server (Heiter), kita perlu menjalankan command dibawah ini

### Script
```sh
echo 'zone "riegel.canyon.IT28.com" {
    type master;
    file "/etc/bind/sites/riegel.canyon.IT28.com";
};

zone "granz.channel.IT28.com" {
    type master;
    file "/etc/bind/sites/granz.channel.IT28.com";
};

zone "1.247.192.in-addr.arpa" {
    type master;
    file "/etc/bind/sites/1.247.192.in-addr.arpa";
};' > /etc/bind/named.conf.local

mkdir -p /etc/bind/sites
cp /etc/bind/db.local /etc/bind/sites/riegel.canyon.IT28.com
cp /etc/bind/db.local /etc/bind/sites/granz.channel.IT28.com
cp /etc/bind/db.local /etc/bind/sites/1.247.192.in-addr.arpa

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.IT28.com. root.riegel.canyon.IT28.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.IT28.com.
@       IN      A       192.247.4.1     ; IP Fern
www     IN      CNAME   riegel.canyon.IT28.com.' > /etc/bind/sites/riegel.canyon.IT28.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.IT28.com. root.granz.channel.IT28.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.IT28.com.
@       IN      A       192.247.3.1     ; IP Lugner
www     IN      CNAME   granz.channel.IT28.com.' > /etc/bind/sites/granz.channel.IT28.com

echo 'options {
      directory "/var/cache/bind";

      forwarders {
              192.168.122.1;
      };

      // dnssec-validation auto;
      allow-query{any;};
      auth-nxdomain no;    # conform to RFC1035
      listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 start
```
### Result

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176084123819986954/image.png?ex=656d94b1&is=655b1fb1&hm=8c120e7f96146b071434e291892d045043eec57e065c2cdb9bec5727229462a2&)


## Soal 2
>Semua CLIENT harus menggunakan konfigurasi dari DHCP Server. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) untuk DHCP Server terlebih dahulu. Selanjutnya kita perlu menjalankan command dibawah ini pada DHCP Server
### Script 
```sh
echo 'subnet 192.247.1.0 netmask 255.255.255.0 {
}

subnet 192.247.2.0 netmask 255.255.255.0 {
}

subnet 192.247.3.0 netmask 255.255.255.0 {
    range 192.247.3.16 192.247.3.32;
    range 192.247.3.64 192.247.3.80;
    option routers 192.247.3.0;
}' > /etc/dhcp/dhcpd.conf
```

## Soal 3 
>Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

Selanjutnya kita perlu menambahkan beberapa konfigurasi baru untuk switch4 dengan menjalankan command dibawah ini

### Script 
```sh
echo 'subnet 192.247.1.0 netmask 255.255.255.0 {
}

subnet 192.247.2.0 netmask 255.255.255.0 {
}

subnet 192.247.3.0 netmask 255.255.255.0 {
    range 192.247.3.16 192.247.3.32;
    range 192.247.3.64 192.247.3.80;
    option routers 192.247.3.0;
}

subnet 192.247.4.0 netmask 255.255.255.0 {
    range 192.247.4.12 192.247.4.20;
    range 192.247.4.160 192.247.4.168;
    option routers 192.247.4.0;
} ' > /etc/dhcp/dhcpd.conf
```

## Soal 4 
>Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

kita akan menambahkan beberapa konfigurasi seperti ``option broadcast-address`` dan ``option domain-name-server`` agar dapat DNS yang telah disiapkan sebelumnya dapat digunakan

### Script
```sh 
subnet 192.247.3.0 netmask 255.255.255.0 {
    ...
    option broadcast-address 192.247.3.255;
    option domain-name-servers 192.247.1.2;
    ...
}

subnet 192.247.4.0 netmask 255.255.255.0 {
    option broadcast-address 192.247.4.255;
    option domain-name-servers 192.247.1.2;
} 
```

Lalu gunakan ``shell`` script sebagai berikut

```sh
echo 'subnet 192.247.1.0 netmask 255.255.255.0 {
}

subnet 192.247.2.0 netmask 255.255.255.0 {
}

subnet 192.247.3.0 netmask 255.255.255.0 {
    range 192.247.3.16 192.247.3.32;
    range 192.247.3.64 192.247.3.80;
    option routers 192.247.3.0;
    option broadcast-address 192.247.3.255;
    option domain-name-servers 192.247.1.2;
}

subnet 192.247.4.0 netmask 255.255.255.0 {
    range 192.247.4.12 192.247.4.20;
    range 192.247.4.160 192.247.4.168;
    option routers 192.247.4.0;
    option broadcast-address 192.247.4.255;
    option domain-name-servers 192.247.1.2;
} ' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server start
```

Selanjutnya kita perlu untuk melakukan [setup](#sebelum-memulai) untuk DHCP Relay terlebih dahulu. Selanjutnya kita perlu menjalankan command dibawah ini pada DHCP Relay
```sh
echo '# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.247.1.1"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay

service isc-dhcp-relay start 
```

Lalu pada file ``/etc/sysctl.conf`` lakukan uncommented pada ``net.ipv4.ip_forward=1``

Terakhir jangan lupa untuk restart seluruh client agar dapat melakukan leasing IP dari DHCP Server

### Result

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176083890230808576/image.png?ex=656d947a&is=655b1f7a&hm=7cb8544a4197dc3c1bd1bffe606eff0af2b038ddce46b8a7e28764e40fdd4497&)



## Soal 5
>Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

Kita perlu menggunakan bantuan fungsi ``default-lease-time`` dan ``max-lease-team`` dimana satuannya adalah detik.

Karena pada ``switch3`` dapat meminjamkan IP selama ``3 Menit`` dan ``Switch4`` dapat meminjamkan IP selama ``12 Menit``. Sehingga pada ``Switch3`` membutuhkan waktu ``180 s`` dan ``Switch4`` membutuhkan waktu ``720 s`` dan untuk ``max-lease-time`` nya adalah ``96 menit`` dimana akan menjadi ``5760 s``
 
Selanjutnya kita perlu menambahkan beberapa konfigurasi baru untuk mengatur leasing time pada switch3 dan switch4 sesuai dengan aturan soal. Kita dapat menjalankan command berikut pada DHCP Server

### Script
```sh
echo 'subnet 192.247.1.0 netmask 255.255.255.0 {
}

subnet 192.247.2.0 netmask 255.255.255.0 {
}

subnet 192.247.3.0 netmask 255.255.255.0 {
    range 192.247.3.16 192.247.3.32;
    range 192.247.3.64 192.247.3.80;
    option routers 192.247.3.0;
    option broadcast-address 192.247.3.255;
    option domain-name-servers 192.247.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.247.4.0 netmask 255.255.255.0 {
    range 192.247.4.12 192.247.4.20;
    range 192.247.4.160 192.247.4.168;
    option routers 192.247.4.0;
    option broadcast-address 192.247.4.255;
    option domain-name-servers 192.247.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}

service isc-dhcp-server restart
```
### Result

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176083801382858814/image.png?ex=656d9464&is=655b1f64&hm=73c214b89ea43a7b480317df7514f73f890027bc01e6819a12276dc8653ad9b4&)

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176083715517067314/image.png?ex=656d9450&is=655b1f50&hm=060d7756e4db9a8d3f36316229297a526f0b3bed0201f5f04187b3eb5b90a13b&)

## Soal 6
> Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu pada **seluruh PHP Worker**. Jika sudah, silahkan untuk melakukan konfigurasi tambahan sebagai berikut untuk melakukan download dan unzip menggunakan command ``wget``
```sh
wget -O '/var/www/granz.channel.IT28.com' 'https://drive.google.com/u/0/uc?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download'
unzip -o /var/www/granz.channel.IT28.com -d /var/www/
rm /var/www/granz.channel.IT28.com
mv /var/www/modul-3 /var/www/granz.channel.IT28.com
```

### Script
Setelah melakukan download dan unzip. Sekarang kita bisa melakukan konfigurasi pada ``nginx`` sebagai berikut:
```sh 
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/granz.channel.IT28.com
ln -s /etc/nginx/sites-available/granz.channel.IT28.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/granz.channel.IT28.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;  # Sesuaikan versi PHP dan socket
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/granz.channel.IT28.com

service nginx restart
```

### Result 
Jalanin Perintah ``lynx localhost`` pada masing-masing worker dan hasilnya akan sebagai berikut:

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176084314631450664/image.png?ex=656d94df&is=655b1fdf&hm=ecf3eab6e319909fd38adb2537011423e30289f1419b307f8a3f7ef06cdcba77&)

## Soal 7
> Kepala suku dari Bredt Region memberikan resource server sebagai berikut: Lawine, 4GB, 2vCPU, dan 80 GB SSD. Linie, 2GB, 2vCPU, dan 50 GB SSD. Lugner 1GB, 1vCPU, dan 25 GB SSD. Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah melakukan konfigurasi diatas, sekarang lakukan konfigurasi ``Load Balancing`` pada node ``Eisen`` sebagai berikut 

### Script
Sebelum melakukan setup soal 7. Buka kembali Node ``DNS Server`` dan arahkan ``domain`` tersebut pada ``IP Load Balancer Eisen``

```sh
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.IT28.com. root.riegel.canyon.IT28.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.IT28.com.
@       IN      A       192.247.2.2     ; IP LB Eiken
www     IN      CNAME   riegel.canyon.IT28.com.' > /etc/bind/sites/riegel.canyon.IT28.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.IT28.com. root.granz.channel.IT28.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.IT28.com.
@       IN      A       192.247.2.2     ; IP LB Eiken
www     IN      CNAME   granz.channel.IT28.com.' > /etc/bind/sites/granz.channel.IT28.com
```

Lalu kembali ke node ``Eisen`` dan lakukan konfigurasi pada nginx sebagai berikut

```sh 
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
    server 192.247.3.1;
    server 192.247.3.2;
    server 192.247.3.3;
}

server {
    listen 80;
    server_name granz.channel.IT28.com www.granz.channel.IT28.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

Setelah itu lakukan [konfigurasi](#sebelum-memulai) pada salah satu client. Disini kami melakukan konfigurasi pada client ``Revolte``

### Result
Jalankan perintah berikut pada client ``Revolte``
```sh
ab -n 1000 -c 100 http://www.granz.channel.IT28.com/ 
```

dan akan mendapatkan hasil seperti berikut 

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176084686498451477/image.png?ex=656d9537&is=655b2037&hm=f99082b15fe7054bb8014d78a5ad1e724c24a1b2e1813f5c38a88a4b44ce916d&)


## Soal 8
> Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut: 1. Nama Algoritma Load Balancer; 2. Report hasil testing pada Apache Benchmark; 3.Grafik request per second untuk masing masing algoritma; 4. Analisis

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Selebihnya untuk konfigurasinya sama dengan [Soal 7](#Soal-7)

Untuk laporan ``grimoire`` nya kami membuatnya di google.docs pada [link](https://docs.google.com/document/d/1mjKvsNKzQ8XagdoAB8i9nLP6MNEwcRq2ucCuLPb50F0/edit?usp=sharing) ini.

### Script
Jalankan command berikut pada client ``Revolte``
```sh
ab -n 200 -c 10 http://www.granz.channel.IT28.com/ 
```

### Result 

**Round Robin**

![Screenshot (1015)](https://cdn.discordapp.com/attachments/897449980896346166/1176085246815506452/image.png?ex=656d95bd&is=655b20bd&hm=1a0e405564c7a4d71287a71d4fbe379ec25759fad1931a7e3631fbab1c2cad18&)

**Least-connection**

![Screenshot (1019)](https://cdn.discordapp.com/attachments/897449980896346166/1176085592988205056/image.png?ex=656d960f&is=655b210f&hm=57977d45aa9a2547a3eed57a7a7556d130a94caed0315bda9b4c3b2182356c45&)

**IP Hash**

![Screenshot (1020)](https://cdn.discordapp.com/attachments/897449980896346166/1176085497060278282/image.png?ex=656d95f9&is=655b20f9&hm=78e51a870c35a7671b2279e8105e17477040a155a470fb76126cdd33801db31c&)

**Generic Hash**

![Screenshot (1021)](https://cdn.discordapp.com/attachments/897449980896346166/1176085393666478110/image.png?ex=656d95e0&is=655b20e0&hm=6ca913e6517280ad8c8464737d7f1ebc6b04523b9d6b7a1732cfd6014e481c25&)


## Soal 9
> Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah melakukan setup pada node ``Eisen`` sekarang lakukan testing pada load balancer yang telah dibuat sebelumnya. Yang menjadi pembeda adalah kita harus melakukan testing menggunakan ``1 worker``, ``2 worker``, dan ``3 worker``. 

### Script
Jalankan command berikut pada client ``Revolte``
```sh
ab -n 100 -c 10 http://www.granz.channel.IT28.com/ 
```

### Result

**3 Worker**

![Screenshot (1022)](https://cdn.discordapp.com/attachments/897449980896346166/1176094413349466163/image.png?ex=656d9e46&is=655b2946&hm=0ecd17bbc197bdafd15880b7afc11b0542a9b10ab0d8429171cc288d3d7d9e39&)

> Request per second 303.87 [#/sec] (mean)

**2 Worker**

![Screenshot (1023)](https://cdn.discordapp.com/attachments/897449980896346166/1176085726379642940/image.png?ex=656d962f&is=655b212f&hm=83ace43d96913e21bca3875305a53a5b2355ba00197e40b3203e334ef6dbc02e&)

> Request per second 336.77 [#/sec] (mean)

**1 Worker**

![Screenshot (1024)](https://cdn.discordapp.com/attachments/897449980896346166/1176086117146177576/image.png?ex=656d968c&is=655b218c&hm=85ee57a4980c47a22295f84e6b2bd20772b0c32bdd4a4d436a38423a876f81c3&)

> Request per second 393.40 [#/sec] (mean)



## Soal 10
> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/ 

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah itu, lakukan beberapa konfigurasi sebagai berikut

### Script
```sh 
mkdir /etc/nginx/rahasisakita
htpasswd -c /etc/nginx/rahasisakita/htpasswd netics
```

Lalu, masukkan passwordnya ``ajkIT28``

Jika sudah memasukkan ``password`` dan ``re-type password``. Sekarang bisa dicoba dengan menambahkan command berikut pada setup nginx.

```sh
auth_basic "Restricted Content";
auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
```

### Result


![image](https://cdn.discordapp.com/attachments/897449980896346166/1176094777318588466/image.png?ex=656d9e9d&is=655b299d&hm=f5e980149a39ba9e3aef134b1268de7e37752fdfda7af724e5dc360fa44aec42&)



## Soal 11
> Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. (11) hint: (proxy_pass)

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah itu, lakukan beberapa konfigurasi tambahan pada nginx sebagai berikut 

### Script
```sh
location ~ /its {
    proxy_pass https://www.its.ac.id;
    proxy_set_header Host www.its.ac.id;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

Berikut adalah full scriptnya 
```sh
echo 'upstream worker {
    server 192.247.3.1;
    server 192.247.3.2;
    server 192.247.3.3;
}

server {
    listen 80;
    server_name granz.channel.IT28.com www.granz.channel.IT28.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        proxy_pass http://worker;
    }

    location ~ /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}' > /etc/nginx/sites-available/lb_php
```

Maksudnya adalah ketika kita melakukan akses pada endpoint yang mengandung ``/its`` akan diarahkan oleh ``proxy_pass`` menuju ``https://www.its.ac.id``. Jadi ketika melakukan testing pada client ``Revolte`` dengan menggunakan perintah berikut 

```sh
lynx www.granz.channel.IT28.com/its
```

### Result

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176095025352933386/image.png?ex=656d9ed8&is=655b29d8&hm=3307607fce27518b02c7a584dcabfa4b0ded5cd1550c79298f633ce13ef3420e&)

## Soal 12
> Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168. 

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah itu, Kami hanya menambahkan beberapa konfigurasi di nginx sebagai berikut 

### Script

```sh
location / {
    allow 192.247.3.69;
    allow 192.247.3.70;
    allow 192.247.4.167;
    allow 192.247.4.168;
    deny all;
    proxy_pass http://worker;
}
```

Berikut adalah full scriptnya
```sh
echo 'upstream worker {
    server 192.247.3.1;
    server 192.247.3.2;
    server 192.247.3.3;
}

server {
    listen 80;
    server_name granz.channel.IT28.com www.granz.channel.IT28.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        allow 192.247.3.69;
        allow 192.247.3.70;
        allow 192.247.4.167;
        allow 192.247.4.168;
        deny all;
        proxy_pass http://worker;
    }

    location /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}' > /etc/nginx/sites-available/lb_php
```

Disini kami hanya mengizinkan beberapa ``IP`` saja sesuai dengan ketentual soal dan kamu menolak seluruh ``IP`` selain yang telah ditentukan soal. Untuk melakukan testingnya. Bisa dilakukan dengan membuka client yang mendapatkan ``IP 192.247.3.69 atau 192.247.3.70 atau 192.247.4.167 atau 192.247.4.168``

### Result 

**Forbiden**

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176095493814751302/image.png?ex=656d9f48&is=655b2a48&hm=dcc140208ccc9900e93bf39c9c2571c0821f7e0b4150098894272091cce44966&)


## Soal 13
> Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. (13)

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah itu kita buka ``Database Server`` nya yaitu ``Denken`` dan lakukan konfigurasi sebagai berikut

### Script
```sh
# Db akan diakses oleh 3 worker, maka 
echo '# This group is read both by the client and the server
# use it for options that affect everything
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

# Options affecting the MySQL server (mysqld)
[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf
```

Lalu jangan lupa untuk mengganti ``[bind-address]`` pada file ``/etc/mysql/mariadb.conf.d/50-server.cnf`` menjadi ``0.0.0.0`` 

```sh 
cd /etc/mysql/mariadb.conf.d/50-server.cnf

# Changes
bind-address            = 0.0.0.0
```

Jangan lupa untuk restart mysql nya ``service mysql restart``

Setelah itu jalankan perintah berikut: 

```sh
mysql -u root -p
Enter password: 

CREATE USER 'kelompokIT28'@'%' IDENTIFIED BY 'passwordIT28';
CREATE USER 'kelompokIT28'@'localhost' IDENTIFIED BY 'passwordIT28';
CREATE DATABASE dbkelompokIT28;
GRANT ALL PRIVILEGES ON *.* TO 'kelompokIT28'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompokIT28'@'localhost';
FLUSH PRIVILEGES;
```



### Result
Setelah itu lakukan pengecekan di salah satu Laravel Worker. Disini kami akan melakukan pengecekan pada worker ``Fern`` dengan melakukan perintah ``shell`` berikut

```sh
mariadb --host=192.247.2.1 --port=3306 --user=kelompokIT28 --password=passwordIT28 dbkelompokIT28 -e "SHOW DATABASES;"
```

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176096056530980864/image.png?ex=656d9fce&is=655b2ace&hm=9a6f955a2d6cbbd101c2a27290750d38dc547b142142eb1dcc59aeb18621eed1&)

## Soal 14
> Frieren, Flamme, dan Fern memiliki Granz Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer 

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah itu, lakukan konfigurasi lagi sebagai berikut

### Script
Lakukan install composer

```sh
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer
```

Setelah itu install ``git`` dan lakukan cloning terhadap [resource](https://github.com/martuafernando/laravel-praktikum-jarkom) yang telah diberikan 

```sh
apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update
```

Setelah melakukan ``clone`` pada resource tersebut. Sekarang lakukan konfigurasi sebagai berikut pada masing-masing ``worker``

```sh 
cd /var/www/laravel-praktikum-jarkom && cp .env.example .env
echo 'APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=192.247.2.1
DB_PORT=3306
DB_DATABASE=dbkelompokIT28
DB_USERNAME=kelompokIT28
DB_PASSWORD=passwordIT28

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"' > /var/www/laravel-praktikum-jarkom/.env
cd /var/www/laravel-praktikum-jarkom && php artisan key:generate
cd /var/www/laravel-praktikum-jarkom && php artisan config:cache
cd /var/www/laravel-praktikum-jarkom && php artisan migrate
cd /var/www/laravel-praktikum-jarkom && php artisan db:seed
cd /var/www/laravel-praktikum-jarkom && php artisan storage:link
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret
cd /var/www/laravel-praktikum-jarkom && php artisan config:clear
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage
```

Setelah berhasil menjalankan semuanya dan tidak mendapatkan ``error``. Sekarang lakukan konfigurasi ``nginx`` sebagai berikut pada masing-masing worker dimana port nya adalah sebagai berikut 

```sh
192.247.4.1:8001; # Fern 
192.247.4.2:8002; # Flamme
192.247.4.3:8003; # Frieren
```

dan berikut merupakan konfigurasi ``nginx`` nya

```sh
echo 'server {
    listen <X>;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/implementasi_error.log;
    access_log /var/log/nginx/implementasi_access.log;
}' > /etc/nginx/sites-available/laravel-worker
```

dimana ``<X>`` merupakan port masing-masing Worker.


### Result
Setelah berhasil melakukan keseluruhan konfigurasi pada masing-masing ``worker``. Saatnya melakukan testing sebagai berikut

```sh
lynx localhost:[PORT]
```
dimana PORT yang ada adalah ``8001`` ``8002`` dan ``8003``. Sesuaikan dengan setup nginx sebelumnya.

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176096295518228510/image.png?ex=656da007&is=655b2b07&hm=403f0405b3aeb18a0f5a9985633be0e4df94f615c140fe94b91f7a37549687f3&)


## Soal 15
> Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. Untuk POST /api/auth/register

Untuk mengerjakan soal ini. Diperlukan melakukan testing menggunakan ``Apache Benchmark`` pada salah satu worker saja. Disini kami akan menggunakan worker laravel ``Fern`` yang nantinya akan menjadi worker yang akan ditesting oleh client ``Revolte``. Sebelum dilakukan testing, kami menggunakan bantuan file ``.json`` yang akan digunakan sebagai ``body`` yang akan dikirim pada endpoint ``/api/auth/register`` nantinya sebagai berikut

### Script
```sh
echo '
{
  "username": "kelompokIT28",
  "password": "passwordIT28"
}' > register.json
```

Lalu, lakukanlah perintah berikut dari sisis client ``Revolte`` 
```sh
ab -n 100 -c 10 -p register.json -T application/json http://192.247.4.1:8001/api/auth/register
```

### Result
Terdapat error dalam pengiriman sebanyak 100 request. Dikarenakan pada table ``users`` adalah unique. Dimana data ``username`` yang dimasukkan tidak boleh sama. Sehingga menyebabkan hanya ``1`` request saja yang diproses. ``99`` proses lainnya tidak diproses

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176096449843445781/image.png?ex=656da02c&is=655b2b2c&hm=75052052d90c44e695156d0139ac5ac0ecf80d5d4b9ec16fa915ceb31393a173&)


## Soal 16
> Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. Untuk POST /api/auth/login 

Untuk mengerjakan soal ini. Diperlukan melakukan testing menggunakan ``Apache Benchmark`` pada salah satu worker saja. Disini kami akan menggunakan worker laravel ``Fern`` yang nantinya akan menjadi worker yang akan ditesting oleh client ``Revolte``. Sebelum dilakukan testing, kami menggunakan bantuan file ``.json`` yang akan digunakan sebagai ``body`` yang akan dikirim pada endpoint ``/api/auth/login`` nantinya sebagai berikut

### Script
```sh
echo '
{
  "username": "kelompokIT28",
  "password": "passwordIT28"
}' > login.json
```

Lalu, lakukanlah perintah berikut dari sisis client ``Revolte`` 
```sh
ab -n 100 -c 10 -p login.json -T application/json http://192.247.4.1:8001/api/auth/login
```

### Result
Terdapat error dalam pengiriman sebanyak 100 request. Karena satu worker saja tidak kuat untuk mendapatkan request sebanyak itu 100 dalam waktu yang telah diberikan atau dengan kata lain CPU yang diterima tidak sanggup untuk memproses banyaknya request. Sehingga menyebabkan hanya ``63`` request saja yang berhasil di proses sedangkan ``37`` lainnya tidak berhasil di proses

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176096649337118771/image.png?ex=656da05c&is=655b2b5c&hm=c6382ac9206db927871d7ae6c0991e3b555befb11096814a906fc8422c2c8c2f&)

## Soal 17
> Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. Untuk GET /api/me

Untuk mengerjakan soal ini. Diperlukan melakukan testing menggunakan ``Apache Benchmark`` pada salah satu worker saja. Disini kami akan menggunakan worker laravel ``Fern`` yang nantinya akan menjadi worker yang akan ditesting oleh client ``Revolte``. Sebelum dilakukan testing. Ada beberapa konfigurasi yang harus disiapkan sebagai berikut 

### Script
Dapatkan tokennya terlebih dahulu sebelum mengakses endpoint ``/api/me``

```sh
curl -X POST -H "Content-Type: application/json" -d @login.json http://192.247.4.1:8001/api/auth/login > login_output.txt
```

Lalu jalankan perintah berikut untuk melakukan set ``token`` secara global

```sh
token=$(cat login_output.txt | jq -r '.token')
```

Setelah itu jalankan perintah berikut untuk melakukan testing

```sh
ab -n 100 -c 10 -H "Authorization: Bearer $token" http://192.247.4.1:8001/api/me
```

### Result 
Terdapat error dalam pengiriman sebanyak 100 request. Karena satu worker saja tidak kuat untuk mendapatkan request sebanyak itu 100 dalam waktu yang telah diberikan atau dengan kata lain CPU yang diterima tidak sanggup untuk memproses banyaknya request. Sehingga menyebabkan hanya ``62`` request saja yang berhasil di proses sedangkan ``38`` lainnya tidak berhasil di proses

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176096907299409950/image.png?ex=656da099&is=655b2b99&hm=edbeecae476cb7916f749f67a751d3b33d94de16d07518d9a83421d709a89c14&)


## Soal 18
> Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Granz Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern. 

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah itu, karena hanya diberikan perintah ketiga ``worker`` berjalan secara adil, kami memberikan implementasi dari ``Load Balancing`` karena sesuai dengan definisi nya yaitu membagi rata beban kerja. Maka dari itu, berikut merupakan konfigurasi ``nginx``

### Script
```sh
echo 'upstream worker {
    server 192.247.4.1:8001;
    server 192.247.4.2:8002;
    server 192.247.4.3:8003;
}

server {
    listen 80;
    server_name riegel.canyon.IT28.com www.riegel.canyon.IT28.com;

    location / {
        proxy_pass http://worker;
    }
} 
' > /etc/nginx/sites-available/laravel-worker

ln -s /etc/nginx/sites-available/laravel-worker /etc/nginx/sites-enabled/laravel-worker

service nginx restart
```

**Notes**
Hati-hati ``port`` tabrakan dengan ``load balancer`` dari ``php worker`` 

### Result
Setelah melakukan konfigurasi pada ``load balancer`` pada ``Eisen``. Sekarang waktunya melakukan testing pada client ``Revolte`` dengan menjalankan perintah berikut 

```sh
ab -n 100 -c 10 -p login.json -T application/json http://www.riegel.canyon.IT28.com/api/auth/login
```

akan memperoleh hasil sebagai berikut 

**Fern**

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176097029542379520/image.png?ex=656da0b6&is=655b2bb6&hm=c5bac11c7a17173be9ce0abc79797aa9a82f9b85086bb933b04772da0f951188&)

**Flamme**

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176097113017430107/image.png?ex=656da0ca&is=655b2bca&hm=51af637c6ce670c9eaae427c639795f5e45512e8b407af32587dadecbcf955b3&)

**Frieren**

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176097203064950804/image.png?ex=656da0e0&is=655b2be0&hm=afc0b212ae55d62f5d92a88524fc36ca593fc87e0272a49b2dc7e5490ee941c1&)

## Soal 19
> Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan -> pm.max_children, pm.start_servers, pm.min_spare_servers, pm.max_spare_servers sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.

Untuk mengerjakan soal ini terdapat beberapa penjelasan sebagai berikut 

**pm.max_children** 
Menentukan jumlah maksimum pekerja PHP (proses anak) yang dapat berjalan secara bersamaan. Nilai ini sebaiknya disesuaikan dengan kapasitas sumber daya server. Jika terlalu rendah, server mungkin tidak dapat menangani banyak permintaan secara bersamaan, sementara jika terlalu tinggi, dapat menyebabkan kelebihan beban dan kekurangan sumber daya.

**pm.start_servers**
Menentukan jumlah pekerja PHP yang akan dimulai secara otomatis ketika PHP-FPM pertama kali dijalankan atau direstart. Ini membantu dalam mengoptimalkan performa pada saat server pertama kali dimulai.

**pm.min_spare_servers**
Menentukan jumlah minimum pekerja PHP yang tetap berjalan saat server berjalan. Ini membantu menjaga agar server tetap responsif terhadap permintaan bahkan saat lalu lintas rendah.

**pm.max_spare_servers** Menentukan jumlah maksimum pekerja PHP yang dapat berjalan tetapi tidak menangani permintaan. Jumlah ini disesuaikan dengan kebutuhan untuk menangani lonjakan lalu lintas tanpa menambahkan terlalu banyak sumber daya ketika beban rendah.

Akan ada 4 konfigurasi terhadap proses ``package manager`` pada masing-masing worker yang nantinya akan dilakukan untuk testing.

### Script

**Script 1**
```sh
# Setup Awal
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

**Script 2**
```sh
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 25
pm.start_servers = 5
pm.min_spare_servers = 3
pm.max_spare_servers = 10' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

**Script 3**
```sh
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 50
pm.start_servers = 8
pm.min_spare_servers = 5
pm.max_spare_servers = 15' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

**Script 4**
```sh
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

### Result

**Script 1**

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176097342680739860/image.png?ex=656da101&is=655b2c01&hm=f516da183fdfef03c1562e5e80f152829af06f57337ebaef156793826ec15e6d&)

**Script 2**

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176097402646700163/image.png?ex=656da10f&is=655b2c0f&hm=7583c5ec86c57a6c848c57dfb0503c08ee2d63feb7fa2a54da7672e4ed93e52b&)

**Script 3**

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176097486717337620/image.png?ex=656da123&is=655b2c23&hm=06a6fe11c35c5fe377e60b7f7f5ffbc8c3d89734d5c5eb4ac3a01c4f263c37a9&)


## Soal 20
> Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second. (20)

Karena proses yang telah di ``konfigurasi`` sebelumnya pada masing-masing worker tepatnya pada ``package manager`` dan ternyata hasil yang diberikan juga tidak cukup untuk meningkatkan performa ``worker``. Oleh karena itu, ditambahkan ``algoritma`` pada ``load balancer`` tersebut dengan menggunakan ``Least-connection`` dimana algoritma ini akan melakukan prioritas pembagian dari beban kinerja yang paling rendah. Node master akan mencatat semua beban dan kinerja dari semua node, dan akan melakukan prioritas dari beban yang paling rendah. Sehingga diharapkan tidak ada server dengan beban yang rendah.

### Script
```sh
echo 'upstream worker {
    least_conn;
    server 192.247.4.1:8001;
    server 192.247.4.2:8002;
    server 192.247.4.3:8003;
}

server {
    listen 80;
    server_name riegel.canyon.IT28.com www.riegel.canyon.IT28.com;

    location / {
        proxy_pass http://worker;
    }
} 
' > /etc/nginx/sites-available/laravel-worker

service nginx restart
```

**Notes** 
Disini kami masih menggunakan ``setup`` pada ``package manager`` sebagai berikut 

```sh
pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20
```

### Result
Jika ditambahkan Algoritma ``Load Balancing Least-connection``. Hasil yang didapatkan cukup ``signifikan`` sebagai berikut 

![image](https://cdn.discordapp.com/attachments/897449980896346166/1176097584369115136/image.png?ex=656da13a&is=655b2c3a&hm=becceed1c2d4687eaa8a035cf79bac20fa67c4e21d6eabd5893f4f5096d17021&)

Dapat disimpulkan bahwa algoritma ``Least-connection`` dapat berkerja dengan baik
