# Database Schema

## Objective
Exfiltrate the entire DB schema definition via SQL Injection.

## Langkah Eksploitasi
1. Saat melakukan inspect, endpoint berikut teridentifikasi rentan terhadap SQL injection
`/rest/products/search?q=`
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 22 27 05" src="https://github.com/user-attachments/assets/df026faa-c883-49f6-a3ee-c3a7e486c89f" />
2. Parameter q digunakan untuk query pencarian produk. Respon awal (tanpa payload) menunjukkan struktur data JSON yang berisi data-data seperti di gambar. Artinya, query SQL di backend memang menggunakan parameter q.
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 22 27 23" src="https://github.com/user-attachments/assets/a89db682-36c6-42d3-ad00-54f576a81f65" />
3. Payload pertama yang akan diuji adalah `/rest/products/search?q=test'--`. Hasil response:
   
```bash
{
  "status": "success",
  "data": []
}
```

Artinya, backend tidak error (menunjukkan query valid setelah `'--` menutup bagian SQL). Ini mengonfirmasi adanya SQL injection.

4. Schema database dapat diekstrak dari tabel internal sqlite_schema menggunakan payload   `')) UNION SELECT 1,2,3,4,5,6,7,8,sql FROM sqlite_schema--`. Payload sqlite_schema berfungsi untuk mengekstrak definisi struktur database (tabel, kolom, indeks, dan relasi) langsung dari sistem internal SQLite.
5. Hasil payload sqlite_schema menampilkan definisi struktur database (seperti perintah CREATE TABLE, nama tabel, kolom, dan tipe datanya) sehingga penyerang dapat mengetahui keseluruhan susunan tabel yang digunakan aplikasi.
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 22 29 12" src="https://github.com/user-attachments/assets/41e469e9-2d5b-4a82-a7fb-a03c15719def" />

## Catatan
- Hasil: Berhasil.
- Alasan: Payload SQL Injection dengan UNION SELECT ... sql FROM sqlite_schema berhasil menampilkan definisi tabel dari database SQLite, termasuk struktur tabel Users, Products, dan Orders. Dengan payload ini, seluruh schema dapat diekspos melalui response API produk.
- Refleksi:
   - Awalnya sempat gagal karena mencoba payload UNION SELECT tanpa menyesuaikan jumlah kolom, yang menyebabkan error “mismatch”. Setelah menambahkan dummy kolom 1–8, query menjadi valid dan field sql dari sqlite_schema berhasil ditampilkan.
   - Dari sisi keamanan, kelemahan ini menunjukkan bahaya besar SQL Injection: penyerang bisa memetakan struktur database lengkap. Langkah berikutnya untuk eksploitasi biasanya mencakup pencarian data sensitif (misalnya email, password hash) dengan memanfaatkan schema yang sudah terekspos. Mitigasi yang tepat adalah penggunaan prepared statements dan sanitasi input.
