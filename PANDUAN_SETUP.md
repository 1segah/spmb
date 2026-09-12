# PANDUAN INTEGRASI & DEPLOYMENT SPMB ONLINE
## SMP NEGERI 1 SEGAH (KABUPATEN BERAU, KALIMANTAN TIMUR)

Aplikasi web SPMB ini dibangun dengan arsitektur **Serverless & Jamstack Modern**:
- **Frontend**: HTML5, CSS3 Modern, Vanilla JavaScript (ringan, tanpa dependensi framework Node/PHP, 100% kompatibel di GitHub Pages).
- **Backend API**: Google Apps Script (REST Web App dengan penanganan CORS anti-preflight).
- **Database Utama**: Google Sheets terhubung langsung via `SPREADSHEET_ID`.
- **Penyimpanan Berkas**: Google Drive terhubung langsung via `DRIVE_FOLDER_ID`.

---

## 📌 IDENTITAS TARGET DATABASE TERHUBUNG

Konfigurasi backend telah disesuaikan langsung dengan ID yang Anda tentukan:

| Sumber Daya | ID Target | Tautan Langsung |
| :--- | :--- | :--- |
| **Google Spreadsheet** | `1E6W7R3eiC9d7hHxPeTz13eKjN88zqJxCaEyc8Z35diQ` | [Buka Google Spreadsheet](https://docs.google.com/spreadsheets/d/1E6W7R3eiC9d7hHxPeTz13eKjN88zqJxCaEyc8Z35diQ/edit) |
| **Folder Dokumen Drive** | `1HpLSQYs2LCsnJ6OnvKh2C4SxHJvZc9RP` | [Buka Folder Google Drive](https://drive.google.com/drive/folders/1HpLSQYs2LCsnJ6OnvKh2C4SxHJvZc9RP) |

---

## 🚀 LANGKAH AKTIVASI GOOGLE APPS SCRIPT (BACKEND)

### Langkah 1: Buka Editor Apps Script
1. Buka Google Spreadsheet target: [1E6W7R3eiC9d7hHxPeTz13eKjN88zqJxCaEyc8Z35diQ](https://docs.google.com/spreadsheets/d/1E6W7R3eiC9d7hHxPeTz13eKjN88zqJxCaEyc8Z35diQ/edit).
2. Di menu atas, klik **Extensions (Ekstensi)** > **Apps Script**.

### Langkah 2: Masukkan Kode Apps Script
Pilih salah satu metode berikut:
- **Metode 1 (Satu File - Paling Praktis)**:
  1. Buka file `google-apps-script/Code_AllInOne.gs` di folder proyek ini.
  2. Salin seluruh kodenya.
  3. Hapus isi file `Code.gs` di editor Apps Script Anda, lalu tempelkan (paste).
- **Metode 2 (Modular / Terpisah)**:
  1. Salin masing-masing file dari folder `google-apps-script/` ke editor Apps Script:
     `Config.gs`, `Utils.gs`, `Database.gs`, `Pendaftaran.gs`, `Dokumen.gs`, `Auth.gs`, `Seleksi.gs`, `Pengumuman.gs`, dan `Code.gs`.

### Langkah 3: Atur Izin Folder Google Drive
1. Buka Google Drive folder: [1HpLSQYs2LCsnJ6OnvKh2C4SxHJvZc9RP](https://drive.google.com/drive/folders/1HpLSQYs2LCsnJ6OnvKh2C4SxHJvZc9RP).
2. Klik nama folder di bagian atas > **Share (Bagikan)**.
3. Pada bagian *General access (Akses umum)*, pastikan diatur ke **Anyone with the link (Siapa saja yang memiliki link)** dengan peran **Viewer (Pelihat)** agar dokumen pendaftar (KK, Akta, SKL) dapat dibuka oleh panitia seleksi.

### Langkah 4: Jalankan Inisialisasi Database
1. Di editor Apps Script, pada menu drop-down pilihan fungsi di toolbar atas, pilih: **`setupDatabase`**.
2. Klik tombol **Run (Jalankan)** (ikon ▶).
3. Muncul jendela *Authorization Required*:
   - Klik **Review permissions**
   - Pilih akun Google Anda
   - Klik tautan **Advanced** di kiri bawah
   - Klik **Go to SPMB (unsafe)**
   - Klik **Allow**
4. Fungsi `setupDatabase()` akan otomatis membuat 8 sheet lengkap beserta header kolom dan styling resmi:
   - `PELAKSANAAN`
   - `PENDAFTAR`
   - `ORANG_TUA`
   - `PRESTASI`
   - `DOKUMEN`
   - `NILAI_RAPOR`
   - `PENGUMUMAN`
   - `ADMIN` (akun awal: `admin` / password: `admin123`)

### Langkah 5: Deploy sebagai Web App
1. Di pojok kanan atas Apps Script, klik tombol biru **Deploy** > **New deployment**.
2. Klik ikon gerigi di sebelah *Select type* > pilih **Web app**.
3. Atur konfigurasi berikut:
   - **Description**: `SPMB SMPN 1 Segah v1.0`
   - **Execute as**: `Me (<email-anda>)`
   - **Who has access**: **`Anyone`** *(Sangat penting: wajib dipilih 'Anyone' agar formulir pendaftaran dari calon murid dapat mengirim data tanpa perlu login akun Google).*
4. Klik **Deploy**.
5. Salin **Web app URL** yang muncul (format: `https://script.google.com/macros/s/AKfycb.../exec`).

### Langkah 6: Sambungkan URL ke Aplikasi SPMB
Anda memiliki 2 cara mudah:
- **Cara A (Melalui Panel Admin SPMB - Tanpa Edit Kode)**:
  1. Buka web SPMB, masuk ke menu **Login Admin** (`login.html`).
  2. Masuk dengan username: `admin` / password: `admin123`.
  3. Buka tab **Pengaturan SPMB** di sidebar kiri.
  4. Pada bagian *Integrasi Google Sheets & Drive*, tempelkan URL Web App Anda ke kolom **URL Web App Google Apps Script**.
  5. Klik tombol **Tes Koneksi** untuk memastikan status hijau, lalu klik **Simpan URL**.
- **Cara B (Melalui File `js/config.js`)**:
  1. Buka file `js/config.js`.
  2. Ganti baris:
     ```javascript
     const API_URL = "";
     ```
     Menjadi:
     ```javascript
     const API_URL = "https://script.google.com/macros/s/AKfycb.../exec";
     ```

---

## 🌐 INTEGRASI & PUBLISH KE GITHUB PAGES

Aplikasi ini telah disiapkan dengan Git dan alur kerja otomatis GitHub Actions untuk GitHub Pages:

### 1. Buat Repositori di GitHub
1. Masuk ke [GitHub.com](https://github.com) dan klik **New Repository**.
2. Beri nama repositori, misalnya: `spmb` (atau `spmb-smpn1segah`).
3. Pilih **Public**, lalu klik **Create repository**.

### 2. Hubungkan Repositori Lokal dan Push
Buka terminal / PowerShell pada folder proyek ini (`E:\spmb`), lalu jalankan perintah:

```powershell
# Inisialisasi git dan branch main
git branch -M main

# Tambahkan alamat repositori GitHub Anda
git remote add origin https://github.com/USERNAME/spmb.git

# Push seluruh berkas ke GitHub
git push -u origin main
```
*(Ganti `USERNAME` dengan nama akun GitHub Anda).*

### 3. Aktifkan GitHub Pages
1. Pada halaman repositori GitHub Anda, buka menu **Settings** > **Pages** di sebelah kiri.
2. Di bagian **Build and deployment**:
   - Jika ingin menggunakan GitHub Actions yang sudah disediakan: pada dropdown **Source**, pilih **GitHub Actions**. Alur kerja di `.github/workflows/deploy.yml` akan langsung mempublikasikan situs Anda.
   - Atau cara klasik: pada dropdown **Source**, pilih **Deploy from a branch** > branch **main** > folder **/(root)** > klik **Save**.
3. Dalam 1 - 2 menit, web SPMB Anda siap diakses secara publik di:
   ```
   https://USERNAME.github.io/spmb/
   ```

---

## 🔄 BAGAIMANA PEMBAHARUAN PENDAFTARAN ONLINE BEKERJA?

1. **Pengisian Formulir oleh Calon Murid**:
   - Calon murid mengisi formulir di `pendaftaran.html`.
   - File dokumen (KK, Akta Kelahiran, SKL) dikonversi ke Base64 dan dikirim dengan metode aman non-preflight (`text/plain;charset=utf-8`).
   - Google Apps Script menerima data, otomatis membuat file di folder Google Drive `1HpLSQYs2LCsnJ6OnvKh2C4SxHJvZc9RP`, dan mencatat link URL-nya ke baris sheet `DOKUMEN`.
   - Data diri lengkap masuk ke sheet `PENDAFTAR` dan data orang tua ke sheet `ORANG_TUA` pada Spreadsheet `1E6W7R3eiC9d7hHxPeTz13eKjN88zqJxCaEyc8Z35diQ`.
   - Calon murid langsung menerima Nomor Pendaftaran unik (misal: `SPMB-2026-0001`) dan dapat mencetak Bukti Pendaftaran A4 ber-QR Code.

2. **Pengecekan Status Real-Time**:
   - Murid atau orang tua membuka `cek-status.html`, memasukkan Nomor Pendaftaran dan NISN / Tanggal Lahir.
   - Frontend memanggil Google Apps Script untuk mengambil status verifikasi berkas, kelulusan, dan catatan panitia secara real-time dari Google Sheets.

3. **Verifikasi & Seleksi oleh Admin Sekolah**:
   - Panitia membuka `admin.html` untuk memverifikasi keabsahan dokumen berkas yang terunggah ke Drive.
   - Status diubah di panel admin (Terverifikasi / Perlu Perbaikan / Lulus / Tidak Lulus) dan otomatis terupdate ke Google Sheets.
   - Perubahan status langsung tampak saat calon murid mengecek statusnya di `cek-status.html`.
