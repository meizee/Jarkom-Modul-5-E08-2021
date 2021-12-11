# Jarkom-Modul-5-E08-2021

**Lapres Praktikum Jaringan Komputer Modul 5**

- Fais Rafii Akbar Hidiya (05111940000026)
- Zahra Dyah Meilani (05111940000069)
- Aji Rindra Fakhrezi Putra Faisal (05111940000205)

## **A. Topologi**

Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Luffy dibawah ini:

![topologi](./images/topologi.png)

Keterangan:
- Doriki adalah DNS Server

    Untuk menjadikan Doriki sebagai DNS Server: 

    1. Install bind9
     
      ```
      apt-get update
      apt-get install bind9 -y
      ```

    2. Ubah konfigurasi pada file `/etc/bind/named.conf.options` dengan uncomment forwarders dan mengubah IPnya dan tambahkan `allow-query{any;};` di isi file tersebut, isi file sebagai berikut :
        ```
        options {
            directory "/var/cache/bind";

        //  If there is a firewall between you and nameservers you want
        //  to talk to, you may need to fix the firewall to allow multiple
        //  ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        //  If your ISP provided one or more IP addresses for stable
        //  nameservers, you probably want to use them as forwarders.
        //  Uncomment the following block, and insert the addresses replacing
        //  the all-0's placeholder.

            forwarders {
                192.168.122.1;
            };
            allow-query{any;};

        //=====================================================================$
        //  If BIND logs error messages about the root key being expired,
        //  you will need to update your keys.  See https://www.isc.org/bind-keys
        //=====================================================================$
        //  dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on-v6 { any; };
        };
        ```
        
        Setelah itu restart service dns dengan menggunakan command `service bind9 restart`.
        
- Jipangu adalah DHCP Server

    Untuk menjadikan Jipangu sebagai DHCP Server:

    1. Install DHCP
      
      
        ```
        apt-get update
        apt-get install isc-dhcp-server -y
        ```
      
      
    2. Ubah konfigurasi pada file `isc-dhcp-server` pada directory `/etc/default/isc-dhcp-server` dan tambahkan `eth0` di bagian `INTERFACES=`, isi file sebagai berikut :
        ```
         Defaults for isc-dhcp-server initscript
        # sourced by /etc/init.d/isc-dhcp-server
        # installed at /etc/default/isc-dhcp-server by the maintainer scripts

        #
        # This is a POSIX shell fragment
        #

        # Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
        #DHCPD_CONF=/etc/dhcp/dhcpd.conf

        # Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
        #DHCPD_PID=/var/run/dhcpd.pid

        # Additional options to start dhcpd with.
        #       Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
        #OPTIONS=""

        # On what interfaces should the DHCP server (dhcpd) serve DHCP requests?       
        #       Separate multiple interfaces with spaces, e.g. "eth0 eth1".
        INTERFACES="eth0"
        ```
        Setelah itu restart service dhcp dengan menggunakan command `service isc-dhcp-server restart`.
        
- Maingate dan Jorge adalah Web Server
- Jumlah Host pada Blueno adalah 100 host
- Jumlah Host pada Cipher adalah 700 host
- Jumlah Host pada Elena adalah 300 host
- Jumlah Host pada Fukurou adalah 200 host

## **B. Subnetting**

Karena kalian telah belajar subnetting dan routing, Luffy ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. setelah melakukan subnetting.

Pada praktikum kali ini digunakan metode subnetting VLSM. Pembagian subnet ada pada gambar di bawah:

![VLSM](./images/VLSM.jpg)

| Subnet  | Jumlah IP | Netmask  |
| ------------- | ------------- | ------------- | 	       
| A1  | 3  | /29 |
| A2  | 101  | /25 |
| A3  | 701  | /22 |
| A4  | 2  | /30 |
| A5  | 2  | /30 |
| A6  | 301  | /23 |
| A7  | 201  | /24 |
| A8  | 3  | /29 |
| **Total**  | **1314**  | **/21** |

Kemudian dilakukan perhitungan dengan tree untuk mendapatkan NID masing-masing subnet:

![tree](./images/tree.jpg)

Hasil Perhitungan : 

![subnet](./images/subnet.jpg)

## **C. Routing**

Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

## **D. DHCP Server dan DHCP Relay**

Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

## **Soal 1**

Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

**Pembahasan:**

## **Soal 2**
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

**Pembahasan:**

## **Soal 3**
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

**Pembahasan:**

Pada node Jipangu (DHCP Server) dan Doriki (DNS Server) jalankan script berikut:

```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

Setelah itu uji dengan cara ping ke Jipangu dan Doriki dari keempat client. Pada ping keempat, maka ping tidak berhasil.

1. Ping ke Jipangu

    - Blueno

    ![3a](./images/3a.png)

    - Cipher

    ![3b](./images/3b.png)

    - Elena
  
    ![3c](./images/3c.png)

    - Fukurou

    ![3d](./images/3d.png)

2. Ping ke Doriki

    - Blueno

    ![3e](./images/3e.png)

    - Cipher

    ![3f](./images/3f.png)

    - Elena

    ![3g](./images/3g.png)

    - Fukurou

    ![3h](./images/3h.png)

## **Soal 4**	
Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

**Pembahasan:**

Pada node Doriki jalankan script berikut:

```bash
#blueno:
iptables -A INPUT -s 10.33.0.128/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.33.0.128/25 -j REJECT
```

```bash
#cipher
iptables -A INPUT -s 10.33.4.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 10.33.4.0/22 -j REJECT
```

Setelah itu uji dengan cara ping ke Doriki dari Blueno dan Cipher pada tanggal yang sesuai dan tidak sesuai.

1. Pada tanggal yang sesuai (Selasa, 7 Desember 2021 Pukul 09.00)

    ```bash
    date -s "7 DEC 2021 09:00:00"
    ```

    - Blueno

    ![4a](./images/4a.png)

    - Cipher

    ![4b](./images/4b.png)

2. Pada tanggal yang tidak sesuai (Selasa, 7 Desember 2021 Pukul 20.00)

    ```bash
    date -s "7 DEC 2021 20:00:00"
    ```

    - Blueno

    ![4c](./images/4c.png)

    - Cipher

    ![4d](./images/4d.png)


## **Soal 5**
Akses dari subnet Elena dan Fukurou hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya. Selain itu di reject.

**Pembahasan:**

Pada node Doriki jalankan script berikut:

```bash
#elena
iptables -A INPUT -s 10.33.2.0/23 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```

```bash
#fukurou
iptables -A INPUT -s 10.33.1.0/24 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```

Setelah itu uji dengan cara ping ke Doriki dari Elena dan Fukurou pada tanggal yang sesuai dan tidak sesuai.

1. Pada tanggal yang sesuai (Selasa, 7 Desember 2021 Pukul 20.00)

    ```bash
    date -s "7 DEC 2021 20:00:00"
    ```

    - Elena

    ![5a](./images/5a.png)

    - Fukurou

    ![5b](./images/5b.png)

2. Pada tanggal yang tidak sesuai (Selasa, 7 Desember 2021 Pukul 09.00)

    ```bash
    date -s "7 DEC 2021 09:00:00"
    ```

    - Elena

    ![5c](./images/5c.png)

    - Fukurou

    ![5d](./images/5d.png)

## **Soal 6**
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.

**Pembahasan:**
