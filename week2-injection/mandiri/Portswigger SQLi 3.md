# Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

## Langkah Eksploitasi
1. Saat pertama membuka web, aplikasi menampilkan daftar produk berdasarkan kategori. Setiap kali kita memilih kategori, aplikasi mengirimkan request ke server dengan parameter category.
2. Untuk menguji adanya SQL injection, sisipkan payload sederhana OR 1=1-- ke dalam parameter category
<img width="1440" height="900" alt="Screenshot 2025-09-10 at 01 02 23" src="https://github.com/user-attachments/assets/8ce5b085-2794-42fd-a084-a4500b8465bd" />

3. Setelah payload dijalankan, semua produk ditampilkan, termasuk yang tersembunyi. Lab otomatis menandai challenge sebagai Solved.
<img width="1440" height="900" alt="Web Security" src="https://github.com/user-attachments/assets/4de988e0-8421-443f-bb4d-d934c3d7e15a" />

## Catatan
- Hasil: Berhasil.
- Alasan: Payload sederhana OR 1=1-- memodifikasi kondisi WHERE sehingga query mengembalikan semua baris tanpa filter kategori.
- Refleksi: Eksploitasi ini menunjukkan kelemahan umum pada aplikasi yang menyisipkan input user langsung ke dalam query SQL tanpa validasi. Meski payload ini tergolong dasar, ia cukup untuk mengakses data tersembunyi. Dari sisi keamanan, penting menggunakan parameterized queries (prepared statements) agar input tidak memengaruhi struktur query.
