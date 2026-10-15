/*
NAMA   : Christian Junior Panjaitan
NIM    : 41426027
PRODI  : D4 Teknologi Rekayasa Perangkat Lunak
*/

1. TUJUAN
Saya melanjutkan katalog Grid dari Minggu 5, lalu mengubahnya menjadi mobile-first dengan media query. Data yang dipakai fiktif. Hasil yang saya targetkan di Sesi 2:
- menerapkan CSS mobile-first (satu kolom sebagai dasar, dua kolom ditambahkan lewat media query) dengan breakpoint berdasarkan konten,
- membuat gambar dan kontrol form yang fleksibel dan tidak keluar dari wadah,
- menguji tata letak pada viewport kecil, sedang, besar, dan zoom 200%,
- mengenali container query dan preferensi reduced motion (bagian 12).

2. CARA MENALANKAN
1. Buka folder pswi-week-06/sesi-02 di VS Code.
2. Klik kanan index.html lalu pilih Open with Live Server.
3. Halaman terbuka lewat alamat server lokal (contoh: http://127.0.0.1:5500/). Tekan F12 untuk membuka DevTools, lalu Ctrl+Shift+M untuk mode responsif.

Susunan folder:

```
pswi-week-06/
  sesi-02/
    index.html
    style.css
    main.js
    README.md
    bukti/
```

File main.js berisi skrip tombol salam dari Minggu 2 dan tidak saya ubah di sesi ini.

3. PERSIAPAN
Sebelum saya mulai, saya membuat folder baru pswi-week-06/sesi-02, lalu menyalin katalog Grid dari Minggu 5 (index.html, style.css, dan main.js) ke dalamnya tanpa mengubah folder minggu sebelumnya. Setelah itu saya membuat README.md dan folder bukti, menjalankan server lokal seperti di Minggu 1, lalu memeriksa Git dengan git --version dan git status. Hasilnya muncul versi Git dan git status berjalan tanpa error, jadi repository sudah ada dan saya tidak menjalankan git init lagi.

4. STARTER MOBILE
Saya memastikan meta viewport di head sudah benar:

```
<meta name="viewport" content="width=device-width, initial-scale=1">
```

Meta ini membuat lebar halaman mengikuti lebar layar perangkat. Tanpa meta ini, HP menampilkan halaman seolah-olah layarnya selebar sekitar 980px lalu memperkecilnya, sehingga media query tidak bekerja sesuai lebar layar sebenarnya.
Lalu saya membuka mode responsif dan mengatur lebar 320px, dengan satu kolom sebagai dasar:

```
.catalog { display: grid; gap: 1rem; grid-template-columns: 1fr; }
```

Aturan auto-fit dari Minggu 5 saya ganti dengan aturan ini, karena sekarang jumlah kolom diatur oleh media query.
Hasil pengamatan di 320px:
- Semua konten tersedia: judul, profil singkat dan gambar, tombol, navigasi (Kegiatan dan Kontak), semua card beserta link detail-nya, dan footer. Card tersusun satu kolom dari atas ke bawah.
- Ada satu masalah: saat saya jalankan `document.documentElement.scrollWidth > document.documentElement.clientWidth` di Console, hasilnya true, artinya ada scroll ke samping. Penyebabnya gambar profil memakai width="400" sehingga lebih lebar dari wadah (sekitar 254px). Perbaikannya, saya menambahkan:

```
img { max-width: 100%; height: auto; }
```

Retest: gambar mengecil mengikuti wadah dengan perbandingan tinggi-lebar tetap, dan perintah di Console, hasilnya false, yang artinya scroll kesamping mati.

5. BREAKPOINT KONTEN
Saya menambahkan layout dua kolom ketika ruang cukup:

```
.catalog { display: grid; gap: 1rem; grid-template-columns: 1fr; }
@media (min-width: 48rem) {
  .catalog { grid-template-columns: repeat(2, 1fr); }
}
```

Mobile-first artinya aturan dasar ditulis untuk layar kecil, lalu aturan untuk layar yang lebih besar ditambahkan dengan min-width. Breakpoint 48rem dipilih berdasarkan konten, bukan berdasarkan perangkat tertentu: mulai dari lebar itu dua card bisa berdampingan dengan nyaman (tiap card sekitar 343px, jauh di atas 240px yang dibutuhkan agar teksnya tidak sesak).

Lalu saya menguji tiga lebar di mode responsif:

| Breakpoint | Nilai viewport aktual | Kondisi min-width: 48rem | Hasil |
| :--- | :--- | :--- | :--- |
| 48rem (= 768px) | 767px | tidak terpenuhi | 1 kolom |
| 48rem (= 768px) | 768px | terpenuhi (batas termasuk) | 2 kolom |
| 48rem (= 768px) | 769px | terpenuhi | 2 kolom |

Layout berubah tepat ketika kondisi media query terpenuhi. Rem di dalam media query mengikuti ukuran font bawaan browser, bukan font-size yang saya tulis di CSS, jadi kalau pengguna mengubah ukuran font di pengaturan browser, breakpoint ikut bergeser (misalnya jika font bawaan 20px, 48rem menjadi 960px). Karena itu saya tidak menganggap rem selalu 16px, dan nilai 768px di atas berlaku untuk konfigurasi saya.

6. NAVIGASI DAN FORM
Saya mengubah jarak navigasi dan membuat kontrol form tidak melebihi wadah:

```
.nav-list { display: flex; flex-wrap: wrap; gap: .75rem; }
input, select, textarea { max-width: 100%; }
```

Karena katalog Minggu 5 belum punya form, saya menempelkan form pendaftaran dari Minggu 3 (nama, email, program studi, catatan, dan tombol Daftar) di bagian bawah main. Label nama sengaja saya buat panjang ("Nama lengkap sesuai kartu identitas mahasiswa yang masih berlaku (wajib)") untuk pengujian. Atribut action saya hilangkan supaya HTML valid.
Hasil pengamatan di 320px:
- Label panjang turun ke beberapa baris, tidak terpotong, dan tidak melebihi lebar wadah.
- Input, select, textarea, dan tombol Daftar semuanya berada di dalam wadah main. max-width: 100% menjaga kontrol tidak lebih lebar dari wadahnya walaupun lebar bawaannya lebih besar.
- Navigasi tetap berjajar dan aturan flex-wrap membuat tautan turun ke baris baru kalau ruang tidak cukup (dengan dua tautan, di 320px keduanya masih muat satu baris).
- Tab: fokus berpindah melalui tautan navigasi, link detail card, semua kontrol form, sampai tombol Daftar, urutannya sama dengan urutan di layar. Garis fokus bawaan browser terlihat jelas pada setiap elemen. Semua interaksi tetap tersedia.

7. MATRIKS VIEWPORT
Aturan min-width: 0 pada card sudah ada dari Minggu 5:

```
/* jika item tidak boleh mempertahankan lebar minimum besar */
.card { min-width: 0; }
```

Hasil di tiap ukuran saya simpan sebagai tabel, bukan hanya gambar. Lebar isi main adalah lebar tampilan dikurangi padding body dan main (sekitar 66px), dan maksimal sekitar 734px karena body dibatasi max-width: 50rem.

| Lebar tampilan | Query 48rem | Kolom | Lebar isi main | Scroll horizontal |
| :--- | :--- | :--- | :--- | :--- |
| 320px | tidak aktif | 1 | sekitar 254px | tidak ada |
| 768px | aktif | 2 | sekitar 702px | tidak ada |
| 1200px | aktif | 2 | sekitar 734px | tidak ada |
| Zoom 200% (jendela 1200px, tampilan 600px) | tidak aktif | 1 | sekitar 534px | tidak ada |

"Tidak ada" berarti perintah scrollWidth > clientWidth di Console menghasilkan false. Di 1200px layout tetap dua kolom dan berada di tengah, karena body dibatasi 50rem dan breakpoint yang saya buat hanya sampai dua kolom. Pada zoom 200%, lebar tampilan menjadi 600px sehingga query 48rem tidak aktif dan layout kembali ke satu kolom. Teks menjadi dua kali lebih besar tetapi tidak ada yang terpotong.

Overflow yang disengaja: saya memberi card pertama width: 900px. Prediksi saya, di 320px halaman akan bisa digeser ke samping karena card lebih lebar dari layar.
- Sebelum: benar, card keluar dari wadah dan muncul scroll horizontal. Di Console scrollWidth lebih dari 900 sedangkan clientWidth 320, jadi hasilnya true. min-width: 0 tidak menolong karena lebar 900px ditulis eksplisit.
- Perbaikan: saya menambahkan max-width: 320px pada card yang mengunci ukuran card pada 320px.
- Sesudah: card kembali selebar kolom, scrollWidth sama dengan clientWidth (320), hasilnya false.

8. LATIHAN MANDIRI DAN TABEL UJI
Saya mengulang percobaan di atas sendiri dan menulis prediksi sebelum menjalankannya. Untuk data tambahan, saya mengganti judul satu card dengan satu kata panjang tanpa spasi (sekitar 50 huruf, misalnya "LokakaryaPengembanganSitusWebDasarUntukMahasiswaBaru"), lalu mengembalikannya setelah uji.

| Kasus | Tindakan | Harapan | Hasil aktual | Status |
| :--- | :--- | :--- | :--- | :--- |
| Normal: 320px | Atur lebar 320px, periksa semua konten | Konten lengkap | Satu kolom, semua konten dan link tersedia, tanpa scroll ke samping setelah gambar diperbaiki | Lulus |
| Normal: 1200px | Atur lebar 1200px | Layout menyesuaikan | Dua kolom di tengah halaman, tanpa scroll ke samping | Lulus |
| Normal: keyboard | Tab di lebar 320px | Fokus terlihat | Semua tautan, kontrol form, dan tombol bisa dicapai berurutan, garis fokus terlihat | Lulus |
| Batas: 767px | Atur lebar 767px | Query belum aktif | 1 kolom | Lulus |
| Batas: 768px | Atur lebar 768px | Query aktif tepat di batas | 2 kolom | Lulus |
| Batas: 769px | Atur lebar 769px | Query aktif | 2 kolom | Lulus |
| Batas: zoom 200% | Zoom browser ke 200% di jendela 1200px | Tidak terpotong | Satu kolom, teks 2x lebih besar, tidak ada yang terpotong | Lulus |
| Batas: label panjang | Isi label nama dengan teks panjang, lebar 320px | Interaksi tetap tersedia | Label turun baris, kontrol tetap di dalam wadah | Lulus |
| Batas: kata panjang | Ganti judul card dengan kata 50 huruf, lebar 320px | Prediksi: tetap di dalam card | Benar, judul terpecah di dalam card oleh overflow-wrap: anywhere, tanpa scroll ke samping | Lulus |
| Gagal: gambar width 400 | Cek scroll di 320px | Prediksi: gambar melebihi wadah | Benar, scrollWidth lebih besar dari clientWidth. Setelah img max-width: 100% ditambahkan, tidak ada scroll | Awalnya gagal, setelah perbaikan lulus |
| Gagal: tanpa meta viewport | Hapus meta viewport sementara, lebar 320px | Prediksi: tampilan seperti desktop yang diperkecil | Benar, halaman dilayout selebar sekitar 980px lalu diperkecil, teks sangat kecil, dan media query memakai lebar 980px sehingga dua kolom aktif. Setelah meta dikembalikan, normal lagi | Awalnya gagal, setelah perbaikan lulus |
| Gagal: overflow 900px | Beri card width: 900px, lebar 320px | Prediksi: scroll ke samping | Benar, lalu diperbaiki dengan menghapus width: 900px | Awalnya gagal, setelah perbaikan lulus |

Lulus berarti hasil nyata sama dengan harapan atau prediksi. Setelah semua percobaan, judul card dikembalikan ke teks asli dan width 900px sudah dihapus.
Saya juga meminta teman memeriksa satu alasan teknis, yaitu kenapa min-width: 0 tidak mencegah overflow ketika width: 900px ditulis. Teman saya setuju bahwa min-width: 0 hanya melonggarkan lebar minimum otomatis, sedangkan lebar yang ditulis eksplisit tetap dipakai.

9. AI USE STATEMENT
Alat: Claude (Anthropic). Tujuan: memahami isi modul dan menyusun README. Bagian yang dibantu: susunan README, bahasa penjelasan, dan hitungan lebar tampilan serta breakpoint. Verifikasi: semua percobaan saya jalankan sendiri di browser dan DevTools, lalu saya cocokkan dengan tabel uji, Nu HTML Checker, dan CSS Validator. Saya tidak memasukkan data pribadi atau password ke AI dan saya bisa menjelaskan kode ini saat walkthrough.

