### Laporan Hasil Pratikum 1 - Sistem Administrasi Server 

Nadila Chusnul K - 1202190020

Anastasya Rahma Juniarti - 120219058 

#Hal pertama pastikan adaptop bridge dan setting ip static

        ![A1](aset/bridge.jpg)

- Check lxc  dari praktikum sebelumnya
    ```bash
    lcx-ls - f
    ```
    ![A1.1](aset/cek.jpg)

### 1. Mengubah nama container dari ubuntu_php5.6 menjadi ubuntu_landing dengan menyesuaikan ip yang terbaru 
- rename ubuntu_php5.6 to ubuntu_landing
     ```bash 
    lxc-copy -R -n ubuntu_php5.6 -N ubuntu_landing
    ```
  - check list container 
     ```bash 
    lxc-ls -f
     ```
    ![A1.2](aset/1.ubuntujadilanding.jpg)

- start lxc php landing dan php 7
    ```bash 
    lxc-start -n ubuntu_landing 
    ```
    ![A1.3](aset/1.ubuntujadilanding.jpg)

- masuk kedalam container dan buka network interfaces
    ```bash 
    lxc-attach -n ubuntu_landing 
    nano /etc.network/interfaces
    ```
    ![A1.5](aset/1.lxcphplandingdanphp7.jpg)

- ubah ip ubuntu landing 

    ![A1.6](aset/1.ubahiplanding.jpg)

- restart ip. Lalu cek ip menggunakan ifconfig 
    ```bash
    shutdwon now 
    lxc-star -n ubuntu_landing
    lcx-attach -n ubuntu_landing
    ifconfig 
    ```
    ![A1.7](aset/1.restratip.jpg)

- keluar dari ubuntu landing untuk lanjut sebelumnya 
    ```bash
    exit
    ```
    ![A1.8](aset/1.keluarubuntulanding.jpg)

### 2.  Membuat container lxc debian 9 dengan nama debian_php5.6 dengan ip yang baru 
- Cek koneksi internet dengan ping google.com / 8.8.8.8 / 1.1.1.1
    ```bash 
    ping google.com 
    ```
    ![A1.9](aset/2.cekkoneksiinternet.jpg)
- Update repositori Debian 
    ```bash 
    nano /etc/apt/sources.list
    ```
    ![A1.10](aset/2.updaterespositori.jpg)
- Install lxc Debian
    ```bash 
    lxc-create -n debian_php5.6 -t download  -- --dist debian ---release stretch ---arch amd64 --force-cache --n0-validate --server imager.linuxcocntainers.org 
    ```
    ![A1.11](aset/2.installxcdebian.jpg)

- cek lxc Debian 
    ```bash 
    lxc-ls -f 
    ```
    ![A1.12](aset/2.ceklxcdebian.jpg)

### 3. Setting nginx debian_php5.6 dengan domain http://lxc_php5.dev dan buat keterangan lxc didalam nya

- start lxc debian 
    ```bash 
    lxc-start -n debian_php5.6
    lxc-ls -f
    ```
- install nginx dan nginx-extras di dalam container
    ```bash 
    apt install nginx nginx-extras
    ```
    ![A1.13](aset/3.instalnginx.jpg)

- masuk container dan install net-tools curl  buka network interfaces dan setting ip static debian php
    ```bash 
    apt install nano net-tools curl 
    nano /etc/network/interfaces
    ```
    ![A1.14](aset/3.instalnano.jpg)
    ![A1.15](aset/3.gnunano1.jpg)

- Restart pengaturan dan cek ip
    ```bash 
    systemctl restrat networking.service
    ifconfig
    ```
    ![A1.16](aset/3.restart.jpg)

- setting nginx

    masuk kedalam direktori sites available dan buat file kosong dengan nama lxc_php5.6.dev
    ```bash
    cd /etc/nginx/sites-available
    touch lxc_php5.6.dev
    nano lxc_php5.6.dev 
    ```
    ![A1.17](aset/3.settingnginx.jpg)
    ![A1.18](aset/3.etc2.jpg)
    ![A1.19](aset/3.gnunano3.jpg)
    ![A1.18](aset/3.etc2.jpg)

- Test dengan curl 
    ```bash 
    curl -i http://lxc_php5.dev 
    ```
    ![A1.19](aset/3.testcurl.jpg)

- keluar dari debian php untuk lanjut pada soal selanjtnya 
    ```bash 
    exit 
    ```

### 4. Setting nginx ubuntu_landing dengan domain http://lxc_landing.dev dan buat keterangan lxc didalam nya

- Masuk kedalam container serta masuk ke dalam direktori sites available
    ```bash
    lxc-ls -f
    lxc-attach -n ubtuntu_landing 
    cs /etc.nginx/sites-available 
     ```
    ![A1.19](aset/4.ubuntulanding.jpg)

- rename direktori lxc agar tidak bingung 
    ```bash 
    mv lxc_php5.6.dev lxc_landing.dev 
    nano lxc_landing.dev 
    ```
    ![A1.20](aset/4.rename.jpg)
- kemudian masuk pada network interfaces untuk mengedit lxc 
    
    ![A1.21](aset/4.gnunano.jpg)

- masuk ke dalam direktori sites enable, membuat symbolic link file container dan melakukan test serta buka file direktori host
    ```bash 
    cd ../sites-enabled
    ln -s /etc/nginx/sites-available/lxc_php5.6.dev 
    nginx -t
    ```
    ![A1.21](aset/4.etc.jpg)

    ![A1.22](aset/4.etc2.jpg)

    ![A1.23](aset/4.gnunano2.jpg)

    ![A1.22](aset/4.etc3.jpg)

- Test dengan curl
    ```bash 
    curl -i http:///lxc_landing.dev 
    ```
    ![A1.22](aset/4.curl.jpg)

- edit dari ubuntu landing 

### 5. Setting ubuntu landing autostart ketika vm terbuka
- setting config container

    stop ubuntu landing lalu setting config auto start dengan lxc.start.auto = 1
    ```bash 
    lxc-stop -n ubuntu_landing 
    lxc-ls -f 
    ```
- Masuk ke direktori /var/lib/lxc dan cek ada direktori apa saja
    ```bash 
    cd /var/lib/lxc 
    ```
- Menggunakan auto start pada ubuntu landing, tambahkan config sseperti dibawah 

    ![A1.23](aset/5.autostart.jpg)

- Cek auto start dengan cara reboot

      ![A1.24](aset/5.carareboot.jpg)
### 6. Setting nginx pada vm sesuai soal
- setting nginx

    setting nginx hosts
    ```bash 
    nano /etc/hosts
    ```
    ![A1.25](aset/6.nginx.jpg)

   ![A1.25](aset/6.gnunano.jpg)


- Masuk ke dalam direktori sites available dan buka vm local

    ```bash 
    cd /etc/nginx/sites-available
    nano vm.local
    ```
    ![A1.25](aset/6.sysadmin.jpg)
- melakukan test serta mengecek konektivitas ke URL

     ```bash
     sudo nginx -t
     sudo nginx -s reload
     curl -i http://vm.local
     ```
    ![A1.25](aset/6.gnunano2.jpg)
### 7. Pengujian pada web browser ketiga web
- setting host pada windows
-
    ![A1.25](aset/6.settinghost.jpg)
    
    hasil test web 
    - mengakses http://vm.local
    
    ![A1.25](aset/6.http.jpg)
    
    ![A1.25](aset/uhhi.jpg)

    - mengakses http://vm.local/blog
    
    ![A1.25](aset/blog.jpg)
    
    ![A1.25](aset/welcome.jpg)

    - mengakses http://vm.local/app
    
     ![A1.25](aset/app.jpg)
     
     ![A1.25](aset/welcomelxc.jpg)   
     
 ### 8. Soal analisa 
    - mengapa untuk kebutuhan php5.6 tidak bisa menggunakan ubuntu 16.04, sehingga perlu diganti os ke debian 9?

        Ubuntu 16.04 will reach the end if otherwise support and unusable on php 5.6

    - kenapa harus menggunakan virtualisasi LXC pada skema website yang akan didevelop?

          Because the website uses Linux. So, using LXC virtualization to make servers easier

    - apa yang dimaksud dengan proxy server? kenapa vm.local bisa kita anggap sebagai proxy server?
    
        A proxy server is a computer program that acts as an intermediary between clients to request content from the Internet or intranet. To control the activity of data               packet traffic that passes through it.
