/*
NAMA   : Christian Junior Panjaitan
NIM    : 41426027
PRODI  : D4 Teknologi Rekayasa Perangkat Lunak
*/

1. TUJUAN
Saya menggabungkan halaman dari minggu-minggu sebelumnya menjadi situs tiga halaman: beranda, kegiatan, dan daftar. Data yang dipakai fiktif. Hasil yang saya targetkan di Sesi 2:
- tiga halaman yang berjalan dengan title berbeda dan satu stylesheet bersama,
- navigasi yang konsisten dengan aria-current pada halaman aktif,
- path dan aset yang benar, tanpa 404,
- audit keyboard, ukuran 320px dan 1200px, zoom, validasi HTML, dan link pada setiap halaman,
- bekerja di branch lalu merge tanpa menghilangkan perubahan.
Simulasi pull request dan review akan saya kerjakan di Sesi 3.

2. CARA MENJALANKAN
1. Buka folder pswi-week-07/sesi-02 di VS Code.
2. Klik kanan index.html lalu pilih Open with Live Server.
3. Halaman terbuka lewat alamat server lokal (contoh: http://127.0.0.1:5500/). Tekan F12 untuk membuka DevTools, lalu Ctrl+Shift+M untuk mode responsif.

Susunan folder:

```
pswi-week-07/
  sesi-02/
    index.html
    kegiatan.html
    daftar.html
    css/
      style.css
    assets/
      foto-320.jpg
      foto-640.jpg
    main.js
    README.md
    bukti/
```

3. PERSIAPAN
Sebelum saya mulai, saya membuat folder baru pswi-week-07/sesi-02, lalu menyalin hasil Minggu 6 Sesi 3 (halaman, style.css, main.js, dan folder assets) ke dalamnya tanpa mengubah folder minggu sebelumnya. Setelah itu saya membuat README.md dan folder bukti, menjalankan server lokal seperti di Minggu 1, lalu memeriksa Git dengan git --version dan git status. Hasilnya muncul versi Git (contoh git version 2.43.0) dan git status berjalan tanpa error, jadi repository sudah ada dan saya tidak menjalankan git init lagi.

4. MENYIAPKAN HALAMAN
Isi satu halaman dari Minggu 6 saya bagi menjadi tiga halaman tanpa membuang struktur yang sudah benar:
- index.html (Beranda): judul sambutan, foto, profil singkat, dan tombol salam.
- kegiatan.html: katalog card kegiatan beserta tautan detailnya.
- daftar.html: form pendaftaran kegiatan.
Stylesheet saya pindahkan ke css/style.css dan dipakai bersama oleh ketiga halaman. Bagian head semua halaman sama polanya, hanya title yang berbeda:

```
<!doctype html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Kegiatan Mahasiswa - Pengembangan Situs Web I</title>
  <link rel="stylesheet" href="css/style.css">
</head>
```

| Halaman | title | h1 |
| :--- | :--- | :--- |
| index.html | Beranda - Pengembangan Situs Web I | Selamat Datang di situs saya |
| kegiatan.html | Kegiatan Mahasiswa - Pengembangan Situs Web I | Kegiatan Mahasiswa |
| daftar.html | Pendaftaran Kegiatan - Pengembangan Situs Web I | Pendaftaran Kegiatan |

Catatan perubahan saat memecah halaman:
- Tiap halaman memakai tepat satu h1. Dulu ada tiga h1 dalam satu halaman, dan sekarang masing-masing ada di halaman sendiri.
- Setiap halaman memakai lang="id" dan meta viewport.
- Saat memindahkan form ke daftar.html saya melihat input nama belum punya atribut name, sehingga isinya tidak ikut terkirim. Saya menambahkan name="nama".
Hasil: ketiga halaman terbuka lewat Live Server (alamat .../sesi-02/index.html, .../kegiatan.html, dan .../daftar.html), tampil dengan gaya yang sama, dan Console tidak menampilkan error. Title yang berbeda terlihat pada tab browser.

5. NAVIGASI KONSISTEN
Saya menyalin pola navigasi yang sama ke semua halaman. Daftar ul dan li dari Minggu 5 tetap saya pakai supaya aturan .nav-list (flex) tidak berubah. Atribut aria-current="page" saya pindahkan sesuai halaman. Contoh pada kegiatan.html:

```
<nav aria-label="Navigasi utama">
  <ul class="nav-list">
    <li><a href="index.html">Beranda</a></li>
    <li><a href="kegiatan.html" aria-current="page">Kegiatan</a></li>
    <li><a href="daftar.html">Daftar</a></li>
  </ul>
</nav>
```

Tautan Kontak (#kontak) dari navigasi lama saya ganti dengan pola tiga halaman ini. Kontak tetap ada di footer setiap halaman. Supaya halaman aktif juga terlihat, saya menambahkan di css/style.css:

```
a[aria-current="page"] { font-weight: bold; }
```

Hasil pengamatan:
1. Klik semua tujuan: saya mengklik tiga tujuan dari setiap halaman (9 klik) dan semuanya membuka halaman yang benar. Mengklik tautan halaman yang sedang dibuka hanya memuat ulang halaman itu.
2. Halaman aktif saja: di Console, perintah `document.querySelectorAll('[aria-current="page"]')` pada tiap halaman menghasilkan tepat satu elemen, yaitu Beranda di index.html, Kegiatan di kegiatan.html, dan Daftar di daftar.html. Tautan aktif tampil tebal.
3. Back dan Forward: saya membuka Beranda, lalu Kegiatan, lalu Daftar. Tombol Back membawa saya ke Kegiatan lalu Beranda, dan Forward membawa kembali ke Kegiatan. Setiap halaman memuat benar dan aria-current mengikuti halaman yang tampil.

6. PATH DAN ASET
Semua halaman berada di folder yang sama, jadi path yang sama berlaku untuk semua halaman:

```
<link rel="stylesheet" href="css/style.css">
<img src="assets/foto-640.jpg" srcset="assets/foto-320.jpg 320w, assets/foto-640.jpg 640w"
     sizes="(min-width: 48rem) 50vw, 92vw" width="640" height="360" alt="...">
```

Kalau suatu halaman nanti berada di subfolder, path harus naik satu tingkat (misalnya ../css/style.css).
Hasil pengamatan:
1. Network: di tiga halaman, css/style.css, assets/foto-320.jpg atau foto-640.jpg (di beranda), dan main.js (hanya di beranda) semuanya berstatus 200, tidak ada 404. Aset termuat melalui path web.
2. Kapitalisasi: saya mengubah src menjadi assets/Foto-640.jpg (huruf F besar). Di komputer saya (Windows) gambar tetap tampil dengan status 200, karena sistem file Windows tidak membedakan huruf besar dan kecil. Di server Linux nama itu akan menghasilkan 404. Karena itu saya menulis semua nama file huruf kecil dan persis sama antara nama file dan path. Setelah percobaan, nama saya kembalikan.
3. Path drive Windows: saya mengganti src menjadi C:\Users\nama\foto-640.jpg. Gambar tidak tampil dan Console menampilkan "Not allowed to load local resource: file:///C:/Users/nama/foto-640.jpg", karena halaman dibuka lewat http dan browser tidak boleh membuka file lokal. Path seperti itu juga hanya ada di komputer saya sehingga tidak akan bekerja di komputer lain. Saya kembalikan ke path web assets/foto-640.jpg.

7. AUDIT KESELURUHAN
Saya membuat tabel audit untuk setiap halaman. Ukuran kecil dan besar saya uji di mode responsif (320px dan 1200px), zoom 200% di jendela 1200px (tampilan menjadi 600px), keyboard dengan Tab, dan validasi HTML dengan Nu HTML Checker. Ada tidaknya scroll ke samping saya periksa dengan perintah `document.documentElement.scrollWidth > document.documentElement.clientWidth` di Console (false berarti tidak ada).

| Halaman | Kasus | Harapan | Hasil nyata | Status |
| :--- | :--- | :--- | :--- | :--- |
| Beranda | Keyboard | Fokus berurutan dan terlihat | Tab melewati Beranda, Kegiatan, Daftar, lalu tombol salam. Garis fokus terlihat, dan Enter pada tombol memunculkan pesan salam | Lulus |
| Beranda | 320px | Konten lengkap, tanpa scroll ke samping | Judul, foto, profil singkat, dan tombol tampil, Console menghasilkan false | Lulus |
| Beranda | 1200px | Tampilan rapi | Konten di tengah dengan lebar terbatas, foto 640 x 360px | Lulus |
| Beranda | Zoom 200% | Konten dan interaksi tersedia | Teks dua kali lebih besar, tidak terpotong, tombol dan tautan tetap bisa dipakai | Lulus |
| Beranda | Validasi HTML | Tanpa error | "Document checking completed. No errors or warnings to show." | Lulus |
| Beranda | Link | Tujuan benar | Tiga tautan navigasi membuka halaman yang benar | Lulus |
| Kegiatan | Keyboard | Fokus berurutan dan terlihat | Tab melewati tiga tautan navigasi lalu lima tautan detail card berurutan, garis fokus terlihat | Lulus |
| Kegiatan | 320px | Konten lengkap, tanpa scroll ke samping | Lima card tersusun satu kolom, Console menghasilkan false | Lulus |
| Kegiatan | 1200px | Tampilan rapi | Card tersusun dua kolom | Lulus |
| Kegiatan | Zoom 200% | Konten dan interaksi tersedia | Satu kolom (tampilan 600px), teks tidak terpotong | Lulus |
| Kegiatan | Validasi HTML | Tanpa error | "No errors or warnings to show" | Lulus |
| Kegiatan | Link | Tujuan benar | Awalnya satu tautan gagal: tautan Daftar tertulis daftar.htm dan menghasilkan 404. Saya perbaiki menjadi daftar.html. Lima tautan detail masih href="#" (tidak 404, hanya kembali ke atas halaman) | Awalnya gagal, setelah perbaikan lulus |
| Daftar | Keyboard | Fokus berurutan dan terlihat | Tab melewati tiga tautan navigasi, nama, email, program studi, catatan, dan tombol Daftar. Enter pada tombol dengan form kosong memunculkan pesan wajib diisi dan fokus pindah ke kolom nama | Lulus |
| Daftar | 320px | Konten lengkap, tanpa scroll ke samping | Label panjang turun beberapa baris, semua kontrol di dalam wadah, Console menghasilkan false | Lulus |
| Daftar | 1200px | Tampilan rapi | Form di tengah dengan lebar terbatas | Lulus |
| Daftar | Zoom 200% | Konten dan interaksi tersedia | Teks terbaca, semua kontrol bisa dipakai | Lulus |
| Daftar | Validasi HTML | Tanpa error | "No errors or warnings to show" | Lulus |
| Daftar | Link | Tujuan benar | Tiga tautan navigasi membuka halaman yang benar | Lulus |
| Semua | CSS (css/style.css) | Tanpa error | W3C CSS Validator hanya menampilkan dua pesan: container-type dan @container tidak dikenal. Keduanya karena validator belum mendukung container query, bukan karena kodenya salah. Aturan lain tanpa error | Lulus dengan catatan |

8. LATIHAN MANDIRI DAN TABEL UJI
Saya mengulang percobaan di atas sendiri dan menulis prediksi sebelum menjalankannya. Untuk data tambahan, saya mengganti teks tautan Daftar di navigasi dengan teks yang sangat panjang ("Pendaftaran Kegiatan Mahasiswa Baru Angkatan 2026"), lalu mengembalikannya setelah uji.

| Kasus | Tindakan | Harapan | Hasil aktual | Status |
| :--- | :--- | :--- | :--- | :--- |
| Normal: semua link | Klik tiga tujuan dari tiap halaman (9 klik) | Tujuan benar | Semua membuka halaman yang benar | Lulus |
| Normal: halaman aktif | Cek aria-current di tiap halaman | aria-current hanya sesuai halaman | Tepat satu elemen per halaman, sesuai halaman yang dibuka | Lulus |
| Normal: Back dan Forward | Beranda, Kegiatan, Daftar, lalu Back dua kali dan Forward satu kali | Halaman dan halaman aktif mengikuti | Sesuai, tidak ada halaman yang salah | Lulus |
| Normal: aset | Lihat Network di tiga halaman | Tidak 404 | Semua aset berstatus 200 | Lulus |
| Normal: zoom dan keyboard | Zoom 200% dan Tab di tiap halaman | Konten dan interaksi tersedia | Semua tersedia, lihat tabel audit di bagian 7 | Lulus |
| Batas: kapitalisasi nama file | src menjadi assets/Foto-640.jpg | Prediksi: tetap tampil di Windows | Benar, status 200 di komputer saya, tetapi di server Linux akan 404 | Lulus |
| Batas: teks tautan panjang | Teks tautan Daftar dibuat panjang, lebar 320px | Prediksi: tautan turun baris tanpa melebihi layar | Benar, aturan flex-wrap membuat tautan turun ke baris baru dan Console menghasilkan false | Lulus |
| Gagal: link salah ketik | Ubah href jadi daftar.htm di kegiatan.html | Prediksi: 404 | Benar, halaman tidak ditemukan (404). Setelah diperbaiki, membuka daftar.html | Awalnya gagal, setelah perbaikan lulus |
| Gagal: path CSS salah | Ubah link di daftar.html jadi href="style.css" | Prediksi: halaman tanpa gaya | Benar, Network menampilkan 404 untuk style.css dan halaman tampil polos (tanpa warna, kartu, dan navigasi berjajar). Setelah dikembalikan ke css/style.css, gaya muncul lagi | Awalnya gagal, setelah perbaikan lulus |
| Gagal: path drive Windows | src berisi C:\Users\nama\foto-640.jpg | Prediksi: gambar tidak tampil | Benar, Console menampilkan Not allowed to load local resource | Awalnya gagal, setelah perbaikan lulus |
| Gagal: main.js di halaman tanpa tombol | Tambahkan script main.js di kegiatan.html | Prediksi: error di Console | Benar, muncul TypeError "Cannot read properties of null (reading 'addEventListener')" karena tombol salam tidak ada di halaman itu. Setelah script dihapus dari kegiatan.html (script hanya di index.html), error hilang | Awalnya gagal, setelah perbaikan lulus |
| Gagal: aria-current salah | Salin nav dari index.html ke kegiatan.html tanpa mengubah aria-current | Prediksi: Beranda masih tampak aktif | Benar, Console menunjukkan elemen aktif bertuliskan Beranda di halaman Kegiatan. Setelah atribut dipindah ke Kegiatan, sesuai | Awalnya gagal, setelah perbaikan lulus |

Lulus berarti hasil nyata sama dengan harapan atau prediksi. Percobaan gagal sengaja saya buat untuk melihat dampaknya, lalu semuanya saya kembalikan.
Saya juga meminta teman memeriksa satu alasan teknis, yaitu kenapa main.js hanya boleh dipasang di halaman yang punya tombol salam. Teman saya setuju bahwa skrip itu mencari elemen #greeting-button, dan di halaman tanpa elemen itu hasilnya null sehingga addEventListener gagal.
Bukti tambahan: bukti/28-css-path-salah.png, bukti/29-script-null-error.png, bukti/30-aria-current-salah.png, bukti/31-nav-teks-panjang-320.png

9. PERBANDINGAN BEFORE-AFTER
| Hal yang diubah | Sebelum (Minggu 6) | Sesudah (Minggu 7) |
| :--- | :--- | :--- |
| Struktur | Semua isi dalam satu index.html | Tiga halaman: index, kegiatan, daftar |
| Stylesheet | style.css di folder utama | css/style.css dipakai bersama oleh semua halaman |
| Navigasi | Tautan fragmen #kegiatan dan #kontak | Tiga tautan antar halaman dengan aria-current |
| Heading | Tiga h1 dalam satu halaman | Satu h1 per halaman |
| Aturan .card | Ditulis berulang dan masih ada sisa percobaan lebar | Satu aturan .card yang rapi |
| main.js | Dihubungkan di satu halaman besar | Hanya dipasang di index.html |
| Input nama | Tanpa name | name="nama" |
| Validasi HTML | Satu halaman | Tiga halaman, masing-masing tanpa error |


| 32-git-branch-merge.png | Hasil git merge dan git log --oneline --graph |
| 33-git-commit.png | Hasil git status, git diff, dan git commit |

11. GIT: BRANCH, MERGE, DAN SETORAN
Supaya perubahan integrasi tidak merusak hasil sebelumnya, saya mengerjakannya di branch terpisah, lalu menggabungkannya ke main. Saya tidak memakai git reset --hard atau perintah penghapusan luas.

```
git switch -c integrasi-multipage
git status
git diff
git add .
git commit -m "PSWI Week 7 Sesi 2: latihan dan pengujian"
git switch main
git merge integrasi-multipage
git log --oneline --graph
```

Hasil yang saya amati:
- git switch -c: muncul "Switched to a new branch 'integrasi-multipage'".
- git status dan git diff: git diff hanya menampilkan perubahan pada file lama, sedangkan file baru (kegiatan.html, daftar.html, dan isi css/ dan assets/) terlihat di git status sebagai untracked. Saya pastikan tidak ada file rahasia, data pribadi, atau file sementara yang besar.
- git commit: muncul baris seperti [integrasi-multipage a1b2c3d] PSWI Week 7 Sesi 2: latihan dan pengujian beserta jumlah file yang berubah.
- git switch main lalu git merge: muncul "Fast-forward" dengan daftar file yang digabung, karena main belum berubah sejak branch dibuat.
- git log --oneline --graph: commit integrasi tampak di main dan di branch integrasi-multipage pada commit yang sama.
Perubahan tidak hilang: setelah merge, file di main sama dengan di branch (git diff main integrasi-multipage tidak menampilkan apa pun) dan ketiga halaman tetap bisa dibuka.
Berkas setoran adalah pswi_4141103_w07s02_41426027.zip yang berisi folder sesi-02 (index.html, kegiatan.html, daftar.html, css, assets, main.js, README.md, dan bukti).
Sumber aset: assets/foto-640.jpg dan assets/foto-320.jpg adalah foto dari situs IT Del serta pintrest.

12. AI USE STATEMENT
Alat: Claude (Anthropic). Tujuan: memahami isi modul dan menyusun README. Bagian yang dibantu: susunan README, bahasa penjelasan, dan contoh alur Git untuk branch dan merge. Verifikasi: semua percobaan saya jalankan sendiri di browser, DevTools, Nu HTML Checker, dan terminal, lalu saya cocokkan dengan tabel uji. Saya tidak memasukkan data pribadi atau password ke AI dan saya bisa menjelaskan kode ini saat walkthrough.
