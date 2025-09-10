# NoSQL Manipulation

## Objective
Update multiple product reviews at the same time.

## Langkah Eksploitasi
1. Tulis sebuah review di salah satu produk. Setelah review tersimpan, klik edit review tersebut. Di sini browser akan mengirim request PATCH untuk memperbarui review sesuai id review.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 22 59 29" src="https://github.com/user-attachments/assets/39aa20a1-da1a-4e1b-b7ae-28ad7695d5c4" />
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 23 00 01" src="https://github.com/user-attachments/assets/9aa0d024-9caa-4d3e-bb14-77ef0129ab41" />
2. Manipulasi Field id dengan payload `{ "$ne": null }` → artinya id tidak sama dengan null, yang pasti true untuk semua dokumen karena semua review pasti punya id.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 23 01 52" src="https://github.com/user-attachments/assets/cf5c8ad7-110a-4b08-bf37-f05108a3c33f" />
3. Setelah review dikirim, semua review di database langsung berubah menjadi message yang sama. Hal ini menandakan query backend tidak memfilter hanya review tertentu, melainkan menerima operator $ne sebagai kondisi global.

## Catatan
- Hasil: Berhasil.
- Alasan: Payload {"id": {"$ne": null}} memanfaatkan kelemahan validasi input. Backend langsung memasukkan field id ke dalam query MongoDB tanpa filtering, sehingga operator $ne dijalankan sebagai query asli. Hasilnya, seluruh review terupdate sekaligus.
- Refleksi:
  - Awalnya request hanya mengubah 1 review. Setelah memodifikasi id dengan operator NoSQL, ternyata query backend tidak aman sehingga semua review ikut ter-update. Ini menunjukkan lemahnya sanitasi input pada aplikasi yang menggunakan database NoSQL.
  - Dari sisi keamanan, serangan ini sangat berbahaya karena bisa digunakan untuk mass update atau bahkan mass delete data penting. Mitigasi wajib: validasi field id agar hanya menerima string, gunakan ORM/ODM dengan parameterized queries, serta blacklist operator MongoDB pada input user.
