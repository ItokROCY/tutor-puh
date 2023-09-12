install kali linux

```bash
wsl --install -d kali-linux
```

didalam terminal kali-linux
change mirrorlist

```bash
$ sudo nano /etc/apt/source.list
```

in GNU nano

```bash
See: https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/
#deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware

# Additional line for source packages
#deb-src http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```

tambahkan baris

```bash
# mirror indo
deb https://xsrv.moratelindo.io/kali kali-rolling main contrib non-free non-free-firmware
```

![img1](img/Screenshot%202023-09-12%20074938.png)

ubah baris pertama dengan `#` menjadi seperti dibawah

```bash
# See: https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/
#deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware

# Additional line for source packages
#deb-src http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware

# mirror indo
deb https://xsrv.moratelindo.io/kali kali-rolling main contrib non-free non-free-firmware
```

jika sudah keluar dari vim dengan menekan tombol `ctrl+x` lalu `Y` dan `enter`

update package

```bash
sudo apt update
```

lalu install win-kex agar kali dapat berjalan dalam bentuk GUI

```bash
$ sudo apt upgrade && sudo apt install kali-win-kex -y
```

menjalankan kali dalam seamless mode
![img2](img/win-kex-sl.png)

```bash
$ kex--sl -s
```

install semua default tools kali
(pastikan mirror link sudah diset supaya tidak memakan waktu lama saat download)

```bash
$ sudo apt install -y kali-linux-large
```
