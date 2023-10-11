# Jarkom-Modul-2-IT19-2023
## Kelompok IT19
- Fina Keiza Arismana       5027211028
- Rifqi Akhmad Maulana      502721035

# Soal 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Membuat topologi no 3. 
## Jawaban
Berikut adalah topologi dari topologi 3 : 
![soal1](https://discord.com/channels/1025213238251630644/1025213238763327683/1161721965665591426)

Kemudian pada router Pandudewanata jalankan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.73.0.0/16`. Dan pada node ubuntu lainnya (Yudhistira, dll) jalankan `echo nameserver 192.168.122.1 > /etc/resolv.conf`.

Kemudian **restart seluruh node** dan lakukan **ping google.com**. 
![ping](https://discord.com/channels/1025213238251630644/1025213238763327683/1161724906300522577)
