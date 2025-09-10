# Lab: SQL injection UNION attack, retrieving multiple values in a single column
https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column

## Langkah Eksploitasi
1. Sama seperti challenge sebelumnya, gunakan ORDER BY untuk mengetahui jumlah kolom pada query. Hasilnya adalah jumlah kolomnya sebanyak 2.
<img width="1440" height="900" alt="Screenshot 2025-09-10 at 00 32 45" src="https://github.com/user-attachments/assets/a0e8714c-6127-4346-b9ab-14434e7f8bf5" />

2. Menentukan tipe data kolom menggunakan payload UNION SELECT dengan kombinasi NULL dan string 'a'. Ditemukan bahwa kolom kedua dapat menampung string.
<img width="1440" height="900" alt="Screenshot 2025-09-10 at 00 35 14" src="https://github.com/user-attachments/assets/583bfdc7-613f-4d67-83d5-41eec7df6d28" />

3. Setelah tahu kolom yang bisa menerima string, uji query untuk menampilkan username menggunakan payload `' UNION SELECT NULL, username FROM users--`
<img width="1440" height="900" alt="Lifestyle3  'UNION SELECT NULL, username from users--" src="https://github.com/user-attachments/assets/3449e723-99be-4191-a18f-fc1098fee664" />

4. Ulangi query untuk menampilkan password `' UNION SELECT NULL, password FROM users--`
<img width="1440" height="900" alt="Lifestyle4  'UNION SELECT NULL, password from users -" src="https://github.com/user-attachments/assets/dba82190-dd5c-403f-bca2-5210d8ec1063" />

5. Menentukan jenis database. Jenis database penting untuk menentukan syntax concatenation string. Coba cek MySQL / Microsoft SQL Server style dengan payload `' UNION SELECT NULL, @@version--`. Hasilnya error. Saat dicoba untuk PostgreSQL style dengan payload `' UNION SELECT NULL, version()--`, hasilnya berhasil. Database menggunakan PostgreSQL 12.22.
<img width="1440" height="900" alt="Screenshot 2025-09-10 at 00 43 01" src="https://github.com/user-attachments/assets/54b40060-0099-4d5c-a47f-0dde3d886ad0" />

6. Menggabungkan Username dan Password. Di PostgreSQL, concatenation string dilakukan dengan operator `||`. Masukkan payload berikut `' UNION SELECT NULL, username || password FROM users--`. Berhasil muncul username disertai passwordnya.
<img width="1440" height="900" alt="Lifestyle'UNION SELECT NULL, username  password from" src="https://github.com/user-attachments/assets/f23cf748-d9fc-4999-aeb1-a54473f0fed5" />

7. Coba login menggunakan akun milik administrator, dan lab berhasil solved!!
<img width="1440" height="900" alt="My Account" src="https://github.com/user-attachments/assets/3a285b8b-2964-4004-a329-d146c70a2e6e" />

## Catatan
- Hasil: Berhasil.
- Alasan: Setelah menemukan jumlah kolom (2) dan tipe data string di kolom kedua, eksploitasi berhasil menampilkan data sensitif dari tabel users. Mengetahui bahwa database menggunakan PostgreSQL memungkinkan penggunaan operator || untuk concatenation sehingga username dan password bisa digabung dalam satu kolom.
- Refleksi: Awalnya sempat gagal saat mencoba @@version karena itu adalah syntax untuk SQL Server/MySQL, bukan PostgreSQL. Dengan beralih ke fungsi version(), informasi DBMS teridentifikasi. Dari sisi keamanan, ini menegaskan bahwa error messages, UNION-based injection, dan fungsi bawaan DBMS bisa dimanfaatkan attacker untuk mengekstrak data penting. Implementasi parameterized queries dan least privilege principle menjadi mitigasi utama terhadap serangan semacam ini.
