<h1 align="center">TUTORIAL DEMO ETS MATAKULIAH CYBER</h1>

Create by : [Naufal Farras Wahyuntitof](https://github.com/Farrszzz)

Editor : [Bessotu Putra](https://github.com/ItokROCY)

Operating System Used in this Tutorial : [Linux Ubuntu](https://ubuntu.com/)

Virtual Environment : [VMWARE](https://www.vmware.com/)

---

## ToDo

- [x] **Studi kasus 1**
  - Defender -> Attacker
- [x] **Studi kasus 2**

  - Menampilkan rules:
    a. Filter table

    ```

    le

    le

    d. RAW table
    ```

  - Mengubah policy chain dan cek ulang

  - Menambah protokol ICMP, SSH, HTTP, HTTPS, FTP

  - Menghapus satu rules, melihat semua rules, menyimpan konfigurasi iptables

**Perlu Diperhatikan**

- Bagi linuxnya berbeda dengan yang saya pakai, kalian tinggal sesuaikan saja. Namun apabila ada error anda bisa bertanya kepada Chat GPT/OpenAI, Jangan Manja ya Gaes!!!
- lakukan sudo apt update && apt upgrade sebelum memulai apapun
- install ssh dan ftp karena sangat dibutuhkan dalam pengetesan pada studi kasus 1

---

## ssh/ftp Install & Konfigurasi

### Install dan Konfigurasi ssh

**Install**

```bash
sudo apt install openssh-server
sudo apt upgrade
```

**Konfigurasi**

```bash
sudo service ssh restart
sudo systemctl enable ssh
sudo ufw allow 22/tcp
sudo ufw enable
```

---

### Install dan Konfigurasi ftp

**Install**

```bash
sudo apt install vsftpd
sudo apt upgrade
```

**Konfigurasi**

```bash
sudo service svsftpd restart
sudo ufw allow 21/tcp
sudo ufw enable
```

---

Setelah selesai Install & Konfigurasi silahkan kalian ketikkan `ifconfig` . Lalu copy IP Address untuk persiapan studi kasus 1 dengan cara (CTRL + SHIFT + C) setelah itu buka pada windows kalian lalu cari powershell dan dibuka menggunakan Administrator. Pada Powershell ketikkan `nmap -p 22,21 -sV IP Address` lalu powershell akan scanning IP Address kalian. setelah muncul hasil SCAN pastikan ssh & ftp berstatus Open.

## Studi kasus 1

Sebelum kita melakukan fail2ban sebaiknya kita melakukan Install fail2ban.
Buka Terminal pada Linux kalian lalu ketikkan :

```bash
sudo apt update
sudo apt install fail2ban
sudo apt upgrade
```

Setelah kalian install kita buka Powershell yang sudah dibuka tadi.

Setelah itu kalian coba ketiikan `ssh root@IP Address` dan akan muncul:

```powershell
> root@IP Address password :
```

kalian bisa ketikkan password sembarang sampai salah semua. Apabila tidak muncul]
`root@IP Address password :` berarti install ssh kalian belum berhasil.
Itu semua bisa kita lakukan karena kita belum melakukan konfigurasi terhadap fail2ban.

Sekarang kita kembali ke Terminal Linux kita. sekarang kita lakukan konfigurasi dengan mengetikkan:

```bash
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
gedit /etc/fail2ban/jail.local
```

(Pada menu gedit perlu kalian perhatikan pada line 92, 101, 105, 108, dan 288. ini bisa juga kalian ubah sesuai keinginan kalian)

Selanjutnya kita mulai proses fail2ban dengan mengetikkan:

```bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
sudo systemctl status fail2ban.service
```

apabila warna hijau, berarti sudah aktif

Selanjutnya kalian coba pada Powershell dengan mengetikkan `ssh root@IP Address` maka return yang dihasilkan adalah error connection dimana itu juga bagian pencegahan oleh fail2ban.

## Studi kasus 2

sebelum kita melakukan fail2ban sebaiknya kita melakukan Install iptables.
Buka Terminal pada Linux kalian lalu ketikkan :

```bash
sudo apt update
sudo apt install iptables
sudo apt upgrade
```

Sebelum kita Menambah atau Mengubah Rules alangkah baiknya kita Tampilkan terlebih dahulu Rules-nya dengan mengetikkan:

1. Filter Table:

```bash
sudo iptables -t filter --list
```

2. Mangle Table:

```bash
sudo iptables -t mangle --list
```

3. NAT Table:

```bash
sudo iptables -t nat --list
```

4. RAW Table:

```bash
sudo iptables -t raw --list
```

Sekarang kita akan melakukan perubahan Default Policy Filter, sebelum mengubah kita lihat dulu Default policy-nya bagaimana, dengan mengetikkan :

```bash
sudo iptables -L | grep policy
```

(maka akan muncul 3 Default policy)

Selanjutnya kita akan mengubahnya dengan mengetikkan :

- `sudo iptables --policy INPUT DROP` (apabila ingin DROP bagian INPUT)
- `sudo iptables --policy FORWARD DROP` (apabila ingin DROP bagian FORWARD)
- `sudo iptables --policy OUTPUT ACCEPT` (apabila ingin ACCEPT bagian OUTPUT)

Setelah itu lihat kembali tampilan bagian policy-nya lagi. ketik :

```bash
sudo iptables --policy INPUT DROP
```

Selanjutnya kita akan Menambahkan Rules, disini kita akan menambahkan protocol icmp,ssh,http,https, dan ftp. Sebelum kita menambahkan kita akan ubah Default policy pada bagian INPUT menjadi ACCEPT. Ketik :

```bash
sudo iptables --policy INPUT ACCEPT
```

Setelah itu kita akan menambahkan semua protocol dengan mengetikkan :

- `sudo iptables -A INPUT -p icmp -j ACCEPT` (icmp)
- `sudo iptables -A INPUT -p tcp --dport 21 -j ACCEPT` (ftp)
- `sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT` (ssh)
- `sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT` (http)
- `sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT` (https)

Selanjutnya kita cek apakah kita sudah berhasil menambahkan protocol yang sudah kita setting dengan mengetikkan :

```bash
sudo iptables -L
```

Pastikan protocol yang kita tambahkan muncul semuanya. Selanjutnya kita akan mencoba untuk menghapus protocol semisal protocol https/tcp 443 dengan mengetikkan :

```bash
sudo iptables -D INPUT tcp --dport 443 -j ACCEPT
```

cek lagi dengan mengetikkan :

```bash
sudo iptables -L
```

Pastikan protocol HTTPS benar-benar terhapus. selanjutnya kita akan menyimpan konfigurasi IPTABLES dengan mengetikkan :

```bash
sudo mkdir /etc/iptables
sudo iptables-save > /etc/iptables/rules.v4
sudo iptables-restore %3C /etc/iptables/rules.v4
```
