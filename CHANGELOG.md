# Catatan Perubahan (Changelog) - TukangBackup.pykit

Dokumen ini mencatat semua perubahan penting pada setiap versi proyek. Format ini didasarkan pada [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.2.0] - Peningkatan Multi-Mode & Keterbacaan - 2025-06-10

Ini adalah pembaruan besar yang memperkenalkan fleksibilitas mode backup dan peningkatan pengalaman pengguna.

### Ditambahkan (Added)

* **Mode Backup Fleksibel**:
    * *Command* `lokal-backup` baru untuk menjalankan backup database langsung dari server lokal (tanpa SSH).
    * *Command* `ssh-backup` baru untuk secara eksplisit menjalankan backup dari server remote via SSH.
    * Konfigurasi `[MAIN]` di `config.ini` dengan `default_backup_mode` (`ssh` atau `lokal`) untuk menentukan perilaku default saat skrip dijalankan tanpa argumen.
* **Perintah Manajemen Telegram**:
    * *Command* `tele [on|off]` untuk mengaktifkan atau menonaktifkan notifikasi Telegram, dengan perubahan yang disimpan ke `config.ini`.
    * *Command* `test-tele` baru untuk mengirim pesan notifikasi Telegram percobaan.
* **Integrasi Google Drive**:
    * Fungsionalitas unggah backup ke folder Google Drive (`GDRIVE_FOLDER_ID`).
    * Metode otentikasi Google Drive via OAuth 2.0 (aplikasi desktop) menggunakan `client_secrets.json` dan `credentials.json`.
    * *Command* `test-gdrive` baru untuk menguji koneksi dan alur otentikasi Google Drive.
* **Bantuan Interaktif**:
    * *Command* `help` baru yang menampilkan deskripsi tool, daftar semua *command* yang tersedia, catatan penting konfigurasi, panduan *troubleshooting* Google Drive, dan petunjuk untuk otomatisasi dengan Cron Job.

### Diubah (Changed)

* **Pesan Notifikasi Telegram**: Ditingkatkan menjadi lebih profesional dan minimalis dengan format HTML (tebal, miring, monospace).
* **Tampilan Bantuan (`help`)**: Dirombak total agar lebih profesional, ringkas, dan menggunakan *bullet point* ASCII (`*`) untuk kompatibilitas terminal yang lebih luas.
* **Pesan Konsol Umum**: Disesuaikan untuk mencerminkan mode backup yang sedang berjalan (`Lokal` atau `SSH`) di output status.
* **`clear_screen()`**: Dioptimalkan agar hanya membersihkan layar terminal sekali di awal eksekusi utama skrip, bukan setiap kali *command* dipanggil.
* **Struktur `config.ini`**: Diperbarui untuk menyertakan bagian `[MAIN]` dan konfigurasi `[TELEGRAM]` yang lebih lengkap.
* **Pesan Error & Peringatan**: Ditingkatkan agar lebih informatif dan mudah dipahami.
* **URL GitHub**: Diperbarui dan konsisten di seluruh skrip.

### Dihapus (Removed)

* Semua komentar tidak penting dan *debug print* dari kode inti untuk menjaga kerapian.
* Ketergantungan pada SSH untuk menjalankan `mysqldump` saat mode `lokal-backup` dipilih.

---

## [1.1.0] - Penambahan Google Drive & Telegram Awal - 2025-06-09

Ini adalah pembaruan pertama yang memperkenalkan fitur integrasi cloud dan notifikasi.

### Ditambahkan (Added)

* **Integrasi Google Drive (Awal)**:
    * Kemampuan untuk mengunggah file backup `.tar.gz` ke Google Drive.
    * Pengaturan `GDRIVE_FOLDER_ID` dan `GDRIVE_AUTH_METHOD` di `config.ini`.
* **Notifikasi Telegram (Awal)**:
    * Fungsionalitas pengiriman notifikasi status backup (sukses/gagal) ke Telegram.
    * Pengaturan `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`, dan `SEND_TELEGRAM_NOTIFICATION` di `config.ini`.

### Diperbaiki (Fixed)

* Penanganan error yang lebih tangguh untuk operasi file dan subprocess.
* Konsistensi pesan status dan error di konsol.

---

## [1.0.0] - Rilis Perdana - 2025-06-09

Ini adalah rilis pertama dari `TukangBackup.pykit`, sebuah alat otomatisasi backup database yang andal dan mudah digunakan.

### Ditambahkan (Added)

* **Fungsionalitas Backup Inti**:
    * Kemampuan untuk melakukan backup semua database MySQL/MariaDB (`--all-databases`) dari server remote.
    * Menggunakan koneksi SSH yang aman dengan autentikasi berbasis kunci (tanpa password).
    * Hasil backup otomatis dikompresi ke format `.tar.gz` untuk menghemat ruang.
* **Konfigurasi & Fleksibilitas**:
    * Semua pengaturan (kredensial, path, dll.) dipisahkan ke dalam file `config.ini` eksternal demi keamanan dan kemudahan modifikasi.
    * Nama file backup kini menyertakan timestamp lengkap (tanggal dan waktu) untuk mencegah file tertimpa saat backup dijalankan beberapa kali di hari yang sama.
* **Manajemen & Monitoring**:
    * Fitur pembersihan otomatis untuk menghapus backup lama yang sudah melewati batas hari yang ditentukan di `config.ini`.
    * Menyimpan catatan waktu backup sukses terakhir di file `.tukangbackup_status` untuk monitoring.
    * Mode `status` (`python TukangBackup.pykit status`) untuk melihat ringkasan cepat: lokasi backup, waktu backup terakhir, dan status eksekusi terakhir dari `backup.log`.
    * Mode `config` (`python TukangBackup.pykit config`) untuk menampilkan pengaturan yang sedang aktif dengan aman (password disamarkan).
* **Tampilan & Pengalaman Pengguna (UI/UX)**:
    * Tampilan terminal yang bersih, terstruktur, dan informatif dengan status berwarna (`[SUCCES]`, `[FAILED]`).
    * Banner ASCII kustom untuk memberikan identitas pada tool.
    * Log proses yang rata dan rapi untuk kemudahan membaca.

### Diperbaiki (Fixed)

* Memperbaiki berbagai *bug* minor pada tampilan dan alur program selama proses pengembangan untuk memastikan stabilitas dan pengalaman pengguna yang lebih baik.
