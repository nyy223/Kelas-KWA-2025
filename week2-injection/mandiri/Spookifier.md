# Spookifier
https://app.hackthebox.com/challenges/Spookifier

## Langkah Eksploitasi
1. Setelah mendownload source yang disediakan, akan didapati beberapa file sebagai berikut
<img width="171" height="516" alt="# index css" src="https://github.com/user-attachments/assets/b6c62b41-d07f-4a31-8464-a3a08194b441" />
<img width="267" height="101" alt="= flag txt" src="https://github.com/user-attachments/assets/945815f8-701f-4b39-9c2e-976612aa2a77" />
2. Pada file application/blueprints/routes.py, ita dapat melihat input yang diberikan pengguna ditetapkan ke variabel teks, lalu diteruskan ke fungsi spookify(). Kita juga melihat bahwa fungsi tersebut diambil dari util.py dengan melihat impor berkas tersebut. Kemungkinan besar, di sinilah kerentanannya berada.
<img width="534" height="178" alt="Screenshot 2025-09-10 at 12 38 06" src="https://github.com/user-attachments/assets/19a166c4-da32-43b6-bf03-5ca31f2b02f3" />
3. Pada file util.py, kita melihat ada fungsi generate_render() sebagai kode yang rentan.
<img width="290" height="389" alt="Screenshot 2025-09-10 at 12 44 31" src="https://github.com/user-attachments/assets/55473652-d499-421f-b018-d0142e16509f" />
4. 
