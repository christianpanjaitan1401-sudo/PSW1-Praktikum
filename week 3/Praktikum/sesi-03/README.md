1. Perbedaan inputmode dengan validasi
Kalau inputmode memgatur pengalaman pengguna saat mengetik sedangkan Validasi adalah proses memeriksa apakah ada kesalahan pada situs web.

2. MATRIKS UJI DAN EKSPERIMEN KEGAGALAN
| Kasus | Harapan |
| :--- | :--- |
| Email kosong | Ditolak karena required(harus diisi) |
| Email abc | Ditolak karena bentuk email(harus menggunakan format sesuai permintaan) |
| Email contoh valid | Diterima jika lainnya valid(tidak ada masalah) |
| Jumlah kosong | ditolak(saat kosong muncul teks yang menyatakan harus berisi) |
| Jumlah 0 / 6 | Ditolak(saat 0 muncul teks yang menyatakan harus lebih besar dari atau sama dengan 1, saat 6 muncul teks yang menyatakan harus <=5) |
| Jumlah 1 / 5 | Diterima(tidak terjadi masalah) |
| Jumlah 1.5 | Ditolak bila step=1(uncul teks yang menyatakan harus menggunakan nilai yang valid) |
| Kode 12345 | Ditolak(harus diawali 0) |
| Kode 001234 | Diterima(tidak terjadi masalah) |
| Keyboard | Semua kontrol dapat dicapai |