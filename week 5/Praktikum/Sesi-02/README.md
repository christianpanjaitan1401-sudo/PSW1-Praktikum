/*
NAMA   : Christian Junior Panjaitan
NIM    : 41426027
PRODI  : D4 Teknologi Rekayasa Perangkat Lunak
*/

1. TUJUAN
Saya membuat halaman katalog kegiatan mahasiswa dengan navigasi dan card yang ditata memakai Flexbox. Data yang dipakai fiktif. Hasil yang saya targetkan di Sesi 2:
- menjelaskan normal flow dan memilih Flexbox atau Grid sesuai kebutuhan,
- menyusun navigasi dan card dengan Flexbox,
- menguji tata letak dengan konten panjang dan ukuran layar yang berbeda.
Bagian membuat grid responsif dan pola class yang bisa dipakai ulang akan saya kerjakan di Sesi 3.

2. CARA MENJALANKAN
1. Buka folder pswi-week-05/sesi-02 di VS Code.
2. Klik kanan index.html lalu pilih Open with Live Server.
3. Halaman terbuka lewat alamat server lokal (contoh: http://127.0.0.1:5500/). Tekan F12 untuk membuka DevTools, lalu Ctrl+Shift+M untuk mode responsif.

Susunan folder:

```
pswi-week-05/
  sesi-02/
    index.html
    style.css
    README.md
    bukti/
```

3. PERSIAPAN
Sebelum saya mulai, saya membuat folder baru pswi-week-05/sesi-02 tanpa mengubah folder minggu sebelumnya. Lalu saya membuat index.html, style.css, README.md, dan folder bukti. Setelah itu saya menjalankan server lokal seperti di Minggu 1 dan memeriksa Git dengan git --version dan git status. Hasilnya muncul versi Git (contoh git version 2.43.0) dan git status berjalan tanpa error, jadi repository sudah ada dan saya tidak menjalankan git init lagi.
Bukti: bukti/01-git-version-status.png

4. STARTER KATALOG
Saya membuat index.html dengan struktur minimum dari Minggu 2 (doctype, html lang="id", meta charset, meta viewport, title, dan link ke style.css). Di dalam body saya memasukkan navigasi, tiga card, dan footer:

```
<nav aria-label="Navigasi utama">
  <ul class="nav-list">
    <li><a href="#kegiatan">Kegiatan</a></li>
    <li><a href="#kontak">Kontak</a></li>
  </ul>
</nav>
<main id="kegiatan">
  <h1>Kegiatan Mahasiswa</h1>
  <div class="cards">
    <article class="card"><h2>Lokakarya HTML</h2><p>Belajar struktur.</p></article>
    <article class="card"><h2>Diskusi CSS</h2><p>Belajar tampilan.</p></article>
    <article class="card"><h2>Audit Web</h2><p>Menguji kualitas.</p></article>
  </div>
</main>
<footer id="kontak">Kontak: kegiatan@example.com</footer>
```

Di head saya menambahkan `<link rel="stylesheet" href="style.css">`. Di style.css saya baru menuliskan reset dari Minggu 4: `*, *::before, *::after { box-sizing: border-box; }`.

Hasil pemeriksaan:
- Struktur dan heading: ada satu h1 ("Kegiatan Mahasiswa") dan tiga h2 (judul tiap card), urutannya tidak melompat. Tiap card memakai article dengan heading sendiri.
- Tautan fragment: saat tautan Kegiatan diklik pengguna langsung diarahkan ke bagian kegiatan, dan saat Kontak diklik pengguna langsung diarahkan ke bagian kontak. Itu cocok dengan id="kegiatan" pada main dan id="kontak" pada footer. Karena halaman masih pendek, tampilan hanya sedikit bergeser, jadi buktinya terutama perubahan URL(pengguna diarah kan kebagian alamat kegiatan dan kontak).
- Nu HTML Checker: "Document checking completed. No errors or warnings to show." Jadi starter valid dengan tiga card.
- Normal flow (sebelum Flexbox): daftar tautan tampil bertumpuk ke bawah lengkap dengan bullet, dan tiga card bertumpuk dari atas ke bawah dengan lebar penuh.

5. NORMAL FLOW DAN PILIHAN FLEXBOX ATAU GRID
- Normal flow adalah susunan bawaan browser sebelum ada CSS tata letak. Elemen block (seperti li, article, dan div) tersusun dari atas ke bawah satu per baris, sedangkan elemen inline (seperti teks dan a) mengalir di dalam baris.
- Flexbox mengatur susunan dalam satu dimensi, yaitu satu baris atau satu kolom, dan bisa wrap. Karena itu cocok untuk navigasi dan deretan card yang jumlah per barisnya mengikuti ruang.
- Grid mengatur dua dimensi sekaligus (baris dan kolom). Karena itu cocok untuk tata letak dengan kolom yang rapi dan sejajar.
Di sesi ini saya memakai Flexbox untuk navigasi dan card. Wadah card akan saya ubah menjadi Grid di Sesi 3.

6. NAVIGASI FLEKSIBEL
Saya mengaktifkan Flexbox pada daftar, bukan pada setiap li:

```
.nav-list { display: flex; flex-wrap: wrap; gap: 1rem;
            list-style: none; padding: 0; }
```

Hasilnya, tautan Kegiatan dan Kontak berjajar mendatar, jaraknya 1rem (16px), dan bullet serta indentasi daftar hilang karena list-style: none dan padding: 0.
Flexbox dipasang pada ul karena Flexbox hanya mengatur anak langsungnya. Kalau display: flex dipasang pada tiap li, yang diatur hanya isi li (tautan a), sedangkan li tetap bertumpuk seperti normal flow.

Pengamatan sumbu:
- flex-direction: row (bawaan): sumbu utama mendatar, tautan berjajar dari kiri ke kanan.
- flex-direction: column (saya coba sementara): sumbu utama menjadi vertikal, tautan bertumpuk dari atas ke bawah dengan jarak gap yang sama.
- Setelah itu saya kembalikan ke row.

7. CARD FLEKSIBEL
Saya mengatur basis dan jarak card (box-sizing: border-box dari Minggu 4 sudah aktif):

```
.cards { display: flex; flex-wrap: wrap; gap: 1rem; }
.card { flex: 1 1 15rem; padding: 1rem;
        border: 1px solid #ccd; min-width: 0; }
```

Arti flex: 1 1 15rem adalah flex-grow 1 (boleh melebar membagi sisa ruang), flex-shrink 1 (boleh mengecil), dan flex-basis 15rem = 240px (ukuran awal). min-width: 0 membuat card boleh lebih kecil dari isinya, sehingga card tidak melebarkan wadah.

Saya mengujinya pada 360px dan 1200px dengan mode responsif. Lalu saya menambahkan card keempat ("Pameran Proyek", isi "Menampilkan hasil.") dan mencatat ukuran finalnya:

| Kondisi | Lebar wadah | Card per baris | Lebar card final |
| :--- | :--- | :--- | :--- |
| 1200px, 3 card | sekitar 1184px | 3 | sekitar 384px |
| 1200px, 4 card | sekitar 1184px | 4 | sekitar 284px |
| 360px, 3 atau 4 card | sekitar 344px | 1 | sekitar 344px |

Penjelasan hitungan: wadah adalah lebar tampilan dikurangi margin bawaan body (8px kiri dan kanan). Satu baris muat 4 card kalau wadah minimal 4 x 240 + 3 x 16 = 1008px. Di 1200px wadah 1184px, sehingga card membagi sisa ruang sama rata: (1184 - 32) / 3 = 384px untuk 3 card, dan (1184 - 48) / 4 = 284px untuk 4 card. Di 360px muat 2 card butuh 496px, padahal wadah hanya 344px, jadi tiap card berdiri sendiri satu baris dan melebar penuh ke 344px lewat flex-grow. Tidak ada scroll ke samping. Ukuran dan jumlah baris memang menyesuaikan ruang.
Bukti: bukti/08-card-1200-tiga.png, bukti/09-card-360-wrap.png, bukti/10-card-empat-1200.png, bukti/11-card-empat-360.png

8. KONTEN PANJANG DAN PENGUKURAN
Saya mengganti judul satu card dengan teks yang sangat panjang tanpa spasi (sekitar 50 huruf, misalnya "LokakaryaPengembanganSitusWebDasarUntukMahasiswaBaru"). Prediksi saya, sebelum aturan overflow-wrap ditambahkan, teks akan keluar dari kotak card.
- Hasil nyata: benar, teks keluar melewati tepi kanan card karena kata tanpa spasi tidak dipecah. Card sendiri tetap selebar 284px (di 1200px) karena ada min-width: 0.
- Perbaikan: saya menambahkan `.card h2 { overflow-wrap: anywhere; }`. Retest: judul terpecah ke beberapa baris di dalam card dan semua hurufnya tetap terlihat.

Saya mengaktifkan overlay Flexbox (klik badge flex di samping .cards pada tab Elements) lalu membandingkan flex-basis dengan ukuran final:
- flex-basis di CSS: 15rem = 240px (ukuran awal).
- Ukuran final di tab Computed: sekitar 284px pada 1200px dengan 4 card, dan sekitar 344px pada 360px.
- Ukuran final lebih besar dari basis karena flex-grow: 1 membagikan sisa ruang di baris itu sama rata.

Zoom 200% (di jendela 1200px, lebar tampilan menjadi 600px): card tersusun 2 kolom (sekitar 284px per card) dalam 2 baris, karena 3 card butuh 752px sedangkan wadah hanya sekitar 584px. Judul panjang tetap terpecah di dalam card, jadi tidak ada informasi yang terpotong.
Alasan teknis: min-width: 0 mencegah card melebar mengikuti isi, dan overflow-wrap: anywhere membuat teks panjang boleh dipecah di dalam card. Keduanya dibutuhkan, karena min-width: 0 saja membuat card tetap kecil tetapi teksnya tumpah keluar.
Setelah selesai, judul saya kembalikan ke teks asli, sedangkan aturan overflow-wrap tetap saya pertahankan.

9. LATIHAN MANDIRI DAN TABEL UJI
Saya mengulang percobaan di atas sendiri dan menulis prediksi sebelum menjalankannya. Untuk data tambahan, saya mengisi satu card dengan paragraf yang sangat panjang (sekitar 300 karakter berisi kata biasa), lalu mengembalikannya setelah uji.

| Kasus | Tindakan | Harapan | Hasil aktual | Status/bukti |
| :--- | :--- | :--- | :--- | :--- |
| Normal: starter | Periksa struktur dan Nu HTML Checker | Starter valid dengan tiga card | Satu h1 dan tiga h2, tidak ada error atau peringatan | Lulus, bukti 03 |
| Normal: tautan fragment | Klik Kegiatan lalu Kontak | URL ikut berubah | URL berubah menjadi #kegiatan dan #kontak | Lulus, bukti 04 |
| Normal: nav row | Lihat nav dengan flex-direction row | Tautan berjajar mendatar | Berjajar mendatar dengan jarak 16px | Lulus, bukti 05 |
| Normal: nav column | Ubah ke column sementara | Tautan bertumpuk | Bertumpuk vertikal, lalu dikembalikan ke row | Lulus, bukti 06 |
| Normal: 1200px | Lihat 3 card | Card sebaris menyesuaikan ruang | Tiga card sebaris, masing-masing sekitar 384px | Lulus, bukti 08 |
| Normal: 360px | Lihat 3 card | Card wrap tanpa overflow | Satu card per baris selebar sekitar 344px, tidak ada scroll ke samping | Lulus, bukti 09 |
| Batas: empat item | Tambah card keempat, uji 1200px dan 360px | Semua card tampil | Empat card sebaris (sekitar 284px) di 1200px, dan empat baris di 360px | Lulus, bukti 10 dan 11 |
| Batas: nav wrap | Tambah 4 tautan percobaan, lebar 360px | Item turun ke baris baru | Sebagian tautan turun ke baris kedua | Lulus, bukti 07 |
| Batas: zoom 200% | Zoom browser ke 200% | Konten tetap tersedia | Card 2 kolom, semua teks terlihat | Lulus, bukti 14 |
| Batas: paragraf panjang | Isi satu card dengan paragraf panjang | Prediksi: card di baris yang sama ikut setinggi itu, tanpa teks keluar | Benar, card satu baris sama tinggi (bawaan align-items: stretch), tidak ada teks keluar | Lulus, bukti 16 |
| Normal: Tab | Tekan Tab dari awal halaman | Link tetap dapat dicapai | Fokus ke Kegiatan lalu Kontak, garis fokus terlihat, urutan sama dengan tampilan | Lulus, bukti 15 |
| Gagal: judul panjang | Ganti judul dengan kata 50 huruf, tanpa overflow-wrap | Prediksi: teks keluar dari kotak | Benar, teks keluar dari tepi card. Setelah overflow-wrap: anywhere ditambahkan, teks tetap di dalam card | Awalnya gagal, setelah perbaikan lulus, bukti 12, 13, dan 14 |

Lulus berarti hasil nyata sama dengan harapan atau prediksi. Urutan Tab sama dengan tampilan karena saya tidak memakai order atau row-reverse, jadi urutan DOM tidak berubah.
Saya juga meminta teman memeriksa satu alasan teknis, yaitu kenapa card butuh min-width: 0. Teman saya setuju bahwa tanpa itu card tidak mau lebih sempit dari isinya, sehingga judul panjang akan melebarkan wadah.
Bukti tambahan: bukti/15-tab-fokus-link.png dan bukti/16-paragraf-panjang-stretch.png

10. PEMERIKSAAN HTML DAN BEFORE-AFTER
Setelah semua percobaan selesai (tautan percobaan dihapus, card keempat dan aturan overflow-wrap dipertahankan), saya memeriksa HTML akhir dengan Nu HTML Checker, hasilnya "Document checking completed. No errors or warnings to show." Bukti: bukti/17-nu-html-checker-final.png

| Hal yang diubah | Sebelum | Sesudah |
| :--- | :--- | :--- |
| Navigasi | Tautan bertumpuk dengan bullet | Berjajar mendatar, bisa wrap (flex) |
| Wadah card | Tiga card bertumpuk penuh lebar | Card berjajar, jumlah per baris mengikuti ruang (flex wrap) |
| Jumlah card | 3 | 4 |
| Judul panjang | Keluar dari card | Terpecah di dalam card (overflow-wrap) |

11. DAFTAR BUKTI
Semua berkas ada di folder bukti/:

| Berkas | Isi yang harus terlihat |
| :--- | :--- |
| 01-git-version-status.png | Hasil git --version dan git status |
| 02-starter-normal-flow.png | Starter tanpa Flexbox: nav dan card bertumpuk |
| 03-nu-html-checker-starter.png | Nu HTML Checker starter tanpa error |
| 04-tautan-fragment-kontak.png | URL berakhiran #kontak setelah tautan diklik |
| 05-nav-row.png | Nav mendatar (row) |
| 06-nav-column.png | Nav bertumpuk (column) |
| 07-nav-wrap-360.png | Tautan percobaan turun ke baris kedua di 360px |
| 08-card-1200-tiga.png | Tiga card sebaris di 1200px |
| 09-card-360-wrap.png | Card satu per baris di 360px |
| 10-card-empat-1200.png | Empat card sebaris di 1200px |
| 11-card-empat-360.png | Empat card bertumpuk di 360px |
| 12-judul-panjang-keluar.png | Judul panjang keluar dari card (sebelum perbaikan) |
| 13-judul-panjang-overlay-flexbox.png | Overlay Flexbox, flex-basis, dan ukuran final |
| 14-judul-panjang-zoom-200.png | Zoom 200% dengan judul terpecah di dalam card |
| 15-tab-fokus-link.png | Garis fokus pada tautan nav |
| 16-paragraf-panjang-stretch.png | Card satu baris sama tinggi |
| 17-nu-html-checker-final.png | Nu HTML Checker HTML akhir |
| 18-git-commit.png | Hasil git status, git diff, dan git commit |

12. GIT DAN SETORAN
Sebelum git add, saya cek perubahan dengan git status dan git diff, dan memastikan tidak ada file rahasia, data pribadi, atau file sementara yang besar. Lalu saya menjalankan:

```
git status
git diff
git add .
git commit -m "PSWI Week 5 Sesi 2: latihan dan pengujian"
```

Setelah commit muncul baris seperti [main a1b2c3d] PSWI Week 5 Sesi 2: latihan dan pengujian beserta jumlah file yang berubah. Bukti: bukti/18-git-commit.png
Berkas setoran adalah pswi_4141103_w05s02_41426027.zip yang berisi folder sesi-02 (index.html, style.css, README.md, dan folder bukti). Saya tidak memakai gambar, font, atau aset luar, jadi tidak ada sumber aset yang perlu dicatat.

13. PERSIAPAN SESI 3
HTML akan saya pertahankan sama. Yang berubah hanya wadah card, dari Flexbox menjadi Grid:

```
.cards { display: grid;
         grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
         gap: 1rem; }
```

Rencana perubahannya:
- Aturan flex: 1 1 15rem pada .card dihapus karena tidak berlaku pada item grid. min-width: 0, padding, dan border tetap.
- Navigasi tetap memakai Flexbox, karena hanya perlu satu baris yang bisa wrap.
- Saya akan menguji ulang empat kasus yang sama (360px, empat card, judul panjang, dan Tab). Yang saya perhatikan: pada Grid, card yang tersisa di baris terakhir tidak melebar penuh seperti pada Flexbox, tetapi tetap selebar satu kolom.

14. BATASAN
Saya hanya mengujinya di satu browser (Chrome). Angka lebar wadah dan card bergantung pada ukuran jendela dan lebar scrollbar, jadi bisa sedikit berbeda di perangkat lain. Pemeriksaan ini hanya pemeriksaan dasar, bukan jaminan semua aturan aksesibilitas terpenuhi.

15. AI USE STATEMENT
Alat: Claude (Anthropic). Tujuan: memahami isi modul dan menyusun README. Bagian yang dibantu: susunan README, bahasa penjelasan, dan hitungan ukuran card. Verifikasi: semua percobaan saya jalankan sendiri di browser dan DevTools, lalu saya cocokkan dengan tabel uji dan Nu HTML Checker. Saya tidak memasukkan data pribadi atau password ke AI dan saya bisa menjelaskan kode ini saat walkthrough.

16. REFLEKSI
Saya jadi paham bahwa Flexbox hanya mengatur anak langsung dari wadahnya, jadi display: flex harus dipasang pada ul atau div pembungkus, bukan pada tiap item. Saya juga belajar bahwa flex-basis hanyalah ukuran awal, dan ukuran final ditentukan oleh flex-grow, flex-shrink, dan ruang yang tersedia. Hal yang paling berguna bagi saya adalah overlay Flexbox di DevTools, karena saya bisa melihat langsung bagaimana ruang dibagi antar card.
