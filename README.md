# Jarkom-Modul-3-C15-2021

# Nomor 8

Pada Loguetown, proxy harus bisa diakses dengan nama `jualbelikapal.yyy.com` dengan port yang digunakan adalah 5000.

#### Langkah pengerjaan:

### Water7

- Install terlebih dahulu aplikasi **squid** dan **apache2-utils**
  ```
  apt-get install squid -y
  apt-get install apache2-utils -y
  ```
- Buat konfigurasi pada folder Squid
  ```
  vim /etc/squid/squid.conf
  ```
- Lakukan edit pada file tersebut seperti di bawah ini:

![image](https://user-images.githubusercontent.com/64303057/141612029-ce6f468b-f15a-4454-ba11-a038698eea4b.png)


- Tambahkan juga IP EniesLobby pada `/etc/resolv.conf` seperti berikut:


![image](https://user-images.githubusercontent.com/64303057/141612056-13100407-eba9-4108-b8f4-091634bd2b83.png)


- Restart squid dengan `service squid restart`

### Loguetown

- Install aplikasi **lynx**
`
apt-get install lynx -y
`
- Aktifkan proxy dengan menambahkan syntax pada terminal

`
export http_proxy="http://192.191.2.3:5000"
`
- Buka `its.ac.id` dengan lynx

![image](https://user-images.githubusercontent.com/64303057/141612665-119bf0fa-b2de-4ef7-b6a2-eaf649ee82dd.png)

# Nomor 9

Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu `luffybelikapalyyy` dengan password `luffy_yyy` dan `zorobelikapalyyy` dengan password `zoro_yyy`.

#### Langkah pengerjaan:

### Water7

- Kita perlu membuat autentikasi baru untuk username `luffybelikapalc15` dan `zorobelikapalc15` dengan syntax seperti ini :


```
htpasswd -cm /etc/squid/passwd luffybelikapalc15
htpasswd -m /etc/squid/passwd zorobelikapalc15
```

`-cm` memiliki arti create new file dengan enkripsi MD5 dan `-m` untuk membuat user baru di file tersebut.

- Password untuk `luffybelikapalc15` adalah `luffy_c15`, serta `zorobelikapalc15` adalah `zoro_c15`

- Kita dapat cek apakah username & password sudah tersimpan pada `/etc/squid/passwd`.

- Edit file `squid.conf` pada `/etc/squid` menjadi:

![image](https://user-images.githubusercontent.com/64303057/141613260-19768470-32c6-4f7d-9648-f0a7ada3fc1e.png)

- Jangan lupa untuk `service squid restart` untuk menggunakan konfigurasi yang baru.

### Loguetown

- Kembali export proxy dan lakukan command `lynx its.ac.id`
- Masukkan username & password sebagai berikut (contoh pada luffy) :


![image](https://user-images.githubusercontent.com/64303057/141613549-58b2fdb2-cef4-4050-b3f1-2225936433db.png)


![image](https://user-images.githubusercontent.com/64303057/141613555-7c048c3a-fff2-4530-bc4f-0cc2f48fbac8.png)


Hasil lynx :

![image](https://user-images.githubusercontent.com/64303057/141613585-23e2949a-4c7d-4c7d-b680-99a034592a3a.png)


# Nomor 10

Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00).

#### Langkah pengerjaan:

### Water7

- Buatlah pembatasan waktu akses transaksi jual beli pada file `acl.conf` di directory `/etc/squid`. Tambahkan sebagai berikut:

![image](https://user-images.githubusercontent.com/64303057/141615171-5e8c594a-9d12-43a7-8b6e-570a71e292b1.png)

Syntax di atas berguna untuk menyesuaikan persyaratan yang diminta pada soal. Setiap `AVAILABLE_WORKING` menunjukkan waktu akses proxy yang dapat digunakan. `AVAILABLE_WORKING` menunjukkan pada hari Senin - Kamis pukul 07.00 - 11.00. `AVAILABLE_WORKING1` menunjukkan pada hari Selasa - Jumat pukul 17.00 - 24.00. `AVAILABLE_WORKING2` menunjukkan hari Rabu - Sabtu pukul 24.00 - 03.00.

- Edit juga file `squid.conf` di directory `/etc/squid` seperti di bawah ini:

```
http_port 5000
visible_hostname jualbelikapal.c15.com

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

include /etc/squid/acl.conf
http_access allow AVAILABLE_WORKING USERS
http_access allow AVAILABLE_WORKING1 USERS
http_access allow AVAILABLE_WORKING2 USERS
http_access deny all
```

- Restart kembali squid dengan `service squid restart`.

### Loguetown

- Export kembali proxy pada command dan set *date* di Loguetown `date -s "10 NOV 2021 10:00"`. Lanjutkan dengan lynx ke situs `its.ac.id`.

![image](https://user-images.githubusercontent.com/64303057/141620546-bd0d0cb6-a161-406d-93e0-a8af9754a3be.png)

Hasil lynx (situs bisa dikunjungi):

![image](https://user-images.githubusercontent.com/64303057/141621581-da53df01-3bb2-4be6-a84d-f17ce7366c91.png)

- Coba juga apabila *date*-nya diubah ke jadwal yang tidak bisa diakses. Contoh tuliskan command `date -s "10 NOV 2021 12:00"`. Lanjutkan dengan lynx ke situs `its.ac.id`.

![image](https://user-images.githubusercontent.com/64303057/141622874-76bd373f-43a2-45dc-a5df-cc56cf0764d6.png)

Hasil lynx (situs tidak bisa dikunjungi):

![image](https://user-images.githubusercontent.com/64303057/141624118-41a29bea-ac49-4bba-8531-745fb54039e1.png)

# Nomor 11

Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju `super.franky.yyy.com` dengan website yang sama pada soal shift modul 2. Web server `super.franky.yyy.com` berada pada node Skypie.

#### Langkah pengerjaan:

### EniesLobby

- Edit file `/etc/bind/named.conf.local` seperti pada gambar berikut:

![image](https://user-images.githubusercontent.com/64303057/141630513-9b584683-8c31-465a-ad1e-7e6cc6ae6166.png)

- Buat folder baru bernama `kaizoku` pada direktori `/etc/bind` dengan command `mkdir /etc/bind/kaizoku`.

- Copy juga file `db.local` yang ada pada `/et/bind` ke dalam folder kaizoku, dan ubah namanya menjadi `super.franky.c15.com`

```
cp /etc/bind/db.local /etc/bind/kaizoku/super.franky.c15.com
```

- Edit file yang baru di-copy tadi sebagai berikut :

![image](https://user-images.githubusercontent.com/64303057/141633869-ff6388b7-1639-4b22-a9d4-060f2b755d72.png)

- Jangan lupa untuk restart bind9 dengan command `service bind9 restart`.

### Skypie

- Copy file `000-default.conf` yang ada pada `sites-available` ke file `super.franky.c15.com.conf` dengan command :

```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/super.franky.c15.com.conf`
```

- Lakukan edit pada file `super.franky.c15.com.conf` menjadi :

![image](https://user-images.githubusercontent.com/64303057/141636665-e1236bde-d984-4dc6-bac6-fdcd025295e2.png)

- Aktifkan konfigurasi dengan `a2ensite super.franky.c15.com` serta restart apache `service apache2 restart`.

- Ketik `cd` dan pindah ke directory `/var/www` dengan command `cd /var/www`.

- Download file zip dengan command :

 ```
 wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.franky.zip
 ```
 
 serta lakukan unzip `unzip super.franky.zip`
 
 - Pindahkan file `error` & `public` ke `super.franky.c15.com` seperti berikut :
 
```
cp -r /root/super.franky/error /var/www/super.franky.E08.com
cp -r /root/super.franky/public /var/www/super.franky.E08.com
```

### Water7

- Kembali ke file `/etc/squid/squid.conf` dan tambahkan sebagai berikut:

```
acl BLACKLIST dstdomain .google.com
deny_info http://super.franky.c15.com/ BLACKLIST
http_access deny AVAILABLE_WORKING BLACKLIST
```

dan jalankan command `service squid restart` untuk menggunakan konfigurasi squid yang baru.

### Loguetown

Berikut hasil `lynx google.com` yang akan melakukan *redirect* menuju `super.franky.c15.com`:

![2021-11-13 (11)](https://user-images.githubusercontent.com/64303057/141642161-2a07cf39-7878-4387-81ed-8ed5b6bd9265.png)

![image](https://user-images.githubusercontent.com/64303057/141642121-e83b4620-3401-4aeb-99f9-2331814dfa98.png)


`










