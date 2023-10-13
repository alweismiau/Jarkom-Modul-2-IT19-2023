# Jarkom-Modul-2-IT19-2023
## Kelompok IT19
- Fina Keiza Arismana       5027211028
- Rifqi Akhmad Maulana      502721035

# Soal 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Membuat topologi no 3. 
## Jawaban
Berikut adalah topologi dari topologi 3 : 
![soal1](https://cdn.discordapp.com/attachments/1025213238763327683/1161721965443297321/image.png?ex=653954e4&is=6526dfe4&hm=e765a4e0bbf9f176606c094d7e8ea36c909dde5ef636dffc888a9b12e38e5d1d&)

Kemudian pada router Pandudewanata jalankan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.73.0.0/16`. Kemudian lakukan `cat /etc/resolv.conf`  untuk mendapatkan ip dari *nameserver x.x.x.x*. Kemudian pada node ubuntu lainnya (Yudhistira, dll) jalankan `echo nameserver 192.168.122.1 > /etc/resolv.conf`.

Kemudian **restart seluruh node** dan lakukan **ping google.com**. 
![ping](https://cdn.discordapp.com/attachments/1025213238763327683/1161724906082410536/image.png?ex=653957a1&is=6526e2a1&hm=4a0706a2587efd231961e81588eb2bc12b88acd970bc73483bfe2cc7066aad1a&)

# Soal 2 dan 3
Praktikan membuat website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok yaitu IT19. Pada soal 3 juga demikian tetapi memiliki askes ke abimanyu.yyy.com
## Jawaban
### Pada Yudhistira
#### Setup Domain Name di Master DNS Server
- Setup `/etc/bind/named.conf.local`
```
zone “arjuna.it19.com” {
	type master;
	file “/etc/bind/arjuna/arjuna.it19.com";
};

zone “abimanyu.it19.com” {
	type master;
	file “/etc/bind/abimanyu/abimanyu.it19.com";
};
```
- Membuat direktori baru bernama `arjuna` dan `abimanyu`
```
mkdir /etc/bind/arjuna
mkdir /etc/bind/abimanyu
```
- Lakukan copy `/etc/bind/db.local` ke dalam direktori `arjuna` dan ubah namanya menjadi `arjuna.it19.com`. Hal itu juga berlaku pada abimanyu.
```
cp /etc/bind/db.local /etc/bind/arjuna/arjuna.it19.com
cp /etc/bind/db.local /etc/bind/arjuna/abimanyu.it19.com
```
- Setup pada `/etc/bind/arjuna/arjuna.it19.com` begitu juga pada abimanyu pada `/etc/bind/arjuna/abimanyu.it19.com`, perlu diperhatikan mengubah nama dan IP masing-masing. 
```
$TTL	604800		
@	IN	SOA	arjuna.it19.com.	root.arjuna.it19.com. (
			2023101101		; Serial
			604800			; Refresh
			86400			; Retry
			2419200			; Expire
			604800	)		; Negative Cache TTL
;
@	IN	NS	arjuna.it19.com.
@	IN	A	10.73.4.5		; IP Arjuna-LB
www	IN	CNAME	arjuna.it19.com.
@	IN	AAAA	::1
```
![arjuna.it19.com](https://cdn.discordapp.com/attachments/1025213238763327683/1161876127422361650/image.png?ex=6539e477&is=65276f77&hm=6bbd9731611b27567ca85829cf86827f3a664ccd68798a590ab6cedb5c83ee3a&)
![abimanyu.it19.com](https://cdn.discordapp.com/attachments/1025213238763327683/1161902991528443925/image.png?ex=6539fd7c&is=6527887c&hm=3187a2c0bb28cd1576b3ea778edc6bd7a6930a45e7a14e48d22d358523613d9d&)
- Tambahkan code diatas pada `.bashrc`
- Lakukan restart bind9 
```
service bind9 restart	
```

### Pada Arjuna
- Setup `/etc/resolv.conf` dengan IP dari Yudhistira
![arjuna](https://cdn.discordapp.com/attachments/1025213238763327683/1161876998059204698/image.png?ex=6539e546&is=65277046&hm=cf782e210772144d4f40749883f75903e6c7ca66f543fc1a697a6723b08a0afc&)
- Lakukan test dengan `ping arjuna.it19.com` & `ping www.arjuna.it19.com`
![ping arjuna](https://cdn.discordapp.com/attachments/1025213238763327683/1161878181293338796/image.png?ex=6539e661&is=65277161&hm=9725e437816651e78c8849381e37078620cdf2ad46f3cbe2a2b2a92f3759696e&)

### Pada Abimanyu
- Setup `/etc/resolv.conf` dengan IP dari Yudhistira
![abimanyu](https://cdn.discordapp.com/attachments/1025213238763327683/1161876998059204698/image.png?ex=6539e546&is=65277046&hm=cf782e210772144d4f40749883f75903e6c7ca66f543fc1a697a6723b08a0afc&)
- Lakukan test dengan `ping abimanyu.it19.com` & `ping www.abimanyu.it19.com`
![ping abimanyu](https://cdn.discordapp.com/attachments/1025213238763327683/1161897116768149504/image.png?ex=6539f803&is=65278303&hm=ed3c71492b9ec8ae052a4e96edc2fb3f9c366edb4391e1b6c388e6863a6559ae&)

# Soal 4
Praktikan membuat subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu. 
## Jawaban
### Pada Yudhistira
- Setup `/etc/bind/abimanyu/abimanyu.it19.com` dengan menambahkan IP untuk **parikesit**. Sesuaikan dengan ip sebelumnya yang terhubung dengan abimanyu. 
![abimanyu.it19.com](https://cdn.discordapp.com/attachments/1025213238763327683/1161896579544911964/image.png?ex=6539f783&is=65278283&hm=c698469a4f854e911508f80e949c7dd4f65a281d33ae6aabacfea1a197354a56&)
- Lakukan restart bind9 
```
service bind9 restart	
```

### Pada Abimanyu
- Uji dengan `ping parikesit.abimanyu.it19.com`
![hasil subdomain](https://cdn.discordapp.com/attachments/1025213238763327683/1161900858984906832/image.png?ex=6539fb7f&is=6527867f&hm=2a556170c54e8ca27bb3f24ec527cb3861fccea66c25f35ffacfa3aac7c331bd&)

# Soal 5 
Praktikan membuat reverse domain untuk domain utama yaitu abimanyu.
## Jawaban
### Pada Yudhistira
- Setup `/etc/bind/named.conf.local`
![reverse](https://cdn.discordapp.com/attachments/1025213238763327683/1161912080954302504/image.png?ex=653a05f3&is=652790f3&hm=1068a9c4c2a36f577d08317263fa9d607961719ff857a256c1088a7844cee9b1&)
Untuk `4.73.10` didapatkan dari reverse IP abimanyu. 
- Melakukan copy `db.local` ke `abimanyu`
```
cp /etc/bind/db.local /etc/bind/abimanyu/4.73.10.in-addr.arpa
```	
- Setup `/etc/bind/abimanyu/4.73.10.in-addr.arpa`
![reverse](https://cdn.discordapp.com/attachments/1025213238763327683/1161915726404263986/image.png?ex=653a0958&is=65279458&hm=ced7f66e4c4170196ce748d9e89b988c7c0e1894179bc3fd7227bf7b7464097c&)

### Pada Abimanyu
- Cek dengan `host -t PTR 10.73.4.3` dimana ini merupakan IP dari Abimanyu
![cek](https://cdn.discordapp.com/attachments/1025213238763327683/1162364200107130980/image.png?ex=653bab04&is=65293604&hm=cfc9ea635c83fc12157d490d2f93218f8b8e11adc6cf5513684d5961e5b63cc4&)

# Soal 6 
Praktikan membuat Werkudara sebagai DNS Slave agar tetap dihubungi ketika DNS Server Yudhistira bermasalah.
## Jawaban
### Pada Yudhistira
- Setup `/etc/bind/named.conf.local` dengan : 
![slave](https://cdn.discordapp.com/attachments/1025213238763327683/1161933400777687060/image.png?ex=653a19ce&is=6527a4ce&hm=6b474461ea120dcdb3745a3a32c6dd06a105e89567be16171a31c5c766319da0&)
- Lakukan restart bind9 
```
service bind9 restart	
```
-  Kemudian `stop bind9`
```
service bind9 stop
```
![slave1](https://cdn.discordapp.com/attachments/1025213238763327683/1161936402389418075/image.png?ex=653a1c9a&is=6527a79a&hm=ea17e70be016c91f86ed1b44ec447bad0c7b00ef034eeb4346db4dc3745f12d6&)
### Pada Werkudara
- Setup pada `/etc/bind/named.conf.local`
![slaved](https://cdn.discordapp.com/attachments/1025213238763327683/1161940625160142899/image.png?ex=653a2088&is=6527ab88&hm=81f33d67b09cd62bfd833320d9b599a6859a78783f0e2ff14747a9563896e9d2&)
### Pada Sadewa sebagai client
- Setup `nameserver` di `/etc/resolv.conf` dengan IP Yudhistira dan IP Werkudara.
- Coba lakukan `ping abimanyu.it19.com`
![sadwea](https://media.discordapp.net/attachments/1025213238763327683/1161928958409973821/image.png?ex=653a15ab&is=6527a0ab&hm=0310e9acaf8c317ba866039baff5d6c0e845754a42e37bd3e2a489b533e40230&=&width=1057&height=258)

# Soal 7
Praktikan membuat subdomain khusus untuk perang yaitu `baratayuda.abimanyu.yyy.com` dengan alias `www.baratayuda.abimanyu.yyy.com` yang didelegasikan dari Yudhistira ke Werkudara 
## Jawaban
### Pada Yudhistira
- Setup `/etc/bind/abimanyu/baratayuda.abimanyu.it19.com`
![nsl](https://cdn.discordapp.com/attachments/1025213238763327683/1162009984188559360/image.png?ex=653a6121&is=6527ec21&hm=3e8b5243a88519c1312bb9d334fa858b36c7bfa9d24f9db58d02400e758110b7&)
- Setup untuk mengizinkan akses query pada `/etc/bind/named.conf.options`
![queri](https://cdn.discordapp.com/attachments/1025213238763327683/1162010446837075968/image.png?ex=653a618f&is=6527ec8f&hm=114da5a5a9c2c8832085798099de074fe3d94252326257cd772ddf940c5bb62a&)
-  Kemudian `restart bind9`
```
service bind9 restart
```

### Pada Werkudara
- Setup `/etc/bind/named.conf.options` sama seperti pada Yudhistira
- Buat direktori baru dan lakukan copy `db.local` ke `/etc/bind/abimanyu/baratayuda.abimanyu.it19.com`
```
mkdir /etc/bind/abimanyu
cp /etc/bind/db.local /etc/bind/abimanyu/baratayuda.abimanyu.it19.com
```
- Setup `/etc/bind/abimanyu/baratayuda.abimanyu.it19.com`
![setup](https://cdn.discordapp.com/attachments/1025213238763327683/1162025196069330954/image.png?ex=653a6f4c&is=6527fa4c&hm=82b24a60a3980ff2d116b5c4bfa5eb8d3497072b7f9572ab83e63e3936420144&)
-  Kemudian `restart bind9`
```
service bind9 restart
```

### Pada Sadewa 
- Uji menggunakan `ping baratayuda.abimanyu.it19.com` dan `www.baratayuda.abimanyu.it19.com`
![sadewa](https://media.discordapp.net/attachments/1025213238763327683/1161981196075536384/image.png?ex=653a4651&is=6527d151&hm=9dd64ce60fdcf642947208b779444adc381c5ca300eae131ea12ae75ee088d00&=&width=1057&height=579)

# Soal 8
Praktikan membuat ubdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu untuk informasi yang lebih spesifik mengenai ranjapan baratayuda.
## Jawaban
### Pada Werkudara
- Setup `/etc/bind/named.conf.local`
![setup](https://cdn.discordapp.com/attachments/1025213238763327683/1162370741828849715/image.png?ex=653bb11c&is=65293c1c&hm=287f9a6e305c3efa6f36df635139be1162e791762050fa08a3d6dd43daaabe53&)
- Kemudian membuat direktori baru
- Setup ` /etc/bind/abimanyu/rjp.baratayuda.abimanyu.it19.com`
![setup](https://cdn.discordapp.com/attachments/1025213238763327683/1162369889231708170/image.png?ex=653bb051&is=65293b51&hm=48a972da066b64ca610a836eec6c838c0a160f5a099e7ea94aa86a361ff7d88a&)
- -  Kemudian `restart bind9`
```
service bind9 restart
```
### Pada Sadewa
- Uji menggunakan `ping rjp.baratayuda.abimanyu.it19.com -c 2` dan `ping www.rjp.baratayuda.abimanyu.it19.com -c 2`
![uji](https://cdn.discordapp.com/attachments/1025213238763327683/1162370648006479922/image.png?ex=653bb106&is=65293c06&hm=dc5370eafb9faf31db3ba4c2c9e903e583543522915de218894ffc3b14e2add3&)

# Soal 9
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Praktikan melakukan deployment pada masing-masing worker.
## Jawaban

# Soal 10
Menggunakan **Round Robin** untuk Load Balancer pada Arjuna. Praktikan menggunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. 
## Jawaban

# Soal 11
Praktikan melakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com.
## Jawaban

# Soal 12
Praktikan perlu mengubah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.
## Jawaban

# Soal 13
Pada subdomain www.parikesit.abimanyu.yyy.com, praktikan menyimpan DocumentRoot pada /var/www/parikesit.abimanyu.yyy.
## Jawaban

# Soal 14
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses .
## Jawaban

# Soal 15
Praktikan membuat kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.
## Jawaban

# Soal 16 
Praktikan membuat suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js.
## Jawaban

# Soal 17
Praktikan membuat konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400 agar aman.
## Jawaban

# Soal 18
Untuk mengaksesnya praktikan perlu membuat autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.
## Jawaban

# Soal 19
Praktikan membuat agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com.
## Jawaban

# Soal 20
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka praktikan perlu mengubah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

## Jawaban
