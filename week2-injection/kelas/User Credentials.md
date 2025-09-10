# User Credentials

## Objective
Retrieve a list of all user credentials via SQL Injection.

## Langkah Eksploitasi
1. Coba masukkan endpoint search sebagai bagian yang rentan akan sql injection
<img width="1440" height="900" alt="Screenshot 2025-09-10 at 12 05 51" src="https://github.com/user-attachments/assets/dacafcc7-8299-4d5f-9bc5-8a582d447148" />
2. Coba payload pertama `test' UNION SELECT * FROM USERS`. Hasilnya gagal karena jumlah kolom tidak cocok dengan tabel Products.
<img width="1440" height="900" alt="OWASP Juice Shop (Express" src="https://github.com/user-attachments/assets/8d21b1ee-2903-4c85-8040-584ad6e89d3d" />
3. Setelah percobaan, ditemukan struktur query yang benar dengan menyesuaikan jumlah kolom:
`test')) UNION SELECT username, password, role, deletedAt, isActive, createdAt, id, deluxeToken, profileImage FROM USERS--`

4. Namun, untuk mendapatkan challenge flag, kolom email juga harus ditambahkan:
`test')) UNION SELECT username, password, role, deletedAt, isActive, createdAt, id, email, profileImage FROM USERS--`
<img width="1440" height="900" alt="Screenshot 2025-09-10 at 11 53 28" src="https://github.com/user-attachments/assets/576f21f4-2b14-4ec9-a64a-9ad1bcaaf0e9" />

## Catatan
- Hasil: Berhasil.
- Alasan: Endpoint /rest/products/search?q= rentan SQLi. Payload UNION SELECT dengan jumlah kolom yang tepat memungkinkan kita menyuntikkan query ke tabel Users. Setelah menambahkan email ke kolom SELECT, kredensial user berhasil diekstrak.
- Refleksi:
  - Awalnya sempat error karena jumlah kolom UNION SELECT tidak sesuai. Setelah menyesuaikan jumlah kolom dengan struktur tabel Products, query berhasil dijalankan.
  - Kerentanan ini menunjukkan betapa berbahayanya SQL Injection, karena bukan hanya bisa bypass login, tapi juga dump data sensitif langsung dari database. Mitigasi harus dengan prepared statements dan validasi input.
