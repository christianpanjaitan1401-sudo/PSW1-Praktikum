/*
NAMA   : Christian Junior Panjaitan
NIM    : 41426027
PRODI  : D4 Teknologi Rekayasa Perangkat Lunak
*/

1. TUJUAN
Saya melanjutkan katalog kegiatan mahasiswa dari Sesi 2, lalu mengubah wadah card dari Flexbox menjadi CSS Grid. Data yang dipakai fiktif. Hasil yang saya targetkan di Sesi 3:
- mengubah wadah card menjadi Grid dan memahami bahwa anak langsungnya menjadi grid item,
- membuat grid adaptif dengan repeat(auto-fit, minmax(...)) yang jumlah kolomnya menyesuaikan ruang,
- memakai Flexbox hanya untuk isi card dan memakai satu class card untuk semua item,
- menguji tata letak dengan nol, satu, dan lima card serta ukuran layar yang berbeda,
- menjelaskan pilihan Flexbox atau Grid beserta alasannya.

2. CARA MENJALANKAN
1. Buka folder pswi-week-05/sesi-03 di VS Code.
2. Klik kanan index.html lalu pilih Open with Live Server.
3. Halaman terbuka lewat alamat server lokal (contoh: http://127.0.0.1:5500/). Tekan F12 untuk membuka DevTools, lalu Ctrl+Shift+M untuk mode responsif.

Susunan folder:

```
pswi-week-05/
  sesi-03/
    index.html
    style.css
    percobaan-0-card.html
    percobaan-1-card.html
    percobaan-5-card.html
    README.md
    bukti/
```

File percobaan-*.html adalah versi percobaan yang diminta modul (bagian 7). File main.js tidak saya buat karena tidak dipakai.

3. PERSIAPAN
Sebelum saya mulai, saya membuat folder baru pswi-week-05/sesi-03, lalu menyalin hasil Sesi 2 (index.html dan style.css) ke dalamnya tanpa mengubah folder sesi-02. Setelah itu saya membuat README.md dan folder bukti, menjalankan server lokal seperti di Minggu 1, lalu memeriksa Git dengan git --version dan git status. Hasilnya muncul versi Git (contoh git version 2.43.0) dan git status berjalan tanpa error, jadi repository sudah ada dan saya tidak menjalankan git init lagi.
Bukti: bukti/01-git-version-status.png

4. MENGUBAH CONTAINER
Pada HTML saya mengganti class cards menjadi catalog. Pada style.css saya menghapus aturan flex pada wadah (display: flex dan flex-wrap) dan menggantinya dengan:

```
.catalog { display: grid; gap: 1rem;
           grid-template-columns: 1fr 1fr; }
```

Aturan flex: 1 1 15rem pada .card juga saya hapus, karena aturan itu hanya berlaku pada item di dalam wadah flex dan tidak ada gunanya pada grid item.
Hasil pengamatan:
- Dua kolom: card tersusun dalam dua kolom yang sama lebar. Di jendela 1200px setiap kolom sekitar 584px, sehingga empat card menjadi dua baris berisi dua card.
- Overlay Grid (klik badge grid di samping .catalog pada tab Elements, lalu centang overlay di tab Layout): terlihat dua track kolom beserta area gap di antaranya.
- Anak langsung menjadi grid item: empat article.card adalah grid item dan masing-masing menempati satu sel. Elemen di dalam card (h2 dan p) bukan grid item, jadi tidak diatur oleh grid wadah.
Dua kolom tetap ini belum adaptif. Prediksi saya, di jendela 320px kolom menjadi sempit (sekitar 144px per kolom). Hasil nyatanya memang begitu: card terasa sesak karena kolomnya tidak bisa berubah menjadi satu kolom. Hal ini diperbaiki di bagian 5.
Bukti: bukti/02-grid-dua-kolom-overlay.png, bukti/03-grid-item-anak-langsung.png, bukti/04-dua-kolom-tetap-320.png

5. GRID ADAPTIF
Saya mengganti definisi kolom dengan pola minmax:

```
.catalog { grid-template-columns:
  repeat(auto-fit, minmax(min(100%, 15rem), 1fr)); }
```

Artinya: setiap kolom minimal selebar 15rem (240px) atau 100% wadah kalau wadahnya lebih sempit dari itu (min(100%, 15rem) mencegah overflow di layar yang sangat sempit), dan maksimal 1fr (membagi sisa ruang). auto-fit membuat browser memuat sebanyak mungkin kolom yang cukup ruang.
Saya mengujinya pada 320px, 768px, dan 1200px dengan empat card, lalu menambahkan item kelima sementara (card "Kuis Aksesibilitas", isi "Menguji pemahaman.") untuk melihat baris barunya. Item kelima saya hapus lagi setelah diamati (versi lima card disimpan di bagian 7).

| Lebar | Lebar wadah | Jumlah kolom | Lebar kolom | 4 card | 5 card |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 320px | sekitar 304px | 1 | sekitar 304px | 4 baris | 5 baris |
| 768px | sekitar 752px | 3 | sekitar 240px | 3 + 1 | 3 + 2 |
| 1200px | sekitar 1184px | 4 | sekitar 284px | 4 | 4 + 1 |

Penjelasan hitungan: wadah adalah lebar tampilan dikurangi margin bawaan body (8px kiri dan kanan). Jumlah kolom = lebar wadah ditambah gap, dibagi (240 + gap), dibulatkan ke bawah. Contoh di 1200px: (1184 + 16) / 256 = 4,69 menjadi 4 kolom, dan lebar kolom (1184 - 48) / 4 = 284px. Pada item kelima, card baru turun ke baris baru dan ditempatkan otomatis di sel kosong berikutnya (auto-placement). Card itu hanya selebar satu kolom, tidak melebar penuh.
Catatan: 768px tepat di batas tiga kolom (3 x 240 + 2 x 16 = 752px). Kalau lebar wadah sedikit lebih kecil, misalnya karena ada scrollbar, jumlahnya menjadi 2 kolom. Jadi jumlah kolom memang menyesuaikan ruang yang tersedia.
Bukti: bukti/05-auto-fit-320.png, bukti/06-auto-fit-768.png, bukti/07-auto-fit-1200.png, bukti/08-lima-card-1200.png, bukti/09-lima-card-768.png

6. CARD INTERNAL DAN GAP
Saya menambahkan link detail pada setiap card, dengan teks yang berbeda agar jelas tujuannya (misalnya "Detail Lokakarya HTML"), lalu menulis aturan Flexbox khusus untuk isi card:

```
<a href="#">Detail Lokakarya HTML</a>

.card { display: flex; flex-direction: column; gap: .5rem; }
.card h2, .card p { margin: 0; }
.card a { align-self: start; }
```

Hasil pengamatan:
- Margin dan gap tidak berlipat. Saat aturan margin: 0 saya matikan sementara, jarak judul ke paragraf menjadi sekitar 44px (margin bawah h2 sekitar 20px + gap 8px + margin atas p 16px), karena margin tidak menyatu di dalam wadah flex. Setelah margin: 0 dihidupkan lagi, jaraknya tinggal 8px, yaitu hanya dari gap.
- Link tidak meregang. Saat align-self: start saya matikan sementara, link melebar selebar isi card sehingga area klik dan garis fokusnya terlalu lebar (bawaan align-items: stretch). Dengan align-self: start, link hanya selebar teksnya.
- Satu class: semua article memakai class="card" saja, tanpa class tambahan per item. Satu aturan .card menata semua card dengan sama.
Bukti: bukti/10-card-margin-berlipat.png, bukti/11-card-flex-gap-overlay.png, bukti/12-link-meregang.png, bukti/13-link-align-self-start.png

7. REVIEW LAYOUT
Aturan padding card yang saya pakai:

```
/* urutan konten tetap ditentukan HTML */
.card { padding: var(--space-2, 1rem); }
```

Karena --space-2 belum saya definisikan, browser memakai nilai cadangan 1rem. Di tab Computed padding card terbaca 16px dan tidak ada error.

Uji nol, satu, dan lima card. Saya menyimpan tiga versi percobaan sebagai file terpisah (percobaan-0-card.html, percobaan-1-card.html, percobaan-5-card.html), lalu mengembalikan index.html menjadi katalog final.
- Nol card: wadah .catalog kosong dan tingginya 0. Judul h1 dan footer tetap tampil, dan tidak ada error di Console. Halaman terlihat kosong karena tidak ada pesan pengganti.
- Satu card: card melebar penuh (sekitar 1184px di 1200px) karena auto-fit menciutkan kolom kosong. Teks tetap terbaca.
- Lima card: baris baru terbentuk sesuai tabel di bagian 5, dan urutan card sesuai urutan HTML.

Urutan Tab: saya tidak memakai order, jadi urutan Tab sama dengan alur DOM dan sama dengan urutan tampilan. Fokus berjalan dari Kegiatan, Kontak, lalu link detail pada card dari kiri ke kanan dan atas ke bawah. Garis fokus bawaan browser terlihat jelas.
Bukti: bukti/14-percobaan-nol-card.png, bukti/15-percobaan-satu-card.png, bukti/17-tab-urutan-dom.png

Review bersama teman (pilihan Flexbox atau Grid dan alasannya):

| Bagian | Pilihan | Alasan |
| :--- | :--- | :--- |
| Navigasi | Flexbox | Hanya satu dimensi (satu baris), item mengalir dan bisa wrap |
| Wadah card | Grid | Dua dimensi: kolom sejajar rapi dan jumlah kolom menyesuaikan ruang |
| Isi card | Flexbox (column) | Susunan vertikal satu dimensi dengan jarak lewat gap |

Teman saya setuju dengan alasan itu dan memeriksa satu alasan teknis, yaitu kenapa card yang tersisa di baris terakhir pada Grid tidak melebar penuh seperti pada Flexbox. Jawabannya, pada Grid card mengikuti lebar kolom yang tetap sama untuk semua baris, sedangkan pada Flexbox tiap baris membagi ruangnya sendiri lewat flex-grow.

Validasi: Nu HTML Checker menghasilkan "Document checking completed. No errors or warnings to show." dan W3C CSS Validator tidak menampilkan error.
Bukti: bukti/18-nu-html-checker.png dan bukti/19-css-validator.png

8. LATIHAN MANDIRI DAN TABEL UJI
Saya mengulang percobaan di atas sendiri dan menulis prediksi sebelum menjalankannya. Untuk data tambahan, saya mengganti judul satu card dengan satu kata panjang tanpa spasi (sekitar 50 huruf, misalnya "LokakaryaPengembanganSitusWebDasarUntukMahasiswaBaru"), lalu mengembalikannya setelah uji.

| Kasus | Tindakan | Harapan | Hasil aktual | Status/bukti |
| :--- | :--- | :--- | :--- | :--- |
| Normal: dua kolom tetap | Lihat 1fr 1fr di 1200px | Dua track kolom terlihat | Dua kolom sekitar 584px, overlay Grid menampilkan dua track | Lulus, bukti 02 dan 03 |
| Normal: 1200px | Lihat auto-fit dengan 4 card | Jumlah kolom menyesuaikan ruang | 4 kolom, tiap kolom sekitar 284px | Lulus, bukti 07 |
| Normal: 768px | Lihat auto-fit dengan 4 card | Jumlah kolom menyesuaikan ruang | 3 kolom (tepat di batas), card keempat di baris kedua | Lulus, bukti 06 |
| Normal: 320px | Lihat auto-fit dengan 4 card | Tidak overflow | 1 kolom selebar sekitar 304px, tidak ada scroll ke samping | Lulus, bukti 05 |
| Normal: lima card | Tambah item kelima, uji 1200px dan 768px | Auto-placement benar | 4 + 1 di 1200px dan 3 + 2 di 768px, urutan sesuai HTML | Lulus, bukti 08 dan 09 |
| Normal: Tab | Tekan Tab dari awal halaman | Sama dengan alur DOM | Kegiatan, Kontak, lalu link detail card berurutan, fokus terlihat | Lulus, bukti 17 |
| Batas: satu card | Buka percobaan-1-card.html | Card tetap terbaca | Card melebar penuh (sekitar 1184px), teks tetap terbaca | Lulus, bukti 15 |
| Batas: nol card | Buka percobaan-0-card.html | Halaman tidak rusak | Wadah kosong tinggi 0, h1 dan footer tetap tampil, tidak ada error | Lulus, bukti 14 |
| Batas: judul panjang | Ganti judul dengan kata 50 huruf, lebar 320px | Teks tidak keluar kotak | Judul terpecah di dalam card oleh overflow-wrap: anywhere, kolom tidak melebar, tidak ada scroll ke samping | Lulus, bukti 16 |
| Gagal: dua kolom tetap di 320px | Lihat 1fr 1fr di 320px | Prediksi: kolom sempit dan sesak | Benar, kolom sekitar 144px. Setelah diganti auto-fit, menjadi 1 kolom sekitar 304px | Awalnya kurang baik, setelah perbaikan lulus, bukti 04 dan 05 |
| Gagal: margin berlipat | Matikan margin: 0 sementara | Prediksi: jarak lebih besar dari gap | Benar, jarak judul ke paragraf sekitar 44px. Setelah margin: 0 dihidupkan, jaraknya 8px | Awalnya gagal, setelah perbaikan lulus, bukti 10 dan 11 |
| Gagal: link meregang | Matikan align-self: start sementara | Prediksi: link melebar | Benar, link selebar isi card. Setelah align-self: start dihidupkan, selebar teksnya | Awalnya gagal, setelah perbaikan lulus, bukti 12 dan 13 |

Lulus berarti hasil nyata sama dengan harapan atau prediksi. Setelah semua percobaan, index.html dikembalikan menjadi katalog final berisi empat card dengan link detail.
Bukti tambahan: bukti/16-judul-panjang-320.png

9. PERBANDINGAN DENGAN CONTOH LAMPIRAN
Setelah selesai berlatih, saya membandingkan hasil saya dengan Lampiran A1 dan A2. Perbedaannya:
- Lampiran membatasi lebar nav, main, dan footer dengan width: min(92%, 68rem) dan margin-inline: auto, sedangkan hasil saya belum membatasi lebar.
- Lampiran memakai body dengan font Arial dan line-height 1.6, serta aturan a:focus-visible dengan outline 3px. Hasil saya memakai garis fokus bawaan browser.
- Lampiran memasang overflow-wrap: anywhere pada h2 dan p di dalam card, sedangkan hasil saya baru memasangnya pada h2 (dari Sesi 2).
- Lampiran memakai padding: 1rem langsung, sedangkan hasil saya memakai var(--space-2, 1rem) seperti di bagian 7.
- Lampiran berisi tiga card tanpa link detail, sedangkan hasil saya berisi empat card dengan link detail.
Satu keputusan yang saya pahami: wadah card memakai Grid dengan auto-fit karena butuh kolom yang sejajar dan jumlahnya menyesuaikan ruang, sedangkan isi card memakai Flexbox karena hanya satu dimensi. Satu risiko yang ditangani: kolom minimal 15rem bisa menyebabkan overflow di layar yang sangat sempit, dan itu dicegah dengan min(100%, 15rem).

| Hal yang diubah | Sebelum (Sesi 2) | Sesudah (Sesi 3) |
| :--- | :--- | :--- |
| Wadah card | Flexbox dengan wrap | Grid dengan auto-fit dan minmax |
| Card tersisa di baris terakhir | Ikut melebar karena flex-grow | Hanya selebar satu kolom |
| Isi card | Margin bawaan h2 dan p | Flexbox column dengan gap, margin 0 |
| Link detail | Belum ada | Ada di setiap card dan tidak meregang |

10. DAFTAR BUKTI
Semua berkas ada di folder bukti/:

| Berkas | Isi yang harus terlihat |
| :--- | :--- |
| 01-git-version-status.png | Hasil git --version dan git status |
| 02-grid-dua-kolom-overlay.png | Overlay Grid dengan dua track kolom |
| 03-grid-item-anak-langsung.png | Article.card sebagai grid item |
| 04-dua-kolom-tetap-320.png | Dua kolom tetap yang sempit di 320px |
| 05-auto-fit-320.png | Auto-fit satu kolom di 320px |
| 06-auto-fit-768.png | Auto-fit tiga kolom di 768px |
| 07-auto-fit-1200.png | Auto-fit empat kolom di 1200px |
| 08-lima-card-1200.png | Lima card (4 + 1) di 1200px |
| 09-lima-card-768.png | Lima card (3 + 2) di 768px |
| 10-card-margin-berlipat.png | Jarak berlipat saat margin 0 dimatikan |
| 11-card-flex-gap-overlay.png | Overlay Flexbox pada card dengan gap 8px |
| 12-link-meregang.png | Link melebar penuh tanpa align-self |
| 13-link-align-self-start.png | Link selebar teksnya |
| 14-percobaan-nol-card.png | Halaman dengan nol card |
| 15-percobaan-satu-card.png | Halaman dengan satu card |
| 16-judul-panjang-320.png | Judul panjang terpecah di dalam card |
| 17-tab-urutan-dom.png | Garis fokus pada link, urutan sesuai DOM |
| 18-nu-html-checker.png | Hasil Nu HTML Checker tanpa error |
| 19-css-validator.png | Hasil W3C CSS Validator tanpa error |
| 20-git-commit.png | Hasil git status, git diff, dan git commit |

11. GIT DAN SETORAN
Sebelum git add, saya cek perubahan dengan git status dan git diff, dan memastikan tidak ada file rahasia, data pribadi, atau file sementara yang besar. Lalu saya menjalankan:

```
git status
git diff
git add .
git commit -m "PSWI Week 5 Sesi 3: latihan dan pengujian"
```

Setelah commit muncul baris seperti [main a1b2c3d] PSWI Week 5 Sesi 3: latihan dan pengujian beserta jumlah file yang berubah. Bukti: bukti/20-git-commit.png
Berkas setoran adalah pswi_4141103_w05s03_41426027.zip yang berisi folder sesi-03 (index.html, style.css, tiga file percobaan, README.md, dan folder bukti). Saya tidak memakai gambar, font, atau aset luar, jadi tidak ada sumber aset yang perlu dicatat.

12. PERSIAPAN MINGGU 6
Untuk Responsive Web Design, saya menyiapkan daftar berikut. Gambarnya belum saya pasang di halaman.

Ukuran layar yang akan diuji:

| Lebar | Perangkat | Keterangan |
| :--- | :--- | :--- |
| 320px | HP kecil | Batas paling sempit yang diuji |
| 360px | HP umum | Ukuran yang dipakai di Sesi 2 |
| 768px | Tablet | Tepat di batas tiga kolom |
| 1024px | Laptop kecil | Untuk memeriksa perubahan dari tablet ke laptop |
| 1200px | Desktop | Ukuran yang dipakai di Sesi 2 dan 3 |

Konten gambar: satu gambar per card (empat gambar), dibuat sendiri atau memakai gambar yang boleh dipakai, dengan sumber dan izin dicatat. Ukuran yang saya rencanakan 800 x 450px (rasio 16:9) dengan format JPG atau WebP dan ukuran file di bawah 150KB. Setiap gambar informatif diberi alt yang menjelaskan isinya, misalnya "Peserta lokakarya menulis kode HTML di laptop".

13. BATASAN
Saya hanya mengujinya di satu browser (Chrome). Angka lebar wadah dan kolom bergantung pada ukuran jendela dan lebar scrollbar, jadi bisa sedikit berbeda di perangkat lain, dan lebar 768px tepat di batas tiga kolom. Link detail masih memakai href="#" karena halaman detailnya belum dibuat. Pemeriksaan ini hanya pemeriksaan dasar, bukan jaminan semua aturan aksesibilitas terpenuhi.

14. AI USE STATEMENT
Alat: Claude (Anthropic). Tujuan: memahami isi modul dan menyusun README. Bagian yang dibantu: susunan README, bahasa penjelasan, dan hitungan jumlah kolom serta jarak. Verifikasi: semua percobaan saya jalankan sendiri di browser dan DevTools, lalu saya cocokkan dengan tabel uji, Nu HTML Checker, dan CSS Validator. Saya tidak memasukkan data pribadi atau password ke AI dan saya bisa menjelaskan kode ini saat walkthrough.

15. REFLEKSI
Saya jadi paham bahwa Grid cocok untuk mengatur wadah dalam dua dimensi, sedangkan Flexbox cocok untuk satu dimensi seperti navigasi dan isi card. Saya juga belajar bahwa pola repeat(auto-fit, minmax(...)) membuat jumlah kolom menyesuaikan ruang tanpa media query. Hal yang paling berguna bagi saya adalah mencoba nol, satu, dan lima card, karena saya melihat bahwa auto-fit membuat satu card melebar penuh dan itu perlu saya sadari sebagai risiko.
