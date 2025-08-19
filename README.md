# Aplikasi Check-in Aktivitas Harian - Bright Scholarship YBM BRILiaN

![Bright Scholarship](https://placehold.co/800x200/1a202c/fbb_f_1_971?text=Bright+Scholarship+Daily+Check-in&font=inter)

Aplikasi web sederhana yang dirancang untuk membantu para penerima beasiswa (awardee) **Bright Scholarship** dari **YBM BRILiaN** dalam mencatat dan memantau aktivitas harian mereka. Proyek ini bertujuan untuk mendigitalisasi proses pelaporan kegiatan sebagai bagian dari program pembinaan karakter yang komprehensif.

## Latar Belakang

**YBM BRILiaN** adalah Lembaga Filantropi Islam yang salah satu program unggulannya adalah **Bright Scholarship**. Beasiswa ini tidak hanya memberikan dukungan finansial berupa biaya kuliah dan tunjangan hidup, tetapi juga pembinaan intensif di asrama untuk membentuk pemimpin masa depan yang berkarakter, berdaya saing, dan mengaplikasikan nilai-nilai Islam.

Aplikasi ini dibuat untuk mempermudah para awardee dalam melaporkan aktivitas harian yang menjadi bagian dari evaluasi program pembinaan.

---

## ✨ Fitur Utama

-   **🔐 Autentikasi Pengguna:** Sistem pendaftaran dan login yang aman untuk setiap awardee.
-   **✅ Check-in Aktivitas Harian:** Antarmuka berbasis kartu yang intuitif untuk menandai aktivitas yang telah selesai.
-   **🔄 Reset Aktivitas Manual:** Opsi bagi pengguna untuk mereset semua aktivitas mereka pada hari itu jika terjadi kesalahan input.
-   **📈 Pencatatan Otomatis:** Data check-in harian secara otomatis dicatat dan diarsipkan ke dalam sheet terpisah untuk rekapitulasi.
-   **☀️ Reset Harian Otomatis:** Sistem akan secara otomatis mereset status check-in setiap hari, siap untuk pencatatan baru.
-   **📄 Laporan Individual:** Kemampuan untuk mencatat data ke spreadsheet individual milik masing-masing awardee untuk portofolio pribadi.

---

## 🛠️ Teknologi yang Digunakan

-   **Frontend:**
    -   HTML5
    -   [Tailwind CSS](https://tailwindcss.com/)
    -   JavaScript (Vanilla)
-   **Backend & Database:**
    -   [Google Sheets API v4](https://developers.google.com/sheets/api)

---

## ⚙️ Cara Kerja Aplikasi

Aplikasi ini beroperasi sebagai *Single-Page Application (SPA)* yang memanfaatkan Google Sheets sebagai backend untuk otentikasi dan penyimpanan data.

### Alur Pengguna

1.  **Pendaftaran & Login:** Pengguna baru mendaftarkan `username` dan `password`. Data ini disimpan di sheet `Pengguna`. Pengguna yang sudah terdaftar dapat langsung login.
2.  **Halaman Aktivitas:** Setelah login, pengguna akan melihat daftar aktivitas harian dalam bentuk kartu.
3.  **Proses Check-in:** Dengan mengklik kartu aktivitas, sebuah modal konfirmasi akan muncul. Jika dikonfirmasi, status aktivitas di Google Sheet akan diperbarui dari `0` menjadi `1`.
4.  **Logout:** Pengguna dapat keluar dari sesi, yang akan menghapus token otorisasi.

### Proses Otomatis di Latar Belakang

-   **Pencatatan Data Bulanan:** Setiap hari, saat pengguna pertama login, sistem akan memeriksa tanggal. Jika hari telah berganti, sistem akan:
    1.  Menyalin rekap data dari `Sheet1` ke dalam sheet `monthly data` sebagai arsip.
    2.  Mencatat data yang sama ke spreadsheet individual awardee (jika dikonfigurasi).
-   **Reset Harian:** Setelah proses pencatatan selesai, sistem akan mereset semua status aktivitas di `Sheet1` kembali ke `0` dan memperbarui tanggal reset terakhir di sheet `Config`.

---

## 📊 Struktur Google Sheets

Untuk menjalankan aplikasi ini, Anda perlu membuat sebuah Google Spreadsheet dengan 4 sheet yang memiliki nama dan struktur persis seperti di bawah ini:

1.  **`Sheet1`** - Untuk data check-in harian.
    -   `Kolom A`: Berisi daftar nama aktivitas (contoh: "Tahajjud", "Dhuha", dll.).
    -   `Baris 1` (mulai dari `Kolom B`): Berisi nama `username` para awardee.
    -   Sel lainnya (`B2` dan seterusnya): Berisi status aktivitas, `0` untuk belum dan `1` untuk sudah.

2.  **`Pengguna`** - Untuk menyimpan data kredensial.
    -   `Kolom A`: `username`
    -   `Kolom B`: `password`

3.  **`monthly data`** - Sebagai arsip historis.
    -   `Kolom A`: `Tanggal` (diisi otomatis)
    -   `Kolom B`: `Nama Awardee` (diisi otomatis)
    -   `Kolom C`: `Nama Aktivitas` (diisi otomatis)
    -   `Kolom D`: `Status` (0 atau 1, diisi otomatis)

4.  **`Config`** - Untuk menyimpan konfigurasi sistem.
    -   `Sel A1`: Tanggal terakhir kali reset harian dilakukan (format: `YYYY-MM-DD`).

---

## 🚀 Panduan Setup Lokal

Ikuti langkah-langkah berikut untuk menjalankan proyek ini di lingkungan lokal Anda.

### 1. Prasyarat

-   Akun Google.
-   Sebuah proyek di [Google Cloud Console](https://console.cloud.google.com/).
-   `Node.js` dan `npm` (opsional, untuk menjalankan server lokal).

### 2. Konfigurasi Google Cloud & Sheets API

1.  Di Google Cloud Console, aktifkan **Google Sheets API** untuk proyek Anda.
2.  Buka menu **Credentials**, klik **+ CREATE CREDENTIALS**, dan pilih **OAuth client ID**.
3.  Pilih **Web application** sebagai tipe aplikasi.
4.  Pada **Authorized JavaScript origins**, tambahkan URI server lokal Anda (misalnya `http://localhost:8080` atau `http://127.0.0.1:5500`).
5.  Salin **Client ID** yang dihasilkan.
6.  Buat Google Spreadsheet baru, atur 4 sheet di dalamnya sesuai dengan struktur yang dijelaskan di atas. Salin **ID Spreadsheet** dari URL (bagian antara `.../d/` dan `/edit...`).

### 3. Konfigurasi Kode

1.  Buka file `index.html`.
2.  Temukan dan ganti nilai variabel berikut di dalam tag `<script type="module">`:
    ```javascript
    const GOOGLE_CLIENT_ID = 'GANTI_DENGAN_CLIENT_ID_ANDA';
    const SPREADSHEET_ID = 'GANTI_DENGAN_ID_SPREADSHEET_ANDA';
    ```
3.  (Opsional) Jika Anda ingin menggunakan fitur pencatatan ke spreadsheet individual, isi objek `awardeeSpreadsheetIds`:
    ```javascript
    const awardeeSpreadsheetIds = {
        "NamaUsernameAwardee1": "ID_SPREADSHEET_AWARDDEE_1",
        "NamaUsernameAwardee2": "ID_SPREADSHEET_AWARDDEE_2"
    };
    ```

### 4. Menjalankan Proyek

Karena proyek ini menggunakan Google OAuth, file `index.html` harus dijalankan melalui web server, bukan dengan membukanya langsung (`file:///...`).

**Cara Mudah (Menggunakan VS Code):**

1.  Instal ekstensi **Live Server**.
2.  Klik kanan pada file `index.html` dan pilih **Open with Live Server**.

**Cara Alternatif (Menggunakan http-server):**

1.  Buka terminal di direktori proyek.
2.  Jalankan perintah: `npx http-server -p 8080`
3.  Buka browser dan akses `http://localhost:8080`.
