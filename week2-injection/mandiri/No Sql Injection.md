# No Sql Injection - PicoCTF
https://play.picoctf.org/practice/challenge/443?category=1&page=1&search=no%20sql

## Langkah Eksploitasi
1. Setelah mendownload source code dari soal, kita akan mendapati beberapa file seperti di gambar
<img width="426" height="68" alt="nayla@naylas-MacBook-Air app  ls" src="https://github.com/user-attachments/assets/3fe2a6e1-8bda-4b1b-8a83-46f97b930a0d" />

2. Di file server.js, terdapat bagian yang terlihat rentan
<img width="462" height="186" alt="password" src="https://github.com/user-attachments/assets/14026ead-7abc-4b88-9a01-2f6fa6527cbd" />

Endpoint menerima body JSON: { "email": "...", "password": "..." }. Ketika input berbentuk string JSON ("{"$ne": null}"), server akan JSON.parse dan menggunakan objek yang dihasilkan sebagai query MongoDB.
3. Ketika dicoba memasukkan payload berikut di login form, user berhasil masuk
```bash
{
  "email": "{\"$ne\": null}",
  "password": "{\"$ne\": null}"
}
```
Pada payload tsb, nilai email dan password dikirim sebagai string yang berisi JSON. Server melakukan JSON.parse → menghasilkan objek { "$ne": null }. Query di server jadi User.findOne({ email: { $ne: null }, password: { $ne: null } }) → cocok dengan hampir semua user.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 11 01 50" src="https://github.com/user-attachments/assets/fdc169b3-4227-48a1-9bac-108b26173e87" />
4. Coba capture menggunakan burpsuite, dan ganti email dan password dengan payload tadi. Maka server akan merespon JSON seperti yang menghasilkan token JWT yang merupakan string base-64, yang jika di decode akan menghasilkan flagnya.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 13 58 24" src="https://github.com/user-attachments/assets/661e27f7-06c8-43a5-828f-6e4282266904" />

## Catatan
- Hasil: Berhasil.
- Alasan: Payload NoSQL injection dengan {"$ne": null} berhasil memanipulasi query User.findOne() sehingga kondisi login selalu bernilai benar. Aplikasi mengembalikan data user pertama di database, yaitu admin, lengkap dengan token yang berisi flag.
- Refleksi:
Awalnya sempat bingung karena payload standar seperti email/password string biasa tidak berhasil. Setelah menyadari bahwa kode backend melakukan JSON.parse ketika input diawali dan diakhiri {...}, maka bisa dimanfaatkan untuk menyuntikkan operator MongoDB ($ne). Dari sisi keamanan, hal ini menunjukkan betapa berbahayanya parsing input tanpa validasi ketat. Celah seperti ini sering terjadi pada aplikasi yang memakai NoSQL database tanpa prepared query. Perbaikan yang seharusnya dilakukan adalah validasi tipe data, penggunaan schema strict di Mongoose, serta tidak pernah mengizinkan input mentah dikonversi langsung menjadi query object.
