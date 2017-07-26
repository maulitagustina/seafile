\begin{titlepage}

    \pagenumbering{gobble}

    \begin{center}

        { \huge \bfseries LAPORAN PROJECT MATA KULIAH KOMUNIKASI DATA DAN JARINGAN KOMPUTER}\\[1cm]
        { \huge \bfseries SEAFILE}\\[1cm]

        % Logo
        \includegraphics[width=2in, height=2in]{./gambar/logo}\\[1cm]

        \textsc{\Large \normalfont Disusun oleh:}\\[0.5cm]

        \begin{framed}

          \begin{tabular}{ l c l }
            Wulan Maulida & : & G64140061 \\
            Maulita Agustina  & : & G64130056 \\
            Sita Nabila Kamelina & : & G64130096 \\
            Siti Syarah Annisa & : & G64130023 \\
          \end{tabular}

        \end{framed}

        
        {\LARGE \bfseries DEPARTEMENT ILMU KOMPUTER}\\[1cm]
        {\LARGE \bfseries FAKULTAS MATEMATIKA DAN ILMU PENGETAHUAN ALAM}\\[1cm]
        {\LARGE \bfseries INSTITUT PERTANIAN BOGOR}\\[1cm]
        {\LARGE \bfseries 2017}\\[1cm]

        \vfill

    \end{center}

\end{titlepage}



# Deskripsi Aplikasi

Seafile adalah open source cloud storage yang bisa digunakan pada sebuah team atau organisasi sehingga memungkinkan anggota dari sebuah organisasi untuk saling berkolaborasi dalam mengelola sebuah file yang sama. File pada seafile disimpan pada sebuah server/librari yang bisa disinkronisasikan ke personal computer and mobile devices menggunakan seafile client. File juga bisa diakses lewat server web interface. Seafile mirip dengan dropbox dan google drive pada google. Perbedaannya adalah seafile open source cloud service dengan space memori dan koneksi yang tanpa batas. <br />


Seafile dirancang dengan menggunakan bahasa pemprograman C dengan performance yang tinggi. Proses upgrade sangat mudah dilakukan karena pada seafile tidak dibutuhkan pembaruan yang besar pada database. Sebuah librari file yang dimiliki user bisa dienkripsi menggunakan sebuah password yang tidak harus di store ke server sehingga hanya bisa diketahui oleh user, bahkan admin juga bisa tidak mengetahuinya. <br />


Fitur  fitur yang dimiliki oleh seafile adalah : <br />
* Selective sync
* Differential sync to minimize bandwidth
* Client-side encryption
* User and group permission management
* Public link-sharing
* Version control
* Online file editing

# Instalasi

### **Step1**
Install Seafile Dependencies.<br />
Untuk menginstall seafile, terlebih dahulu harus menginstall software berikut: <br />
* Openjdk-8jre
* Poppler-utils
* Libreoffice 4.1+ and Python-uno
* Libpython 2.7
* Python libraries

Install menggunakan command pada terminal sebagai root user: <br />
<pre><code> apt-get install openjdk-8-jre poppler-utils libreoffice libreoffice-script-provider-python libpython2.7 python-pip mysql-server python-setuptools python-imaging python-mysqldb python-memcache ttf-wqy-microhei ttf-wqy-zenhei xfonts-wqy python-pip</code></pre>

Kemudian check apakah versi python yang sudah terinstall sesuai dengan requirement seafile (Python 2.7.6): <br />
<pre><code> python -V</code></pre>

Install pip package menggunakan easy_install <br />
<pre><code> easy_install pip</code></pre>

Install boto package <br />
<pre><code>pip install boto</code></pre>

Jika terjadi error ketika setting lokal, dapat run command berikut <br />
<pre><code> export LANGUAGE=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
locale-gen en_US.UTF-8
dpkg-reconfigure locales </code></pre>

### **Step2**
Membuat user baru “Seafile” <br />
Membuat user baru “Seafile” untuk installasi<br />
<pre><code> useradd -m -s /bin/bash seafile
passwd seafile </code></pre>

### **Step3**
Download dan ektraksi file Seafile <br />
Seafile dapat didownload dari situs resminya  http://seafile.com/en/Download. Cari installer sesuai kebutuhan atau PC yang dipakai. Dapat juga dengan mendapatkan link kemudian masukkan link ke command wget di terminal untuk mendownload. <br />
<pre><code> su - seafile
wget wget https://bintray.com/artifact/download/seafile-org/seafile/seafile-server_5.1.4_x86-64.tar.gz</code></pre>

Kemudian buat direktori khusus di root. Dalam hal ini akan digunakan nama ‘seafile-server’ <br />
<pre><code> mkdir /root/seafile-server/</code></pre>

Ekstrasi file  seafile-server_5.1.4_x86-64.tar.gz <br />
<pre><code> tar -xzf seafile-server_5.1.4_x86-64.tar.gz
mv seafile-server-5.1.4/root/ seafile-server/</code></pre>

Masuk ke direktori project, kemudian extract file seafile-server_5.0.5_i386.tar.gz menggunakan command tar <br />
<pre><code> tar -xzf seafile-server_5.0.5_i386.tar.gz</code></pre>

Buat direktori installed, pindahkan seafile-server_5.0.5_i386.tar.gz ke direktori tersebut <br />
<pre><code> mv seafile-server_5.0.5_i386.tar.gz installed/</code></pre>

### **Step 4**
Membuat database <br />
Seafile membutuhkan 3 databases: <br />
* ccnet server
* seafile server
* seahub

Sekarang kembali ke root user dengan "exit", lalu login ke mysql server sendiri.<br />
buat 3 database dengan satu user dan memberi kepada semua user databases. <br />
<pre><code>#Login to mysql
mysql -u root -p
    
#Create database
create database ccnet_db character set = 'utf8';
create database seafile_db character set = 'utf8';
create database seahub_db character set = 'utf8';

#Create user
create user seacloud@localhost identified by 'yourpassword';

#Grant user to the databases
grant all privileges on ccnet_db.* to seacloud@localhost identified by 'yourpassword';
grant all privileges on seafile_db.* to seacloud@localhost identified by 'yourpassword';
grant all privileges on seahub_db.* to seacloud@localhost identified by 'yourpassword';
flush privileges; </code></pre>

# Konvigurasi
Seafile memiliki 3 komponen inti. Maka dari itu akan dibuat database untuk masing-masing komponen tersebut. <br />
* Ccnet server : digunakan untuk komunikasi internal pada seafile (sebagai penghubung/komunikasi antar komponen).
* Seafile server : menangani masalah file upload, download,dan syncronisasifile
* Seahub : seafile web yang melakukan akses data ke seafile sever.

Akan dibuat tiga database dengan nama ccnet-db, seafile-db, dan seahub-db dan akun yang digunakan adalah seafile. <br />
Login ke mysql sebagai root user <br />
<pre><code> mysql -u root -p </code></pre>

Kemudian masukkan command tersebut ke mysql shell <br />

# Maintenance
Hal yang harus dilakukan untuk menjaga agar seafile tetap bisa berjalan dan digunakan dengan baik adalah denagn mealkukan update secara berkala. <br />

# Otomatisasi
     
<pre><code> Apt-get update
Apt-get upgrade
apt-get install openjdk-7-jre poppler-utils libreoffice libreoffice-script-provider-python libpython2.7 python-pip mysql-server python-setuptools python-imaging python-mysqldb python mempache
python –V
easy_install pip
pip install boto
apt-get install ttf-wqy-microhei ttf-wqy-zenhei xfonts-wqy
uname –m
cd /tmp
wget https://bintray.com/artifact/download/seafile-org/seafile/seafile-server_5.0.5_i386.tar.gz
mkdir /root/project/
mv seafile-server_5.0.5_i386.tar.gz /root/project/
tar -xzf seafile-server_5.0.5_i386.tar.gz
mv seafile-server_5.0.5_i386.tar.gz installed/
./setup-seafile-mysql.sh
./seafile.sh start
./seahub.sh start </code></pre>



# Cara Pemakaian
Masuk sebagai server admin <br />
* Buka alamat domain yang sudah dibuat di ubuntu server dan disesuaikan portnya
* Login menggunakan alamat email dan password yang sudah di create sebagai admin
* Lakukan login dan masuk ke halaman home
* Gunakan seafile sesuai kebutuhan: sharing file, download file dll

#Pembahasan

Pendapat tentang aplikasi web Seafile <br />
Pros: <br />
Diantara keuntungan dari seafile adalah: <br />
    * Ukuran storage tidak terbatas
    * Bisa digunakan oleh beberapa client atau organisasi sekaligus untuk saling berbagi file/data
    * Keamanan/ privasi pengguna terjamin karena adanya enkripsi file 
    * mudah di upgrade

Cons: <br />
* server sering down karena butuh kekuatan jaringan yang tinggi.
* installasi sedikit sulit

Membandingkan dengan aplikasi web kelompok lain yang sejenis : **Owncloud** <br />

### Storage and Sync Capabilities
#### Seafile
* Dapat menyimpan file dan data di dalam central server dan mensikronisasi file dan data dengan PC dan mobile device
* Sinkronisasi dilakukan diantara Seafile server dan Seafile client application yang di instal di laptop, desktop, smartphone, dan tablet.
* Seafile tersedia di Windows dan Linux.
* Aplikasi Seafile client tersedia di Windows, Mac, Linux, Android, dan iOS.
* Web interface dapat mengakses file dan data yang disimpan dalam Seafile server.
* Dapat membuat librari untuk mengupload dan mengatur file.
* Dapat melakukan sinkroisasi secara terpisah. Ketika kita mensinkronisasi file, Seafile menghandle file conflict dan mengirimkan data yang tidak tersedia didalam server untuk mencegah penggunaan bandwidth yang tidak dibutuhkan
* Dapat melakukan sinkronisasi file dengan 2 atau lebih server dan pengiriman file dapat dilanjutkan

#### OwnCloud
* Dapat menyimpan file dan folder, musik, video, kontak, foto, kalendar, dan lain-lain didalam ownCloud server
* Dapat dengan mudah mengakses data dari cloud server menggunakan mobile devices, desktop PC, atau web browser.
* ownCloud sync client tersedia untuk Windows, Mac, Linux, Android, dan iOS OS.
* Dapat menambah external storage ke ownCloud server dengan Dropbox, SWIFT, FTPs, Google Docs, S3, external WebDAV server, dan lain-lain.
* Menyediakan kontrol data untuk menemukan file yang lama untuk dibawa kembali.
* Mendapatkan kembali (recover) data yang tidak sengaja terhapus.
* Dengan Activity feed, kita dapat mengetahui apa yang sedang terjadi di ownCloud. Kita dapat melihat notifikasi ketika seseorang membagi file membuat file, mengubah, atau menghapus file.

### Sharing and Collaboration with Users and Workgroup
#### Seafile
* Membuat grup dengan file syncing, online file editing, wiki, komen, diskusi, dan personal message support.
* File tersimpan otomatis sewaktu mengedit file.
* Membagi librari, sub direktori, link, file, dan folder ke pengguna individu dan grup.
* Membuat file menjadi file publik sehingga dapat didownload oleh semua orang.

#### Owncloud
* Mempunyai fitur yang luas untuk mengatur user dan grup.
* Sampai dengan lima orang pengguna dapat mengedit file ODT atau DOC bersama-sama melalui web browser menggunakan fitur collaborative editing.
* Membuat grup, membagikan file dengan para pengguna, banyak grup, dan membuat link publik file.
* Antarmukanya menampilkan file yang dibagikan dengan kita, file yang kita bagikan, file yang dibagikan dengan link, dan nama dari pemilik file yang berbagi file dengan kita

### Security, Privacy and Encryption in Cloud
#### Seafile
* Fitur enkripsi yang tinggi untuk proteksi file dan privasi.
* Dapat membuat password untuk banyak librari dan semua file serta data dienkripsikan di desktop client atau browser sebelum data tersebut di upload ke server.
* Enkripsi pada browser dapat diaktifkan dari server, dan apabila aktif, semua file telah terenskripsi di browser sebelum file tersebut dikirimkan ke Seafile server.
* Fitur enkripsi pada client membuat cloud storage system sangat aman.
* Enkripsi file diselesaikan di bagian client dan password tidak dikirimkan ke dalam server.
* System administrator server tidak dapat melihat konten file di dalam server.
* Koneksi antara client dan server juga dienkripsikan.

#### Owncloud
* Terdapat aplikasi enkripsi jika kita ingin mengenkripsikan file dan data di dalam server.
* Untuk mengenkripsikan sebuah file atau data harus mengaktifkan aplikasi enkripsi dahulu.
* Enkripsi selesai di bagian server.
* System administrator server dapat melihat konten file yang tersimpan dalam ownCloud server.

# Referensi

https://www.rosehosting.com/blog/how-to-install-seafile-on-an-ubuntu-14-04-vps/

https://www.seafile.com/

https://forum.seafile-server.org/t/seafile-server-installer-for-production-ready-seafile-ce-and-pro-installations/1464

http://www.qoncious.com/questions/seafile-vs-owncloud-comparison-and-review/

https://stackoverflow.com/questions/1484595/how-to-resolve-var-www-copy-write-permission-denied/1484615#1484615?newreg=ba05eb997fb64f27bb454f47dbd19782
