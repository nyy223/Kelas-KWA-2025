# Spookifier
https://app.hackthebox.com/challenges/Spookifier

## Langkah Eksploitasi
1. Setelah mendownload source yang disediakan, akan didapati beberapa file sebagai berikut
<img width="171" height="516" alt="# index css" src="https://github.com/user-attachments/assets/b6c62b41-d07f-4a31-8464-a3a08194b441" />

2. Pada file application/blueprints/routes.py, ita dapat melihat input yang diberikan pengguna ditetapkan ke variabel teks, lalu diteruskan ke fungsi spookify(). Kita juga melihat bahwa fungsi tersebut diambil dari util.py dengan melihat impor berkas tersebut. Kemungkinan besar, di sinilah kerentanannya berada.

<img width="534" height="178" alt="Screenshot 2025-09-10 at 12 38 06" src="https://github.com/user-attachments/assets/19a166c4-da32-43b6-bf03-5ca31f2b02f3" />

3. Pada file util.py, kita melihat ada fungsi generate_render() sebagai kode yang rentan.

<img width="290" height="389" alt="Screenshot 2025-09-10 at 12 44 31" src="https://github.com/user-attachments/assets/55473652-d499-421f-b018-d0142e16509f" />

4. Setelah menemukan kerentanan, buka website yang disediakan di soal lalu masukkan payload berikut `<% import os; x = os.popen('find / -type f -name flag.txt').read() %> ${x}`. Payload tersebut berfungsi untuk mengeksekusi perintah sistem find / -type f -name flag.txt melalui template injection, menyimpan hasilnya ke variabel x, lalu menampilkan isinya di output web.
<img width="1440" height="900" alt="Name Spookiher" src="https://github.com/user-attachments/assets/0a149c07-b484-4e3a-97bb-d1a28b6e8e7a" />

5. Untuk mendapatkan flag, masukkan payload `<% import os; x = os.popen('cat /flag.txt').read() %> ${x}`
<img width="1440" height="900" alt="Screenshot 2025-09-09 at 14 22 16" src="https://github.com/user-attachments/assets/cad6cb6e-205f-4207-9904-cb66b90c9b42" />

## Catatan
- Hasil: Berhasil.
- Alasan: Payload SSTI <% import os; x = os.popen('find / -type f -name flag.txt').read() %> ${x} berhasil dieksekusi oleh Mako template engine sehingga dapat menampilkan lokasi file flag.txt, kemudian payload lanjutan untuk cat /flag.txt berhasil membaca isinya.
- Refleksi: Awalnya sempat membingungkan karena fungsi generate_render() langsung menggabungkan input user dengan template tanpa sanitasi, sehingga input dievaluasi sebagai kode Python oleh Mako. Dengan mencoba payload sederhana untuk eksekusi perintah sistem, terbukti aplikasi rentan terhadap SSTI. Dari sisi keamanan, hal ini menunjukkan pentingnya memisahkan data user dari template logic dengan menggunakan parameter rendering yang aman, serta menghindari concatenation langsung.
