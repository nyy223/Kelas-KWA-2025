# Login Jim

## Objective 
Log in with Jim's user account.

## Langkah Eksploitasi
1. Masuk ke halaman login, terdapat field untuk mengisi bagian email dan password.
2. Karena kita belum mengetahui informasi apapun mengenai akun milik Jim, maka kita bisa coba explore untuk mencari tahu mengenai hal tersebut. Caranya adalah melihat review dari masing masing produk yang tertera untuk menemukan email milik Jim.
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 21 44 50" src="https://github.com/user-attachments/assets/51185ddf-4b87-4524-bb83-f50f6be68cff" />

Di review salah satu produk, kita bisa mendapati bahwa email Jim adalah `jim@juice-sh.op`

3. Di field email, masukkan `jim@juice-sh.op--` dan masukkan bebas di bagian password
4. Payload `--` akan mengomentari sisa query. Pengecekan password diabaikan sehingga database hanya memvalidasi email dan langsung memberi akses ke akun tersebut.
5. Kita pun berhasil masuk ke akun Jim
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 21 45 57" src="https://github.com/user-attachments/assets/34bf0b4b-1be1-46e7-b379-a709041a4a1e" />


