# TukangBackup.pykit

<pre>
    ____ <==> ____
    \___\(**)/___/          TukangBackup.pykit
     \___|  |___/   multi-mode auto backup all database
         L  J
         |__|           https://github.com/ariphx/
          vv
</pre>

A **multi-mode auto backup all database** solution for MySQL/MariaDB. This Python script offers flexible backup options from **remote servers via SSH** or **directly from the local server**. It includes seamless Google Drive integration and Telegram notifications for peace of mind.

---

## ✨ Fitur Utama

* **Backup Multi-Mode**: Lakukan backup dari **server remote via SSH** atau **langsung dari server lokal** tempat skrip berjalan.
* **Backup Otomatis Komprehensif**: Melakukan `mysqldump --all-databases` termasuk *routines*, *events*, dan *triggers*.
* **Koneksi Aman SSH**: Menggunakan SSH untuk akses remote ke database (untuk mode `ssh-backup`).
* **Konfigurasi Fleksibel**: Semua pengaturan dikelola terpisah di `config.ini`.
* **Kompresi `.tar.gz`**: Menghemat ruang penyimpanan.
* **Pembersihan Otomatis**: Menghapus backup lama di lokal berdasarkan durasi yang dikonfigurasi.
* **Integrasi Google Drive**: Unggah otomatis file backup ke folder Google Drive Anda.
* **Notifikasi Telegram**: Dapatkan notifikasi status backup (sukses/gagal) langsung ke Telegram Anda.
* **Manajemen Mudah**: Dilengkapi dengan berbagai *command* untuk status, konfigurasi, tes GDrive, dan kontrol notifikasi Telegram.

---

## 🛠️ Kebutuhan Sistem

* **Python 3.x**
* **Di PC/Server yang menjalankan script:**
    * MySQL/MariaDB Client Tools (`mysqldump`)
    * *Untuk mode `ssh-backup`*: OpenSSH Client
* **Di Server Database (Remote):**
    * *Untuk mode `ssh-backup`*: OpenSSH Server
    * MySQL/MariaDB Client Tools (`mysqldump`)

---

## 🚀 Instalasi & Konfigurasi

1.  **Clone Repository:**
    ```bash
    git clone [https://github.com/ariphx/TukangBackup.git](https://github.com/ariphx/TukangBackup.git)
    cd TukangBackup
    ```

2.  **Buat Virtual Environment (Sangat Disarankan):**
    ```bash
    python -m venv venv
    # Aktifkan:
    # Windows: .\venv\Scripts\activate
    # Linux/macOS: source venv/bin/activate
    ```

3.  **Instal Dependensi:**
    ```bash
    pip install pydrive2 requests
    ```

4.  **Siapkan `config.ini`:**
    * Salin `config.ini.example` menjadi `config.ini`:
        ```bash
        cp config.ini.example config.ini
        ```
    * Buka `config.ini` dan **isi semua nilai** sesuai server, database, dan preferensi Anda (folder tujuan lokal, pengaturan GDrive, detail bot Telegram).
        * **PENTING**: Di bagian `[MAIN]`, atur `default_backup_mode = ssh` atau `default_backup_mode = lokal` sesuai mode backup utama Anda.

5.  **Siapkan Google Drive (OAuth 2.0 Desktop App):**
    * Pergi ke [Google Cloud Console > API & Services > Credentials](https://console.cloud.com/apis/credentials).
    * Buat **"OAuth client ID"** baru, pilih **"Desktop app"**.
    * Unduh file JSON, lalu **ubah namanya menjadi `client_secrets.json`**.
    * Tempatkan `client_secrets.json` ini di direktori yang sama dengan `TukangBackup.pykit`.
    * **PENTING**: Akun Google yang terautentikasi harus memiliki izin `Editor` atau `Contributor` pada folder tujuan Google Drive Anda.

6.  **Siapkan Notifikasi Telegram:**
    * Buat bot baru melalui `@BotFather` di Telegram (`/newbot`) untuk mendapatkan **Token Bot**.
    * Kirim pesan ke bot baru Anda (atau tambahkan bot ke grup dan kirim pesan di sana).
    * Buka `https://api.telegram.org/bot[YOUR_BOT_TOKEN]/getUpdates` di browser Anda untuk mendapatkan **Chat ID**.
    * Isi `telegram_bot_token` dan `telegram_chat_id` di `config.ini`.

7.  **Setup Kunci SSH (Untuk mode `ssh-backup` jika belum):**
    * Pastikan PC/server yang menjalankan script dapat login ke server database remote via SSH tanpa password (gunakan SSH Key).
    ```bash
    # Buat kunci baru jika belum ada
    ssh-keygen -t rsa -b 4096

    # Salin kunci publik Anda ke server remote
    ssh-copy-id username@ip_server_anda
    ```

---

## 🚀 Cara Penggunaan

Pastikan Anda berada di dalam direktori proyek dan **virtual environment Anda aktif**.

* **Menjalankan Backup via SSH:**
    (Gunakan ini jika skrip berada di PC terpisah dari server database).
    ```bash
    python TukangBackup.pykit ssh-backup
    ```

* **Menjalankan Backup Lokal:**
    (Gunakan ini jika skrip berada di server database itu sendiri).
    ```bash
    python TukangBackup.pykit lokal-backup
    ```

* **Menjalankan Backup dengan Mode Default (`config.ini`):**
    (Akan menggunakan `default_backup_mode` yang diset di `config.ini`).
    ```bash
    python TukangBackup.pykit
    ```

* **Mengecek Status Backup Terakhir:**
    ```bash
    python TukangBackup.pykit status
    ```

* **Melihat Konfigurasi Saat Ini:**
    ```bash
    python TukangBackup.pykit config
    ```

* **Melakukan Tes Koneksi Google Drive:**
    (Pertama kali, akan membuka browser untuk otentikasi. Token akan disimpan di `credentials.json`.)
    ```bash
    python TukangBackup.pykit test-gdrive
    ```

* **Mengirim Pesan Tes Notifikasi Telegram:**
    ```bash
    python TukangBackup.pykit test-tele
    ```

* **Mengaktifkan/Menonaktifkan Notifikasi Telegram:**
    ```bash
    python TukangBackup.pykit tele on   # Aktifkan
    python TukangBackup.pykit tele off  # Nonaktifkan
    python TukangBackup.pykit tele      # Lihat status saat ini
    ```

* **Menampilkan Bantuan & Penggunaan (Ini!):**
    ```bash
    python TukangBackup.pykit help
    ```

---

## ⚙️ Otomatisasi dengan Cron Job (Linux/Unix)

Untuk menjalankan backup secara otomatis menggunakan Cron Job:

1.  Buka terminal dan edit crontab Anda:
    ```bash
    crontab -e
    ```
2.  Tambahkan baris berikut di akhir file (sesuaikan jadwal dan path-nya):
    ```crontab
    0 3 * * * /usr/bin/python3 /path/to/TukangBackup.pykit [backup_mode] >> /var/log/tukangbackup.log 2>&1
    ```
    * **`0 3 * * *`**: Jadwal (di sini, setiap hari jam 3 pagi). Format: `menit jam hari_bulan bulan hari_minggu`.
    * **`/usr/bin/python3`**: Path lengkap ke interpreter Python Anda (gunakan `which python3` untuk mencari).
    * **`/path/to/TukangBackup.pykit`**: Path lengkap ke skrip Anda (misalnya, `/home/user/my_backup_project/TukangBackup.pykit`).
    * **`[backup_mode]`**: Ganti dengan `ssh-backup` (untuk mode SSH) atau `lokal-backup` (untuk mode lokal). Anda juga bisa menghapus `[backup_mode]` jika ingin Cron menggunakan `default_backup_mode` dari `config.ini`.
    * **`>> /var/log/tukangbackup.log 2>&1`**: Mengarahkan output dan error ke file log untuk debugging (disarankan).
3.  Simpan dan keluar dari editor (biasanya Ctrl+X, Y, Enter untuk nano).

---

## ⚠️ Troubleshooting Google Drive

Jika Anda mengalami error Google Drive (misalnya, `HttpError`, `token expired`, atau masalah otentikasi):

1.  **Jalankan ulang `python TukangBackup.pykit test-gdrive`**. Ini akan memicu ulang proses otentikasi browser jika token sudah kedaluwarsa atau ada masalah koneksi.
2.  Pastikan file **`client_secrets.json`** sudah benar dan berada di folder yang sama dengan skrip.
3.  Pastikan **`destination_folder_id`** di `config.ini` hanya berisi ID folder (misalnya, `1bFvBiosNkCvdAwjxuy7avjSBKGBgKW-N`), tanpa komentar atau karakter tambahan.
4.  Periksa izin akun Google Anda di Google Drive. Akun yang digunakan harus memiliki izin `Editor` atau `Contributor` pada folder tujuan.
5.  Jika masalah berlanjut, hapus file **`credentials.json`** di folder skrip Anda, lalu jalankan kembali `test-gdrive`. Ini akan memaksa otentikasi ulang dari awal.

---

## 🤝 Kontribusi (Contributing)

Saran, laporan *bug*, dan kontribusi untuk membuat `TukangBackup.pykit` menjadi lebih baik sangat diterima.

* Untuk **laporan *bug*** atau **saran fitur baru**, silakan buat [*Issue* baru](https://github.com/ariphx/TukangBackup/issues).
* Untuk **kontribusi kode**, silakan buat [*Pull Request*](https://github.com/ariphx/TukangBackup/pulls).
* Jika ada pertanyaan atau ingin berdiskusi lebih lanjut, jangan ragu untuk menghubungi saya via **[Facebook](https://fb.me/arif.kun.456)**.

---

## 🙏 Dukungan (Support)

`TukangBackup.pykit` adalah proyek sumber terbuka (*open source*) yang saya kembangkan dan rawat di waktu luang.

Jika Anda merasa alat ini bermanfaat dan ingin memberikan sedikit apresiasi, Anda bisa mendukung saya dengan mentraktir secangkir kopi melalui Trakteer. Setiap dukungan, berapapun nilainya, sangat berarti dan memotivasi saya untuk terus mengembangkan proyek ini.

Terima kasih banyak atas dukungan Anda!

<a href="https://trakteer.id/ariphx/tip" target="_blank"><img id="wse-buttons-preview" src="https://cdn.trakteer.id/images/embed/trbtn-red-5.png" height="50" style="border:0px;height:50px;" alt="Trakteer, Traktir Kopi"></a>

---

## 📄 Lisensi

Proyek ini dilisensikan di bawah [Lisensi MIT](https://github.com/ariphx/TukangBackup/blob/main/LICENSE).
