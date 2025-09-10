# SQL injection UNION attack, finding a column containing text
https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text

## Langkah Eksploitasi
1. Langkah pertama adalah mengetahui berapa banyak kolom yang direturn oleh query asli. Caranya menggunakan ORDER BY. Payload hingga ORDER BY 3 tidak error. Sedangkan, payload ORDER BY 4 menimbulkan error. Kesimpulannya, Query memiliki 3 kolom.
<img width="1326" height="484" alt="Screenshot 2025-09-09 at 23 58 49" src="https://github.com/user-attachments/assets/ca055370-0f98-4c60-90bc-6808fe919d50" />

2. Selanjutnya, kita harus mencari kolom mana yang bisa menerima data bertipe string. Caranya adalah dengan mencoba UNION SELECT dengan kombinasi string 'a' dan NULL. Payload yang berhasil dari `' UNION SELECT NULL, 'a', NULL--`. Sehingga dapat disimpulkan bahwa kolom kedua bertipe data string.
<img width="1384" height="843" alt="Screenshot 2025-09-09 at 23 59 15" src="https://github.com/user-attachments/assets/6b3103b5-0d58-45b7-9d64-1fe665dd3e2b" />

3. Soal meminta kita untuk menampilkan string RiDmZs. Karena kolom kedua bisa menampung string, masukkan payload `' UNION SELECT NULL, 'RiDmZs', NULL--`
<img width="1311" height="753" alt="Web Security" src="https://github.com/user-attachments/assets/5dc51f47-d729-4535-b17d-c00a06bfcf3e" />

Berhasil dan lab solved!!

## Catatan
- Hasil: Berhasil.
- Alasan: Dengan teknik ORDER BY, ditemukan jumlah kolom ada 3. Kolom kedua berhasil menerima data string saat diuji dengan payload 'a'. Selanjutnya, memasukkan string RiDmZs pada kolom kedua membuat database mengembalikan string sesuai permintaan soal.
- Refleksi: Awalnya sempat gagal ketika mencoba menaruh 'a' di kolom pertama dan kombinasi lain, karena tipe data tidak cocok. Setelah memahami bahwa SQL UNION mensyaratkan kecocokan tipe data antar kolom, pendekatan sistematis (uji kolom satu per satu) berhasil. Dari sisi keamanan, hal ini menunjukkan pentingnya validasi input, parameterized queries, dan minimisasi error feedback agar attacker tidak bisa memetakan struktur query secara bertahap.
