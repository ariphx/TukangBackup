# TukangBackup.pykit

<div>
  <pre>
    ____ &lt;==&gt; ____
    \___\(**)/___/                                TukangBackup.pykit
     \___|  |___/                       auto backup all database via login ssh
         L  J                     
         |__|                                  https://github.com/ariphx/
          vv
  </pre>
</div>

Sebuah tools backup database MySQL/MariaDB otomatis via SSH yang simpel, andal, dan mudah dikonfigurasi. Dibuat dengan Python.

---

## Fitur Utama

- **Backup Semua Database**: Secara otomatis melakukan `mysqldump` untuk semua database (`--all-databases`) termasuk *routines*, *events*, dan *triggers*.
- **Koneksi Aman**: Menggunakan koneksi SSH dengan autentikasi berbasis kunci (*SSH Key*), sehingga tidak ada password yang ditulis di dalam script.
- **Konfigurasi Eksternal**: Semua pengaturan (kredensial SSH, database, folder tujuan) disimpan dalam file `config.ini` yang terpisah dan aman.
- **Kompresi Otomatis**: Hasil backup `.sql` akan otomatis dikompres ke format `.tar.gz` untuk menghemat ruang penyimpanan.
- **Pembersihan Otomatis**: Secara otomatis menghapus file backup lama berdasarkan durasi hari yang bisa diatur di konfigurasi.
- **Monitoring Mudah**: Dilengkapi dengan mode `status` untuk mengecek kapan backup terakhir berjalan dan apa hasilnya, serta mode `config` untuk melihat pengaturan saat ini.
- **Tampilan Informatif**: Log yang bersih dengan status berwarna untuk memudahkan monitoring saat eksekusi.

## Kebutuhan Sistem

- Python 3.x
- **Di PC/Server yang menjalankan script:**
    - OpenSSH Client
- **Di Server Database (Remote):**
    - OpenSSH Server
    - MySQL/MariaDB Client Tools (`mysqldump`)

## Instalasi & Konfigurasi

1.  **Clone atau Unduh Repository**
    ```bash
    git clone https://github.com/ariphx/TukangBackup.git
    cd TukangBackup
    ```

2.  **Buat File Konfigurasi**
    Salin file contoh `config.example.ini` menjadi `config.ini`.
    ```bash
    cp config.example.ini config.ini
    ```
    Kemudian, buka `config.ini` dengan editor teks dan isi semua nilainya sesuai dengan data server dan database Anda.

3.  **Setup Kunci SSH**
    Pastikan PC Anda bisa login ke server remote via SSH tanpa password. Jika belum, lakukan setup kunci SSH:
    ```bash
    # Buat kunci baru jika belum ada
    ssh-keygen -t rsa -b 4096

    # Salin kunci publik Anda ke server remote
    ssh-copy-id username@ip_server_anda
    ```

## Cara Penggunaan

Pastikan Anda berada di dalam direktori proyek.

#### 1. Menjalankan Proses Backup
Perintah ini akan menjalankan proses backup lengkap. Ini adalah perintah yang akan Anda gunakan di dalam penjadwal (Cron/Task Scheduler). Untuk logging, disarankan menggunakan redirection.

```bash
python backup.py >> backup.log 2>&1
````

#### 2\. Mengecek Status Terakhir

Untuk melihat ringkasan cepat tanpa menjalankan backup:

```bash
python backup.py status
```

**Contoh Output:**

```text
    ____ <==> ____
    \___\(**)/___/            TukangBackup.pykit
     \___|  |___/     auto backup all database via login ssh
         L  J                     
         |__|              https://github.com/ariphx/
          vv

[TukangBackup Status Check]
---------------------------------------------
  - Lokasi Backup   : D:/TukangBackup/
  - Backup Terakhir : 2025-06-09 14:30:00
  - Status Terakhir : SUKSES
---------------------------------------------
```

#### 3\. Melihat Konfigurasi Saat Ini

Untuk menampilkan pengaturan yang sedang aktif dari `config.ini` (password akan disamarkan):

```bash
python backup.py config
```

**Contoh Output:**

```text
    ____ <==> ____
    \___\(**)/___/            TukangBackup.pykit
     \___|  |___/     auto backup all database via login ssh
         L  J                     
         |__|             https://github.com/ariphx/
          vv

[TukangBackup Current Configuration]
---------------------------------------------
[SSH]
  - server_name : Server Produksi Utama
  - user        : root
  - server_ip   : 192.168.1.100
[DATABASE]
  - user        : user_db_anda
  - password    : ********************
[LOCAL]
  - destination : D:/TukangBackup/
  - cleanup_days: 30
---------------------------------------------
Lokasi file config: D:\TukangBackup\config.ini
```

## Contoh Penjadwalan dengan Cron (Linux)

Untuk menjalankan backup ini secara otomatis setiap hari jam 2 pagi, edit crontab Anda:

```bash
crontab -e
```

Lalu tambahkan baris ini (sesuaikan path-nya):

```crontab
0 2 * * * cd /path/to/backup.py && /usr/bin/python3 backup.py >> backup.log 2>&1
```

*Perintah `cd` memastikan script dijalankan dari direktori yang benar sehingga bisa menemukan `config.ini`.*

## Kontribusi

Saran, laporan bug, dan kontribusi sangat diterima. Silakan buat *Issue* atau *Pull Request*.

## Lisensi

Proyek ini dilisensikan di bawah [Lisensi MIT](https://github.com/ariphx/TukangBackup/blob/main/LICENSE).

