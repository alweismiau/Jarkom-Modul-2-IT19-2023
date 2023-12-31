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
- Kemudian `restart bind9`
```
service bind9 restart
```
### Pada Sadewa
- Uji menggunakan `ping rjp.baratayuda.abimanyu.it19.com -c 2` dan `ping www.rjp.baratayuda.abimanyu.it19.com -c 2`
![uji](https://cdn.discordapp.com/attachments/1025213238763327683/1162370648006479922/image.png?ex=653bb106&is=65293c06&hm=dc5370eafb9faf31db3ba4c2c9e903e583543522915de218894ffc3b14e2add3&)

# Soal 9 & 10
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Praktikan melakukan deployment pada masing-masing worker dengan algoritma Round Robin pada load balancer.
## Jawaban
### Pada load balancer arjuna
- Dilakukan konfigurasi pada node Arjuna dengan membuat file baru di `/etc/nginx/sites-available/arjuna.it19.com`, dengan berisikan:
![arjuna-lb](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/72d2418e-1341-4f9a-88cb-7ae1190565ed)

- 3 IP yang terpasangkan pada blok ```upstream workers``` adalah IP dari Prabukusuma, Abimanyu, Wisanggeni secara berurutan
- Setelah config, hapus file default pada `/etc/nginx/sites-enabled/` agar tidak ter redirect ke page default nginx, serta dibentuk symlink dari file `/etc/nginx/sites-available/arjuna.it19.com` ke folder `/etc/nginx/sites-enabled/`.
- Sesudah config, restart kembali nginx dengan 
```
service nginx restart
``` 

### Pada worker Prabukusuma, Abimanyu, Wisanggeni
- Download resource index.php yang telah disediakan di soal, extract dan pindahkan ke folder `/var/www/arjuna.it19` dengan sejumlah command berikut: 

```
wget -O arjuna.zip --no-check-certificate -r 'https://drive.google.com/uc?export=download&id=17tAM_XDKYWDvF-JJix1x7txvTBEax7vX'

unzip arjuna.zip
rm arjuna.zip # hapus zip
cp /arjuna.yyy.com/index.php /var/www/arjuna.it19/index.php
```

- Buatlah config dalam masing-masing node worker di file dengan file `/etc/nginx/sites-available/default` sebagai template, copy dengan nama `/etc/nginx/sites-available/arjuna.it19`

- Ganti `root` dengan folder tempat `index.php` yang telah diekstrak tadi sebagai value, dan ubah value angka `listen` di awal file config dengan angka yang sesuai untuk tiap worker (Prabukusuma 8001, Abimanyu 8002, Wisanggeni 8003), dalam case ini menggunakan contoh config file dari node Abimanyu
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/a75ec3ec-0129-4a16-81df-d25f0f077c26)

- Setup pada `/var/www/arjuna.it19/index.php`
![setup](https://cdn.discordapp.com/attachments/1025213238763327683/1163405513179017347/image.png?ex=653f74d1&is=652cffd1&hm=4fc2160aa359e7b8f4514803fd03bc356a57118d1bddf601e12b25e95a8c1b13&)

### Pada Sadewa 
- Cek dengan `lynx http://IP yang dituju:800(angka)` 
Pada Prabukusuma `lynx http://10.73.4.2:8001`
![prabukusuma](https://cdn.discordapp.com/attachments/1025213238763327683/1163406326412611676/image.png?ex=653f7593&is=652d0093&hm=2847b79bd2e5b330a96588e87cb0562c407a9019bdfd6b474ff6e7a02b89d8b8&)

Pada Abimanyu `lynx http://10.73.4.3:8002`
![abimanyu](https://cdn.discordapp.com/attachments/1025213238763327683/1163407281690517584/image.png?ex=653f7677&is=652d0177&hm=937b4f639cd55404e3bb27902aa55123b4c35277716543c096123cc07a8e97ea&)

Pada Wisanggeni `lynx http://10.73.4.4:8003`
![wisanggeni](https://cdn.discordapp.com/attachments/1025213238763327683/1163408869368807535/image.png?ex=653f77f1&is=652d02f1&hm=47ecc7b44250e545310bbec839269c46abb04902931614d2b50c9a5eb6b43e21&)

# Soal 11
Praktikan melakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com.
## Jawaban
### Pada Abimanyu
- Download resource pada soal, yang dapat dilakukan dengan set command berikut:
```
wget -O abimanyu.zip --no-check-certificate -r 'https://drive.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc' # Download dari google drive resource
unzip abimanyu.zip # Unzip file
rm abimanyu.zip # Hapus file jika sudah unzip

cp -r /abimanyu.yyy.com/* /var/www/abimanyu.it19/ # Copy apapun yang ada di dalam folder /abimanyu.yyy.com/ (dengan wildcard *) ke dalam folder /var/www/abimanyu.it19/
```
- Membuat file config pada `/etc/apache2/sites-available/abimanyu.it19.com.conf`. Dengan menggunakan file 000-default.conf sebagai template, lalu dimodifikasi dengan beberapa config tambahan sebagai berikut
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/964d68a0-4ca1-4bd6-8bf5-f3cca1cbd7fc)

- Hasil jika konfigurasi benar dan berhasil dapat dicek dengan `lynx http://abimanyu.it19.com` : 
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/57244010-e021-43fd-a2b9-97e64936a089)
atau `curl http://abimanyu.it19.com`:
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/33c57573-c9ef-413e-a391-200b383e67fd)

### Pada Sadewa
- Uji menggunakan `lynx http://www.abimanyu.it19.com`
![uji](https://cdn.discordapp.com/attachments/1025213238763327683/1163422619010334782/image.png?ex=653f84bf&is=652d0fbf&hm=11254bc2065a9fb22ff9a57aba7a07ea3cf7c4ad99784cafc168b244d8cd43bd&)

# Soal 12
Praktikan perlu mengubah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.
## Jawaban
### Pada Abimanyu
- Setup `/etc/apache2/sites-available/abimanyu.it19.com.conf`
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/964d68a0-4ca1-4bd6-8bf5-f3cca1cbd7fc)
Value Alias diatas menggantikan path `/index.php/home` untuk mengakses `home.html` yang ada di folder `/var/www/abimanyu.it19/`. 

### Pada Sadewa
- Uji menggunakan `lynx http://www.abimanyu.it19.com/home`
![uji](https://cdn.discordapp.com/attachments/1025213238763327683/1163432462207168605/image.png?ex=653f8dea&is=652d18ea&hm=5d9705af6cdc061509aa1cb3c9f6e7a2461c927f2ef3e5b7d9f29dc3cc26f446&)

# Soal 13
Pada subdomain www.parikesit.abimanyu.yyy.com, praktikan menyimpan DocumentRoot pada /var/www/parikesit.abimanyu.yyy.
## Jawaban
### Pada Abimanyu
- Download dari resource yang telah disediakan, dengan set command berikut:
```
wget -O parikesit.abimanyu.zip --no-check-certificate -r 'https://drive.google.com/uc?export=download&id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS'
unzip parikesit.abimanyu.zip
rm parikesit.abimanyu.zip

cp -r /parikesit.abimanyu.yyy.com/error /var/www/parikesit.abimanyu.it19/error
cp -r /parikesit.abimanyu.yyy.com/public /var/www/parikesit.abimanyu.it19/public
```
- Salin apapun yang ada dalam folder `parikesit.abimanyu.yyy.com/error` dan `parikesit.abimanyu.yyy.com/public` ke folder root document sebagai entrypoint ketika mengakses parikesit.abimanyu.it19.com menggunakan salinan `abimanyu.it19.com.conf` untuk membuat config `parikesit.abimanyu.it19.com`
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/c8dbef2f-145b-48d5-8cc1-7c1a10b9832f)
- Dalam gambar, value `DocumentRoot` diubah pathnya menjadi `/var/www/parikesit.abimanyu.it19`. 
- Kemudian `restart apache2`
```
service apache2 restart
```

### Pada Sadewa
- Uji dengan `lynx http://www.parikesit.abimanyu.it19.com`
![uji](https://cdn.discordapp.com/attachments/1025213238763327683/1163434445144064060/image.png?ex=653f8fc3&is=652d1ac3&hm=674e7d19a9796c33cac528d5c26fa748fd591a0ba15ac0965aec7855fa8c44e4&)

# Soal 14
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses .
## Jawaban
### Pada Abimanyu
- Setup `/etc/apache2/sites-available/parikesit.abimanyu.it19.com.conf`
![abimanyu](https://cdn.discordapp.com/attachments/1025213238763327683/1163436679474974760/image.png?ex=653f91d7&is=652d1cd7&hm=2e7ef982e8f25a80b0a6d90762f1a0283bb2333016a23479f316ce34aa10c9eb&)
- Kemudian `restart apache2`
```
service apache2 restart
```
- Sedangkan untuk folder /secret ditambahkan beberapa opsi:
	
	`Options +Indexes`

	`AllowOverride none`  
	
	`Require all denied`
	untuk mematikan akses menuju folder tersebut.

- Karena dalam resource tidak disediakan folder /sercet, maka dilakukan `mkdir /var/www/parikesit.abimanyu.it19/secret`

### Pada Sadewa
- Uji `lynx http://parikesit.abimanyu.it19.com/public`
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/45fc032f-dc39-44de-b671-896270533d2c)

- Jika dicoba untuk mengakses /secret pada website, akan muncul:
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/13d61c99-b483-4303-bbdc-d8d028ae3659)
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/067fc07c-e4f8-4520-91e0-9e4a7bfc4524)

# Soal 15
Praktikan membuat kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.
## Jawaban
### Pada abimanyu
- Setup `error`
![setup](https://cdn.discordapp.com/attachments/1025213238763327683/1163448182076887040/image.png?ex=653f9c8e&is=652d278e&hm=5b2b4056e887ef13cb773f749b4962ee2ec8bfb0f94ff9983766d54e75820563&)

### Pada Sadewa
- Melihat gambar nomor 13, terdapat 2 line `ErrorDocument` yang menetapkan untuk `http error code 404` diarahkan ke `404.html` dan `http error code 403` ke `403.html`.

- Jika mencoba untuk mengakses path terlarang maka akan muncul
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/45fa53bf-2c36-479f-bde1-8f38a5231176)

![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/95de15af-201b-4c7f-8722-cd94bc4e6503)

-  Jika dicoba untuk mengakses path sembarang, akan muncul
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/b625dc2c-5a68-47f4-88f9-856d88432e3a)

![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/1593ab5a-44de-4cf5-ac25-d1cb5995ea7b)

# Soal 16 
Praktikan membuat suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js.
## Jawaban
### Pada Abimanyu
- Tambahkan code berikut: `Alias /js /var/www/parikesit.abimanyu.it03/public/js`.
- Kembali melihat ke gambar nomor 13, terdapat blok `Alias` yang meneruskan client ke folder `/public/js` jika mengakses `www.parikesit.abimanyu.it19.com/js`

### Pada Sadewa
- Uji dengan `lynx http://www.parikesit.abimanyu.it19.com/js` ![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/e2ce3f36-ede8-4b63-b32c-3db93eba88f4)

# Soal 17
Praktikan membuat konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400 agar aman.
## Jawaban
### Pada Abimanyu
- Lakukan konfigurasi pada `/etc/apache2/sites-available/rjp.baratayuda.abimanyu.it19.com.conf`![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/31dabe50-cd16-426b-af39-64405190afd8) dengan menggantikan `*:80` di tag `VirtualHost` menjadi `*:14000` dan `*:14400`
- Lalu pada file `/etc/apache2/ports.conf`, tambahkan port yang akan didengar dengan menambahkan Listen dengan `port 14000` dan `14400` seperti berikut: ![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/caa3dc86-4f6e-42e9-bfda-2d9cf06e349c)

### Pada Sadewa
- Uji dengan `lynx http://www.rjp.baratayuda.abimanyu.it19.com` ![ujirjp](https://cdn.discordapp.com/attachments/1025213238763327683/1163474785771204688/image.png?ex=653fb555&is=652d4055&hm=858fb26b47271af50e773af3f338a944b69ee4c0e3de437c468a0acecb4e83fb&)
- `lynx http://www.rjp.baratayuda.abimanyu.it19.com:14000`

# Soal 18
Untuk mengaksesnya praktikan perlu membuat autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.
## Jawaban
### Pada Abimanyu
Dijalankan command berikut untuk membuat `file .htpasswd-rjp` yang nantinya akan digunakan untuk basic authentication saat mengakses `http://rjp.baratayuda.abimanyu.it19.com`
```
htpasswd -c /etc/apache2/.htpasswd-rjp Wayang
``` 
Ini akan membuat file autentikasi basic dengan username "Wayang". Setelahnya akan ditanyakan password yang akan digunakan:
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/c6fb9942-55ca-4310-921a-f4e92c0b52a2)

- Jika sudah, melihat kembali gambar pada nomor 17. Pada blok konfigurasi directory terdapat AuthUserFile yang mengarah ke file `.htpasswd-rjp` yang telah dibuat sebelumnya. Dengan ini, sistem autentikasi sudah terpasangkan dan fungsional:
![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/1dac1d99-a01a-4e59-9ddc-f4909765be34)

![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/5195a951-d8d8-422d-9464-48c9a5e5c589)

# Soal 19
Praktikan membuat agar setiap kali mengakses IP dari Abimanyu dalam kasus ini 10.73.4.3 akan secara otomatis dialihkan ke www.abimanyu.yyy.com.
## Jawaban
### Pada Abimanyu
- Mengubah konfigurasi `/etc/apache2/sites-available/abimanyu.it19.com.conf`, dengan menambahkan `ServerAlias (IP)`. ![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/b39e25fe-9a52-4abf-a5ce-9cdc7e1a9b97)

- Jika dilakukan curl akan terlihat html yang tertera seperti pada nomor 11: ![image](https://github.com/alweismiau/Jarkom-Modul-2-IT19-2023/assets/112788819/b2b2a187-d6d8-4698-8479-44bd216be6ad)

### Pada Sadewa
- Uji dengan lynx http://10.73.4.3 ![uhji](https://cdn.discordapp.com/attachments/1025213238763327683/1163482213623541770/image.png?ex=653fbc40&is=652d4740&hm=19159a4ff3170c06eda5a81d50223e7c4949069298df14e3739195f9a109a20c&)

# Soal 20
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka praktikan perlu mengubah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

## Jawaban
### Pada Abimanyu
- Tambahkan code dibawah ini pada `/etc/apache2/sites-available/parikesit.abimanyu.it19.com.conf`. 
```
RewriteEngine On

    RewriteCond %{REQUEST_URI} abimanyu [NC]

    RewriteRule (.*) /public/images/abimanyu.png [L]
```
### Pada Sadewa
- Uji dengan `lynx http://www.parikesit.abimanyu.it19.com/aksdjabimanyu` 
![hasil](https://cdn.discordapp.com/attachments/1025213238763327683/1163391886950666371/image.png?ex=653f6820&is=652cf320&hm=4e684ca9d07e082b1a84ab3247cb71736d12878aff1305c9516b103c761b0843&)
