# Login Bender

## Objective
Log in with Bender's user account.

## Langkah Eksploitasi
1. Sama seperti Login Jim, karena kita belum mendapatkan informasi apapun mengenai akun Bender, maka kita bisa cobe explore dulu untuk menemukan akun email Bender. Kali ini, kita coba explore di bagian review dari setiap user.
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 21 47 32" src="https://github.com/user-attachments/assets/557b9084-994d-4145-a035-b9ee9030a423" />

Di gambar, kita bisa melihat bahwa akun email bender adalah `bender@juice-sh.op`

2. Di field email, masukkan `bender@juice-sh.op'--` dan masukkan bebas di bagian password. Payload -- akan mengomentari sisa query. Pengecekan password diabaikan sehingga database hanya memvalidasi email dan langsung memberi akses ke akun tersebut.
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 21 51 26" src="https://github.com/user-attachments/assets/943f5c1e-524a-4aea-8179-c6be3c20cb5f" />
3. Kita pun berhasil masuk ke akun Bender
<img width="1440" height="900" alt="Screenshot 2025-09-08 at 21 51 51" src="https://github.com/user-attachments/assets/d8c60c69-4fa7-4d16-ac95-3de8547591e8" />


