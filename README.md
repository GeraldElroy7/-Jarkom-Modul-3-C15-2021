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




