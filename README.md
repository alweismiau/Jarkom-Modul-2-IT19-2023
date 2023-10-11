# Jarkom-Modul-2-IT19-2023
## Kelompok IT19
- Fina Keiza Arismana       5027211028
- Rifqi Akhmad Maulana      502721035

# Soal 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Praktikan membuat topologi dalam link drive **no 3**. 
## Jawaban
Berikut adalah topologi dari topologi 3 : 
![soal1](https://cdn.discordapp.com/attachments/1025213238763327683/1161721965443297321/image.png?ex=653954e4&is=6526dfe4&hm=e765a4e0bbf9f176606c094d7e8ea36c909dde5ef636dffc888a9b12e38e5d1d&)

Kemudian pada router Pandudewanata jalankan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.73.0.0/16`. Dan pada node ubuntu lainnya (Yudhistira, dll) jalankan `echo nameserver 192.168.122.1 > /etc/resolv.conf`.

Kemudian **restart seluruh node** dan lakukan **ping google.com**. 
![ping](https://cdn.discordapp.com/attachments/1025213238763327683/1161724906082410536/image.png?ex=653957a1&is=6526e2a1&hm=4a0706a2587efd231961e81588eb2bc12b88acd970bc73483bfe2cc7066aad1a&)

# Soal 2
Praktikan membuat website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok yaitu **IT19**.
## Jawaban
