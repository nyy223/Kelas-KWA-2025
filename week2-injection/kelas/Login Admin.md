# Login Admin

## Objective
Log in with the administrator's user account.

## Langkap Eksploitasi
1. Masuk ke halaman login, terdapat field untuk mengisi bagian email dan password.
2. Pada field Email, masukkan payload
`' OR true--`
* Bagian OR true membuat kondisi selalu benar.
* -- adalah komentar SQL, yang mengabaikan sisa query termasuk pengecekan password.
3. Sedangkan untuk field password, isi bebas saja.
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 21 41 51" src="https://github.com/user-attachments/assets/5ce9613b-820b-4011-b238-0d2e91446adc" />
4. Karena kondisi `OR true` selalu benar, aplikasi mengembalikan hasil user pertama di tabel database, yaitu administrator account. Sehingga, percobaan login ke akun admin berhasil.
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 21 43 23" src="https://github.com/user-attachments/assets/ff861f0f-6a8b-4488-ae83-73bd1d2ca03c" />

