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

    2. ...

- Jipangu adalah DHCP Server

    Untuk menjadikan Jipangu sebagai DHCP Server:

    1. 

- Maingate dan Jorge adalah Web Server
- Jumlah Host pada Blueno adalah 100 host
- Jumlah Host pada Cipher adalah 700 host
- Jumlah Host pada Elena adalah 300 host
- Jumlah Host pada Fukurou adalah 200 host

## **B. Subnetting**

Karena kalian telah belajar subnetting dan routing, Luffy ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. setelah melakukan subnetting.

Pada praktikum kali ini digunakan metode subnetting VLSM. Pembagian subnet ada pada gambar di bawah:

![subnet](./images/subnet.png)

Kemudian dilakukan perhitungan dengan tree untuk mendapatkan NID masing-masing subnet:

![tree](./images/tree.png)

## **C. Routing**

Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

## **D. DHCP Server dan DHCP Relay**

Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

## **Soal 1**

Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

## **Soal 2**
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

## **Soal 3**
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

## **Soal 4**	
Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

## **Soal 5**
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya. Selain itu di reject.

## **Soal 6**
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.
