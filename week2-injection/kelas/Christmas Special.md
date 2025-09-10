# Christmas Special

## Objective
Order the Christmas special offer of 2014.

## Langkah Eksploitasi
1. Sama seperti challenge sebelumnya, parameter q di endpoint search http://localhost:3000/rest/products/search?q=payload adalah titik lemah SQL Injection.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 21 49 30" src="https://github.com/user-attachments/assets/f2ff8397-5c95-41b4-b266-6bb93134ec3c" />
2. Produk spesial kemungkinan disembunyikan dengan kolom deletedAt yang tidak NULL (soft-delete), dengan memasukkan payload `')) UNION SELECT * FROM PRODUCTS WHERE deletedAt IS NOT NULL--`

3. Dari hasil SQL Injection, ditemukan informasi detail produk bernama Christmas Super-Surprise-Box (2014 Edition) dengan id 10. ID inilah yang nantinya dipakai untuk menambahkan produk ke keranjang.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 21 50 46" src="https://github.com/user-attachments/assets/0ff8273b-5a79-4896-bec6-147368078d32" />
4. Buka burpsuite, tambahkan produk ke keranjang dan capture payloadnya. Akan ada informasi mengenai productid. Di bagian sinilah yang akan kita manipulasi.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 21 59 14" src="https://github.com/user-attachments/assets/2e25cc33-ddd1-4426-88f2-175d4eef7e96" />
5. Ubah bagian product id dengan 10, yaitu id dari produk christmas
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 22 00 21" src="https://github.com/user-attachments/assets/69d4fed0-ab29-49f0-9177-26bc04db791d" />
6. Produk christmas offer akan ditambahkan ke keranjang
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 22 05 48" src="https://github.com/user-attachments/assets/4f3743a6-7b9c-40a9-863b-f58a20c7709e" />
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 22 06 13" src="https://github.com/user-attachments/assets/d2d3362a-f604-464a-944a-2aba11adc863" />

## Catatan
- Hasil: Berhasil.
- Alasan: Payload SQL Injection dengan kondisi deletedAt IS NOT NULL berhasil menampilkan produk yang sebelumnya disembunyikan oleh aplikasi. Dengan memanfaatkan ProductId hasil query, item tersebut bisa ditambahkan ke shopping cart melalui manipulasi request API.
- Refleksi:
  - Awalnya sempat gagal karena payload UNION SELECT tanpa filter tidak menampilkan produk spesial. Setelah memahami bahwa Juice Shop menggunakan mekanisme soft delete dengan kolom deletedAt, payload bisa diarahkan dengan tepat.
  - Langkah selanjutnya yang cukup tricky adalah menemukan ProductId dari hasil query dan menggunakannya di request POST /api/BasketItems. Dari sisi keamanan, kelemahan ini menunjukkan risiko besar dari penggunaan soft delete tanpa kontrol akses tambahan, serta pentingnya penggunaan prepared statements dan validasi input untuk mencegah SQL Injection.
