# Catatan Perubahan (Changelog) - TukangBackup.pykit

Dokumen ini mencatat semua perubahan penting pada setiap versi proyek. Format ini didasarkan pada [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [1.0.0] - Rilis Perdana - 2025-06-09

Ini adalah rilis pertama dari `TukangBackup.pykit`, sebuah tools automasi backup database yang andal dan mudah digunakan.

### Ditambahkan (Added)

* **Fungsionalitas Backup Inti:**
    -   Kemampuan untuk melakukan backup semua database MySQL/MariaDB (`--all-databases`) dari server remote.
    -   Menggunakan koneksi SSH yang aman dengan autentikasi berbasis kunci (tanpa password).
    -   Hasil backup otomatis dikompresi ke format `.tar.gz` untuk menghemat ruang.

* **Konfigurasi & Fleksibilitas:**
    -   Semua pengaturan (kredensial, path, dll.) dipisahkan ke dalam file `config.ini` eksternal demi keamanan dan kemudahan modifikasi.
    -   Nama file backup kini menyertakan timestamp lengkap (tanggal dan waktu) untuk mencegah file tertimpa saat backup dijalankan beberapa kali di hari yang sama.

* **Manajemen & Monitoring:**
    -   Fitur pembersihan otomatis untuk menghapus backup lama yang sudah melewati batas hari yang ditentukan di `config.ini`.
    -   Menyimpan catatan waktu backup sukses terakhir di file `.tukangbackup_status` untuk monitoring.
    -   Mode `status` (`python backup.py status`) untuk melihat ringkasan cepat: lokasi backup, waktu backup terakhir, dan status eksekusi terakhir dari `backup.log`.
    -   Mode `config` (`python backup.py config`) untuk menampilkan pengaturan yang sedang aktif dengan aman (password disamarkan).

* **Tampilan & Pengalaman Pengguna (UI/UX):**
    -   Tampilan terminal yang bersih, terstruktur, dan informatif dengan status berwarna (`[SUCCES]`, `[FAILED]`).
    -   Banner ASCII kustom untuk memberikan identitas pada tools.
    -   Log proses yang rata dan rapi untuk kemudahan membaca.

### Diperbaiki (Fixed)

* Memperbaiki berbagai bug minor pada tampilan dan alur program selama proses pengembangan untuk memastikan stabilitas dan pengalaman pengguna yang lebih baik.
