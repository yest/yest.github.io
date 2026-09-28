<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Modul Basis Data - Normalisasi</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Arial, sans-serif;
    line-height: 1.7;
    color: #2c3e50;
    max-width: 1100px;
    margin: 0 auto;
    padding: 25px;
    background: #f8f9fc;
  }
  h1 {
    color: #0b3a6b;
    border-bottom: 4px solid #1a5490;
    padding-bottom: 12px;
    font-size: 2em;
    text-align: center;
    background: linear-gradient(135deg, #e3f2fd 0%, #ffffff 100%);
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  }
  h2 {
    color: #ffffff;
    background: linear-gradient(135deg, #1a5490 0%, #2a6cb8 100%);
    padding: 12px 18px;
    border-radius: 8px;
    margin-top: 35px;
    box-shadow: 0 3px 6px rgba(26,84,144,0.25);
    font-size: 1.4em;
  }
  h3 {
    color: #1a5490;
    margin-top: 22px;
    border-left: 5px solid #1a5490;
    padding-left: 12px;
    background: #f0f6ff;
    padding: 8px 12px;
    border-radius: 0 6px 6px 0;
  }
  h4 {
    color: #2a6cb8;
    margin-top: 18px;
    font-weight: 600;
  }

  /* ===== TABEL PREMIUM ===== */
  table {
    border-collapse: separate;
    border-spacing: 0;
    width: 100%;
    margin: 20px 0;
    background: #ffffff;
    border: 2px solid #1a5490;
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    font-size: 0.95em;
  }
  table thead tr {
    background: linear-gradient(135deg, #1a5490 0%, #2a6cb8 100%);
    color: #ffffff;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    font-size: 0.9em;
  }
  table th {
    padding: 14px 16px;
    text-align: left;
    border-right: 1px solid rgba(255,255,255,0.25);
    border-bottom: 2px solid #0b3a6b;
  }
  table th:last-child { border-right: none; }
  table td {
    padding: 12px 16px;
    border-bottom: 1px solid #e0e6ed;
    border-right: 1px solid #e0e6ed;
    vertical-align: top;
  }
  table td:last-child { border-right: none; }
  table tbody tr {
    transition: all 0.2s ease;
  }
  table tbody tr:nth-child(even) {
    background-color: #f7faff;
  }
  table tbody tr:nth-child(odd) {
    background-color: #ffffff;
  }
  table tbody tr:hover {
    background-color: #e3f2fd;
    transform: scale(1.005);
    box-shadow: inset 0 0 0 1px #1a5490;
  }
  table tbody tr:last-child td {
    border-bottom: none;
  }
  /* Tabel kecil untuk kunci jawaban */
  table.kunci-jawaban {
    max-width: 300px;
    margin: 15px auto;
  }
  table.kunci-jawaban td:last-child {
    text-align: center;
    font-weight: bold;
    color: #1a5490;
    font-size: 1.1em;
  }

  blockquote {
    background: linear-gradient(135deg, #f0f6ff 0%, #e3f2fd 100%);
    border-left: 6px solid #1a5490;
    margin: 18px 0;
    padding: 14px 22px;
    font-style: italic;
    border-radius: 0 8px 8px 0;
    box-shadow: 0 2px 6px rgba(26,84,144,0.08);
  }
  pre {
    background: #2d2d2d;
    color: #f8f8f2;
    padding: 18px;
    border-radius: 8px;
    overflow-x: auto;
    font-family: 'Courier New', monospace;
    font-size: 0.9em;
    box-shadow: 0 3px 8px rgba(0,0,0,0.2);
    border-left: 4px solid #1a5490;
  }
  code {
    background: #fff3e0;
    padding: 2px 7px;
    border-radius: 4px;
    font-family: 'Courier New', monospace;
    color: #d84315;
    font-weight: 600;
    border: 1px solid #ffe0b2;
  }
  pre code {
    background: transparent;
    color: #f8f8f2;
    padding: 0;
    border: none;
    font-weight: normal;
  }
  .note {
    background: linear-gradient(135deg, #fff8e1 0%, #fff3c4 100%);
    border-left: 6px solid #f9a825;
    padding: 12px 18px;
    margin: 18px 0;
    border-radius: 0 8px 8px 0;
    box-shadow: 0 2px 6px rgba(249,168,37,0.15);
  }
  .warning {
    background: linear-gradient(135deg, #ffebee 0%, #ffcdd2 100%);
    border-left: 6px solid #c62828;
    padding: 12px 18px;
    margin: 18px 0;
    border-radius: 0 8px 8px 0;
    box-shadow: 0 2px 6px rgba(198,40,40,0.15);
  }
  .info {
    background: linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%);
    border-left: 6px solid #1976d2;
    padding: 12px 18px;
    margin: 18px 0;
    border-radius: 0 8px 8px 0;
    box-shadow: 0 2px 6px rgba(25,118,210,0.15);
  }
  .soal {
    background: #ffffff;
    border: 2px solid #1a5490;
    border-left: 8px solid #1a5490;
    padding: 18px 22px;
    margin: 18px 0;
    border-radius: 8px;
    box-shadow: 0 3px 8px rgba(26,84,144,0.1);
  }
  .soal:hover {
    box-shadow: 0 5px 15px rgba(26,84,144,0.2);
    transform: translateY(-2px);
    transition: all 0.3s ease;
  }
  hr {
    border: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent 0%, #1a5490 50%, transparent 100%);
    margin: 35px 0;
  }
  ul, ol { padding-left: 25px; }
  li { margin: 6px 0; }
  strong { color: #0b3a6b; }
  a { color: #1a5490; text-decoration: none; font-weight: 600; }
  a:hover { text-decoration: underline; }
</style>
</head>
<body>

<h1>MODUL MATA KULIAH BASIS DATA<br><small style="font-size:0.6em;color:#2a6cb8;">TOPIK: NORMALISASI BASIS DATA</small></h1>

<h2>DAFTAR ISI</h2>
<ol>
  <li><a href="#bag1">Pengantar &amp; Hubungan dengan ERD</a></li>
  <li><a href="#bag2">Konsep Dasar Normalisasi</a></li>
  <li><a href="#bag3">Anomali Data</a></li>
  <li><a href="#bag4">Ketergantungan Fungsional</a></li>
  <li><a href="#bag5">Bentuk Normal Pertama (1NF)</a></li>
  <li><a href="#bag6">Bentuk Normal Kedua (2NF)</a></li>
  <li><a href="#bag7">Bentuk Normal Ketiga (3NF)</a></li>
  <li><a href="#bag8">Bentuk Normal Boyce-Codd (BCNF)</a></li>
  <li><a href="#bag9">Bentuk Normal Keempat (4NF)</a></li>
  <li><a href="#bag10">Bentuk Normal Kelima (5NF)</a></li>
  <li><a href="#bag11">Ringkasan &amp; Perbandingan</a></li>
  <li><a href="#bag12">Soal Latihan</a></li>
</ol>

<hr>

<h2 id="bag1">1. Pengantar &amp; Hubungan dengan ERD</h2>

<h3>1.1 Kilas Balik: Entity-Relationship Diagram (ERD)</h3>
<p>Pada pertemuan-pertemuan sebelumnya, kita telah mempelajari <strong>Entity-Relationship Diagram (ERD)</strong> sebagai alat pemodelan data secara <strong>konseptual</strong>. ERD membantu kita mengidentifikasi:</p>
<ul>
  <li><strong>Entitas</strong> (objek-objek utama dalam sistem, misalnya: Mahasiswa, Mata Kuliah, Dosen)</li>
  <li><strong>Atribut</strong> (karakteristik dari setiap entitas, misalnya: NIM, Nama, Alamat)</li>
  <li><strong>Relasi</strong> (hubungan antar entitas, misalnya: Mahasiswa <em>mengambil</em> Mata Kuliah)</li>
  <li><strong>Kardinalitas</strong> (1:1, 1:N, M:N)</li>
</ul>

<h3>1.2 Dari ERD ke Tabel Relasional</h3>
<p>Setelah ERD selesai dirancang, langkah selanjutnya adalah <strong>mentransformasikan</strong> model konseptual (ERD) menjadi <strong>model logis</strong>, yaitu sekumpulan <strong>tabel relasional</strong>. Proses transformasi umum meliputi:</p>

<table>
  <thead>
    <tr><th>Komponen ERD</th><th>Hasil Transformasi</th></tr>
  </thead>
  <tbody>
    <tr><td>Entitas kuat</td><td>Tabel terpisah</td></tr>
    <tr><td>Entitas lemah</td><td>Tabel dengan <em>foreign key</em> ke entitas induk</td></tr>
    <tr><td>Atribut sederhana</td><td>Kolom dalam tabel</td></tr>
    <tr><td>Atribut multivalue</td><td>Tabel terpisah</td></tr>
    <tr><td>Relasi 1:1</td><td><em>Foreign key</em> di salah satu tabel</td></tr>
    <tr><td>Relasi 1:N</td><td><em>Foreign key</em> di sisi "N"</td></tr>
    <tr><td>Relasi M:N</td><td>Tabel baru (tabel junction/associative)</td></tr>
  </tbody>
</table>

<h3>1.3 Mengapa ERD Saja Tidak Cukup?</h3>
<p>Meskipun ERD sudah memberikan gambaran struktur data, <strong>hasil transformasi langsung dari ERD ke tabel belum tentu optimal</strong>. Sering kali kita mendapatkan tabel-tabel yang:</p>
<ul>
  <li>Mengandung <strong>redundansi</strong> (data berulang)</li>
  <li>Rentan terhadap <strong>anomali</strong> (kesalahan saat insert, update, delete)</li>
  <li>Memiliki <strong>atribut yang tidak bergantung penuh</strong> pada kunci utama</li>
  <li>Menyimpan <strong>data campuran</strong> yang seharusnya dipisah</li>
</ul>

<blockquote><strong>Inilah alasan mengapa kita memerlukan NORMALISASI.</strong></blockquote>

<h3>1.4 Posisi Normalisasi dalam Siklus Pengembangan Basis Data</h3>
<pre><code>┌─────────────────────────────────────────────────────────┐
│          SIKLUS PENGEMBANGAN BASIS DATA                 │
│                                                         │
│  Analisis Kebutuhan                                     │
│       ↓                                                 │
│  Perancangan Konseptual (ERD)  ◄── Materi sebelumnya    │
│       ↓                                                 │
│  Perancangan Logis (Transformasi ERD → Tabel)           │
│       ↓                                                 │
│  ★ NORMALISASI ★  ◄── MATERI SAAT INI                   │
│       ↓                                                 │
│  Perancangan Fisik (DBMS, indexing, dll.)               │
│       ↓                                                 │
│  Implementasi (SQL DDL/DML)                             │
│       ↓                                                 │
│  Testing &amp; Deployment                                   │
└─────────────────────────────────────────────────────────┘</code></pre>

<p><strong>Normalisasi</strong> adalah proses <strong>evaluasi dan perbaikan</strong> struktur tabel hasil transformasi ERD agar memenuhi standar kualitas tertentu yang disebut <strong>bentuk normal (normal form)</strong>.</p>

<hr>

<h2 id="bag2">2. Konsep Dasar Normalisasi</h2>

<h3>2.1 Definisi</h3>
<blockquote>
<strong>Normalisasi</strong> adalah proses pengorganisasian data dalam basis data dengan tujuan:
<ol>
  <li><strong>Mengeliminasi redundansi</strong> (pengulangan data yang tidak perlu)</li>
  <li><strong>Mengeliminasi anomali</strong> (ketidakkonsistenan data saat operasi insert, update, delete)</li>
  <li><strong>Memastikan ketergantungan data</strong> masuk akal (data disimpan di tempat yang tepat)</li>
</ol>
</blockquote>

<h3>2.2 Sejarah Singkat</h3>
<p>Konsep normalisasi pertama kali diperkenalkan oleh <strong>Edgar F. Codd</strong> pada tahun <strong>1970</strong> dalam makalahnya yang revolusioner <em>"A Relational Model of Data for Large Shared Data Banks"</em>. Codd mendefinisikan <strong>Bentuk Normal Pertama (1NF)</strong>. Selanjutnya, Codd bersama <strong>Raymond F. Boyce</strong> mengembangkan <strong>BCNF</strong> pada tahun 1974. Bentuk normal yang lebih tinggi (4NF, 5NF) dikembangkan oleh <strong>Ronald Fagin</strong> pada tahun 1977 dan 1979.</p>

<h3>2.3 Prinsip Utama</h3>
<p>Normalisasi bekerja dengan prinsip <strong>dekomposisi</strong> (pemecahan):</p>
<ul>
  <li>Satu tabel besar yang bermasalah dipecah menjadi <strong>beberapa tabel kecil</strong> yang lebih terstruktur.</li>
  <li>Tabel-tabel kecil tersebut tetap dapat <strong>dihubungkan kembali</strong> melalui <em>primary key</em> dan <em>foreign key</em> (sifat <strong>lossless join</strong>).</li>
</ul>

<h3>2.4 Tingkatan Bentuk Normal</h3>
<pre><code>  1NF  →  2NF  →  3NF  →  BCNF  →  4NF  →  5NF
  ──────────────────────────────────────────────►
  Semakin tinggi tingkat normalisasi,
  semakin ketat aturannya,
  semakin sedikit redundansi.</code></pre>

<div class="note"><strong>Catatan penting:</strong> Dalam praktik industri, normalisasi hingga <strong>3NF atau BCNF</strong> umumnya sudah dianggap <strong>cukup</strong>. Bentuk normal 4NF dan 5NF lebih bersifat akademis dan digunakan pada kasus-kasus khusus.</div>

<hr>

<h2 id="bag3">3. Anomali Data</h2>

<p>Sebelum masuk ke bentuk-bentuk normal, kita perlu memahami <strong>mengapa</strong> normalisasi diperlukan. Jawabannya terletak pada <strong>anomali data</strong>.</p>

<h3>3.1 Contoh Tabel yang Belum Dinormalisasi</h3>
<p>Perhatikan tabel <strong>KULIAH_MAHASISWA</strong> berikut yang merupakan hasil transformasi langsung dari ERD tanpa normalisasi:</p>

<table>
  <thead>
    <tr>
      <th>NIM</th><th>Nama_Mhs</th><th>Alamat</th><th>Kode_MK</th><th>Nama_MK</th><th>SKS</th><th>NIP_Dosen</th><th>Nama_Dosen</th><th>Nilai</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>101</td><td>Andi</td><td>Jakarta</td><td>MK01</td><td>Basis Data</td><td>3</td><td>D01</td><td>Prof. Susi</td><td>A</td></tr>
    <tr><td>101</td><td>Andi</td><td>Jakarta</td><td>MK02</td><td>Pemrograman</td><td>4</td><td>D02</td><td>Dr. Budi</td><td>B</td></tr>
    <tr><td>102</td><td>Siti</td><td>Bandung</td><td>MK01</td><td>Basis Data</td><td>3</td><td>D01</td><td>Prof. Susi</td><td>A</td></tr>
    <tr><td>102</td><td>Siti</td><td>Bandung</td><td>MK03</td><td>Jaringan</td><td>3</td><td>D03</td><td>Dr. Rina</td><td>C</td></tr>
    <tr><td>103</td><td>Doni</td><td>Surabaya</td><td>MK02</td><td>Pemrograman</td><td>4</td><td>D02</td><td>Dr. Budi</td><td>A</td></tr>
  </tbody>
</table>

<h3>3.2 Tiga Jenis Anomali</h3>

<h4>a) Anomali Penyisipan (<em>Insertion Anomaly</em>)</h4>
<p><strong>Masalah:</strong> Kita <strong>tidak bisa</strong> menambahkan data baru tanpa data lain yang menyertainya.</p>
<div class="info"><strong>Contoh:</strong> Kita ingin menambahkan mata kuliah baru "Kecerdasan Buatan" (MK04, 3 SKS) yang diajarkan oleh Dr. Rina. Namun, karena belum ada mahasiswa yang mengambil mata kuliah tersebut, kita <strong>tidak bisa</strong> menyisipkan baris baru karena kolom NIM (sebagai bagian dari <em>primary key</em>) tidak boleh NULL.</div>

<h4>b) Anomali Pembaruan (<em>Update Anomaly</em>)</h4>
<p><strong>Masalah:</strong> Perubahan satu data mengharuskan kita mengubah <strong>banyak baris</strong> secara konsisten.</p>
<div class="info"><strong>Contoh:</strong> Jika Prof. Susi pindah alamat kantor dan namanya berubah menjadi "Prof. Susi Hartono", kita harus mengubah <strong>semua baris</strong> yang mengandung NIP_Dosen = D01 (baris 1 dan 3). Jika kita lupa mengubah salah satu, data menjadi <strong>tidak konsisten</strong>.</div>

<h4>c) Anomali Penghapusan (<em>Deletion Anomaly</em>)</h4>
<p><strong>Masalah:</strong> Menghapus satu data menyebabkan <strong>hilangnya data lain</strong> yang sebenarnya masih diperlukan.</p>
<div class="info"><strong>Contoh:</strong> Jika mahasiswa Doni (NIM 103) keluar dari universitas dan kita menghapus barisnya, maka informasi bahwa mata kuliah "Pemrograman" diajarkan oleh Dr. Budi juga <strong>ikut hilang</strong> (karena hanya Doni satu-satunya yang mengambil MK02 di data saat ini).</div>

<hr>

<h2 id="bag4">4. Ketergantungan Fungsional (<em>Functional Dependency</em>)</h2>

<h3>4.1 Definisi</h3>
<blockquote>
<strong>Ketergantungan Fungsional (FD)</strong> adalah hubungan antara dua himpunan atribut dalam sebuah relasi, di mana nilai dari satu himpunan atribut <strong>menentukan secara unik</strong> nilai dari himpunan atribut lainnya.
</blockquote>

<p><strong>Notasi:</strong> <code>X → Y</code> dibaca "X menentukan Y secara fungsional" atau "Y bergantung secara fungsional pada X".</p>
<p>Artinya: Untuk setiap pasangan baris dalam tabel, jika nilai X sama, maka nilai Y <strong>pasti</strong> sama.</p>

<h3>4.2 Contoh Ketergantungan Fungsional</h3>
<p>Dari tabel KULIAH_MAHASISWA di atas, kita dapat mengidentifikasi FD berikut:</p>

<table>
  <thead>
    <tr><th>No</th><th>FD</th><th>Penjelasan</th></tr>
  </thead>
  <tbody>
    <tr><td>1</td><td><code>NIM → Nama_Mhs, Alamat</code></td><td>Setiap NIM menentukan satu nama dan alamat mahasiswa</td></tr>
    <tr><td>2</td><td><code>Kode_MK → Nama_MK, SKS</code></td><td>Setiap kode MK menentukan nama dan SKS mata kuliah</td></tr>
    <tr><td>3</td><td><code>NIP_Dosen → Nama_Dosen</code></td><td>Setiap NIP menentukan satu nama dosen</td></tr>
    <tr><td>4</td><td><code>NIM, Kode_MK → Nilai</code></td><td>Kombinasi NIM dan Kode MK menentukan nilai</td></tr>
    <tr><td>5</td><td><code>Kode_MK → NIP_Dosen, Nama_Dosen</code></td><td>Setiap MK diajarkan oleh satu dosen tertentu</td></tr>
  </tbody>
</table>

<h3>4.3 Jenis-Jenis Ketergantungan Fungsional</h3>

<h4>a) Ketergantungan Fungsional Penuh (<em>Full Functional Dependency</em>)</h4>
<p><code>X → Y</code> adalah ketergantungan penuh jika Y bergantung pada <strong>seluruh</strong> atribut dalam X, bukan hanya sebagian.</p>
<div class="info"><strong>Contoh:</strong> <code>{NIM, Kode_MK} → Nilai</code> adalah ketergantungan <strong>penuh</strong> karena Nilai tidak bisa ditentukan hanya oleh NIM saja atau Kode_MK saja.</div>

<h4>b) Ketergantungan Fungsional Parsial (<em>Partial Functional Dependency</em>)</h4>
<p><code>X → Y</code> adalah ketergantungan parsial jika Y hanya bergantung pada <strong>sebagian</strong> dari X.</p>
<div class="info"><strong>Contoh:</strong> <code>{NIM, Kode_MK} → Nama_Mhs</code> adalah ketergantungan <strong>parsial</strong> karena Nama_Mhs hanya bergantung pada NIM saja, tidak perlu Kode_MK.</div>

<h4>c) Ketergantungan Fungsional Transitif (<em>Transitive Functional Dependency</em>)</h4>
<p><code>X → Z</code> adalah ketergantungan transitif jika terdapat atribut Y sehingga <code>X → Y</code> dan <code>Y → Z</code>, di mana Y <strong>bukan</strong> bagian dari X dan X <strong>tidak</strong> bergantung pada Y.</p>
<div class="info"><strong>Contoh:</strong> <code>NIM → Jurusan</code> dan <code>Jurusan → Dekan</code>, maka <code>NIM → Dekan</code> adalah ketergantungan <strong>transitif</strong> melalui Jurusan.</div>

<hr>

<h2 id="bag5">5. Bentuk Normal Pertama (1NF)</h2>

<h3>5.1 Syarat 1NF</h3>
<p>Sebuah tabel memenuhi <strong>1NF</strong> jika:</p>
<ul>
  <li>✅ Setiap kolom berisi <strong>nilai atomik</strong> (tidak dapat dipecah lagi)</li>
  <li>✅ <strong>Tidak ada</strong> kelompok atribut yang berulang (<em>repeating groups</em>)</li>
  <li>✅ Setiap baris bersifat <strong>unik</strong> (terdapat <em>primary key</em>)</li>
  <li>✅ Setiap kolom memiliki <strong>satu tipe data</strong> yang konsisten</li>
</ul>

<h3>5.2 Contoh Pelanggaran 1NF</h3>
<p>Perhatikan tabel <strong>MAHASISWA</strong> berikut:</p>

<table>
  <thead>
    <tr><th>NIM</th><th>Nama</th><th>Telepon</th><th>Mata_Kuliah</th></tr>
  </thead>
  <tbody>
    <tr><td>101</td><td>Andi</td><td>081111, 082222</td><td>Basis Data, Pemrograman</td></tr>
    <tr><td>102</td><td>Siti</td><td>083333</td><td>Jaringan</td></tr>
    <tr><td>103</td><td>Doni</td><td>084444, 085555, 086666</td><td>Basis Data, Jaringan, AI</td></tr>
  </tbody>
</table>

<p><strong>Pelanggaran:</strong></p>
<ul>
  <li>Kolom <strong>Telepon</strong> berisi banyak nilai (multivalue) → tidak atomik</li>
  <li>Kolom <strong>Mata_Kuliah</strong> berisi banyak nilai → <em>repeating group</em></li>
</ul>

<h3>5.3 Proses Normalisasi ke 1NF</h3>
<p><strong>Langkah 1:</strong> Pisahkan nilai-nilai multivalue menjadi baris-baris terpisah.</p>

<p><strong>Hasil 1NF:</strong></p>
<table>
  <thead>
    <tr><th>NIM</th><th>Nama</th><th>Telepon</th><th>Mata_Kuliah</th></tr>
  </thead>
  <tbody>
    <tr><td>101</td><td>Andi</td><td>081111</td><td>Basis Data</td></tr>
    <tr><td>101</td><td>Andi</td><td>082222</td><td>Basis Data</td></tr>
    <tr><td>101</td><td>Andi</td><td>081111</td><td>Pemrograman</td></tr>
    <tr><td>101</td><td>Andi</td><td>082222</td><td>Pemrograman</td></tr>
    <tr><td>102</td><td>Siti</td><td>083333</td><td>Jaringan</td></tr>
    <tr><td>103</td><td>Doni</td><td>084444</td><td>Basis Data</td></tr>
    <tr><td>103</td><td>Doni</td><td>085555</td><td>Basis Data</td></tr>
    <tr><td>103</td><td>Doni</td><td>086666</td><td>Basis Data</td></tr>
    <tr><td>103</td><td>Doni</td><td>084444</td><td>Jaringan</td></tr>
    <tr><td>103</td><td>Doni</td><td>085555</td><td>Jaringan</td></tr>
    <tr><td>103</td><td>Doni</td><td>086666</td><td>Jaringan</td></tr>
  </tbody>
</table>

<p><strong>Primary Key:</strong> <code>{NIM, Telepon, Mata_Kuliah}</code> (kombinasi tiga atribut)</p>

<div class="warning">⚠️ <strong>Perhatikan:</strong> Tabel sudah memenuhi 1NF, tetapi <strong>redundansi sangat tinggi</strong>! Data nama "Andi" diulang 4 kali. Ini akan ditangani di 2NF.</div>

<hr>

<h2 id="bag6">6. Bentuk Normal Kedua (2NF)</h2>

<h3>6.1 Syarat 2NF</h3>
<p>Sebuah tabel memenuhi <strong>2NF</strong> jika:</p>
<ul>
  <li>✅ Sudah memenuhi <strong>1NF</strong></li>
  <li>✅ <strong>Tidak ada</strong> ketergantungan fungsional <strong>parsial</strong> terhadap <em>primary key</em>
    <ul><li>Artinya: Setiap atribut non-key harus bergantung <strong>penuh</strong> pada <strong>seluruh</strong> <em>primary key</em>, bukan hanya sebagian.</li></ul>
  </li>
</ul>

<div class="note"><strong>Catatan:</strong> Jika <em>primary key</em> hanya terdiri dari <strong>satu atribut</strong>, maka tabel <strong>otomatis</strong> memenuhi 2NF (karena tidak mungkin ada ketergantungan parsial).</div>

<h3>6.2 Contoh Pelanggaran 2NF</h3>
<p>Gunakan tabel hasil 1NF di atas dengan <strong>Primary Key = {NIM, Telepon, Mata_Kuliah}</strong>.</p>
<p>Ketergantungan fungsional yang teridentifikasi:</p>
<ul>
  <li><code>NIM → Nama</code> ← <strong>PARSIAL!</strong> (Nama hanya bergantung pada NIM, bukan pada keseluruhan PK)</li>
  <li><code>NIM → Telepon</code> ← PARSIAL</li>
</ul>

<h3>6.3 Proses Normalisasi ke 2NF</h3>
<p><strong>Langkah:</strong> Pisahkan atribut-atribut yang bergantung parsial ke tabel tersendiri.</p>

<p><strong>Tabel 1: MAHASISWA_TELEPON</strong></p>
<table>
  <thead>
    <tr><th>NIM</th><th>Nama</th><th>Telepon</th></tr>
  </thead>
  <tbody>
    <tr><td>101</td><td>Andi</td><td>081111</td></tr>
    <tr><td>101</td><td>Andi</td><td>082222</td></tr>
    <tr><td>102</td><td>Siti</td><td>083333</td></tr>
    <tr><td>103</td><td>Doni</td><td>084444</td></tr>
    <tr><td>103</td><td>Doni</td><td>085555</td></tr>
    <tr><td>103</td><td>Doni</td><td>086666</td></tr>
  </tbody>
</table>
<p><strong>PK:</strong> <code>{NIM, Telepon}</code></p>

<p><strong>Tabel 2: MAHASISWA_MK</strong></p>
<table>
  <thead>
    <tr><th>NIM</th><th>Mata_Kuliah</th></tr>
  </thead>
  <tbody>
    <tr><td>101</td><td>Basis Data</td></tr>
    <tr><td>101</td><td>Pemrograman</td></tr>
    <tr><td>102</td><td>Jaringan</td></tr>
    <tr><td>103</td><td>Basis Data</td></tr>
    <tr><td>103</td><td>Jaringan</td></tr>
  </tbody>
</table>
<p><strong>PK:</strong> <code>{NIM, Mata_Kuliah}</code></p>

<p>✅ Sekarang tidak ada lagi ketergantungan parsial. Setiap atribut non-key bergantung penuh pada PK-nya masing-masing.</p>

<hr>

<h2 id="bag7">7. Bentuk Normal Ketiga (3NF)</h2>

<h3>7.1 Syarat 3NF</h3>
<p>Sebuah tabel memenuhi <strong>3NF</strong> jika:</p>
<ul>
  <li>✅ Sudah memenuhi <strong>2NF</strong></li>
  <li>✅ <strong>Tidak ada</strong> ketergantungan fungsional <strong>transitif</strong> terhadap <em>primary key</em>
    <ul><li>Artinya: Setiap atribut non-key harus bergantung <strong>langsung</strong> pada <em>primary key</em>, <strong>bukan</strong> melalui atribut non-key lainnya.</li></ul>
  </li>
</ul>

<blockquote><strong>Mnemonic populer:</strong> <em>"Every non-key attribute must provide a fact about the key, the whole key, and nothing but the key."</em> (Bill Kent)</blockquote>

<h3>7.2 Contoh Pelanggaran 3NF</h3>
<p>Perhatikan tabel <strong>MAHASISWA_JURUSAN</strong> berikut (sudah 2NF):</p>

<table>
  <thead>
    <tr><th>NIM</th><th>Nama</th><th>Kode_Jurusan</th><th>Nama_Jurusan</th><th>Dekan</th></tr>
  </thead>
  <tbody>
    <tr><td>101</td><td>Andi</td><td>J01</td><td>Teknik Informatika</td><td>Dr. Ahmad</td></tr>
    <tr><td>102</td><td>Siti</td><td>J02</td><td>Sistem Informasi</td><td>Dr. Lina</td></tr>
    <tr><td>103</td><td>Doni</td><td>J01</td><td>Teknik Informatika</td><td>Dr. Ahmad</td></tr>
    <tr><td>104</td><td>Rina</td><td>J03</td><td>Ilmu Komputer</td><td>Dr. Hadi</td></tr>
  </tbody>
</table>

<p><strong>Primary Key:</strong> <code>NIM</code></p>
<p><strong>Ketergantungan Fungsional:</strong></p>
<ul>
  <li><code>NIM → Nama, Kode_Jurusan</code> ✅ (langsung ke PK)</li>
  <li><code>Kode_Jurusan → Nama_Jurusan, Dekan</code> ⚠️ (transitif!)</li>
  <li><code>NIM → Nama_Jurusan, Dekan</code> (melalui Kode_Jurusan → <strong>TRANSITIF</strong>)</li>
</ul>

<p><strong>Masalah:</strong></p>
<ul>
  <li><strong>Anomali Update:</strong> Jika Dekan Teknik Informatika berganti, kita harus update semua baris dengan Kode_Jurusan = J01.</li>
  <li><strong>Anomali Insert:</strong> Tidak bisa menambahkan jurusan baru tanpa ada mahasiswa.</li>
  <li><strong>Anomali Delete:</strong> Jika semua mahasiswa TI dihapus, info jurusan TI ikut hilang.</li>
</ul>

<h3>7.3 Proses Normalisasi ke 3NF</h3>
<p><strong>Langkah:</strong> Pisahkan atribut yang bergantung transitif ke tabel tersendiri.</p>

<p><strong>Tabel 1: MAHASISWA</strong></p>
<table>
  <thead>
    <tr><th>NIM</th><th>Nama</th><th>Kode_Jurusan</th></tr>
  </thead>
  <tbody>
    <tr><td>101</td><td>Andi</td><td>J01</td></tr>
    <tr><td>102</td><td>Siti</td><td>J02</td></tr>
    <tr><td>103</td><td>Doni</td><td>J01</td></tr>
    <tr><td>104</td><td>Rina</td><td>J03</td></tr>
  </tbody>
</table>
<p><strong>PK:</strong> <code>NIM</code> | <strong>FK:</strong> <code>Kode_Jurusan → JURUSAN(Kode_Jurusan)</code></p>

<p><strong>Tabel 2: JURUSAN</strong></p>
<table>
  <thead>
    <tr><th>Kode_Jurusan</th><th>Nama_Jurusan</th><th>Dekan</th></tr>
  </thead>
  <tbody>
    <tr><td>J01</td><td>Teknik Informatika</td><td>Dr. Ahmad</td></tr>
    <tr><td>J02</td><td>Sistem Informasi</td><td>Dr. Lina</td></tr>
    <tr><td>J03</td><td>Ilmu Komputer</td><td>Dr. Hadi</td></tr>
  </tbody>
</table>
<p><strong>PK:</strong> <code>Kode_Jurusan</code></p>

<p>✅ Sekarang semua atribut non-key bergantung <strong>langsung</strong> pada PK-nya masing-masing. Tidak ada lagi ketergantungan transitif.</p>

<hr>

<h2 id="bag8">8. Bentuk Normal Boyce-Codd (BCNF)</h2>

<h3>8.1 Syarat BCNF</h3>
<p>Sebuah tabel memenuhi <strong>BCNF</strong> jika:</p>
<ul>
  <li>✅ Sudah memenuhi <strong>3NF</strong></li>
  <li>✅ Untuk <strong>setiap</strong> ketergantungan fungsional <code>X → Y</code> yang non-trivial (Y ⊄ X), <strong>X harus merupakan <em>superkey</em></strong>.</li>
</ul>

<div class="note"><strong>Perbedaan dengan 3NF:</strong> 3NF masih mengizinkan ketergantungan <code>X → Y</code> di mana X bukan superkey <strong>asalkan</strong> Y adalah bagian dari <em>candidate key</em>. BCNF <strong>tidak</strong> mengizinkan pengecualian ini. BCNF lebih ketat dari 3NF.</div>

<h3>8.2 Kapan 3NF ≠ BCNF?</h3>
<p>3NF dan BCNF <strong>selalu sama</strong> kecuali dalam kondisi khusus:</p>
<ul>
  <li>Tabel memiliki <strong>lebih dari satu <em>candidate key</em></strong></li>
  <li><em>Candidate key</em> tersebut <strong>beririsan</strong> (overlapping)</li>
  <li>Terdapat FD dari atribut non-key ke sebagian <em>candidate key</em></li>
</ul>

<h3>8.3 Contoh Kasus BCNF</h3>
<p>Perhatikan tabel <strong>JADWAL_KULIAH</strong>:</p>

<table>
  <thead>
    <tr><th>Mahasiswa</th><th>Mata_Kuliah</th><th>Dosen</th></tr>
  </thead>
  <tbody>
    <tr><td>Andi</td><td>Basis Data</td><td>Prof. Susi</td></tr>
    <tr><td>Andi</td><td>Pemrograman</td><td>Dr. Budi</td></tr>
    <tr><td>Siti</td><td>Basis Data</td><td>Prof. Susi</td></tr>
    <tr><td>Doni</td><td>Pemrograman</td><td>Dr. Budi</td></tr>
    <tr><td>Doni</td><td>Basis Data</td><td>Dr. Rina</td></tr>
  </tbody>
</table>

<p><strong>Aturan bisnis:</strong></p>
<ol>
  <li>Setiap mahasiswa mengambil beberapa mata kuliah dari beberapa dosen.</li>
  <li>Setiap dosen hanya mengajar <strong>satu</strong> mata kuliah tertentu.</li>
  <li>Setiap mata kuliah bisa diajar oleh <strong>beberapa</strong> dosen.</li>
</ol>

<p><strong>Ketergantungan Fungsional:</strong></p>
<ul>
  <li><code>{Mahasiswa, Mata_Kuliah} → Dosen</code> (mahasiswa + MK menentukan dosen)</li>
  <li><code>Dosen → Mata_Kuliah</code> (setiap dosen hanya mengajar 1 MK)</li>
</ul>

<p><strong>Candidate Keys:</strong></p>
<ul>
  <li><code>{Mahasiswa, Mata_Kuliah}</code> ✅</li>
  <li><code>{Mahasiswa, Dosen}</code> ✅ (karena Dosen → Mata_Kuliah)</li>
</ul>

<p><strong>Analisis:</strong></p>
<ul>
  <li>FD <code>Dosen → Mata_Kuliah</code>: Dosen <strong>bukan</strong> superkey, tetapi Mata_Kuliah adalah bagian dari candidate key → <strong>3NF terpenuhi</strong> (pengecualian 3NF), tetapi <strong>BCNF TIDAK terpenuhi</strong>.</li>
</ul>

<h3>8.4 Proses Normalisasi ke BCNF</h3>

<p><strong>Tabel 1: DOSEN_MK</strong></p>
<table>
  <thead>
    <tr><th>Dosen</th><th>Mata_Kuliah</th></tr>
  </thead>
  <tbody>
    <tr><td>Prof. Susi</td><td>Basis Data</td></tr>
    <tr><td>Dr. Budi</td><td>Pemrograman</td></tr>
    <tr><td>Dr. Rina</td><td>Basis Data</td></tr>
  </tbody>
</table>
<p><strong>PK:</strong> <code>Dosen</code></p>

<p><strong>Tabel 2: MAHASISWA_DOSEN</strong></p>
<table>
  <thead>
    <tr><th>Mahasiswa</th><th>Dosen</th></tr>
  </thead>
  <tbody>
    <tr><td>Andi</td><td>Prof. Susi</td></tr>
    <tr><td>Andi</td><td>Dr. Budi</td></tr>
    <tr><td>Siti</td><td>Prof. Susi</td></tr>
    <tr><td>Doni</td><td>Dr. Budi</td></tr>
    <tr><td>Doni</td><td>Dr. Rina</td></tr>
  </tbody>
</table>
<p><strong>PK:</strong> <code>{Mahasiswa, Dosen}</code></p>

<p>✅ Sekarang setiap FD memiliki determinan yang merupakan superkey. BCNF terpenuhi.</p>

<hr>

<h2 id="bag9">9. Bentuk Normal Keempat (4NF)</h2>

<h3>9.1 Konsep Ketergantungan Multivalue (<em>Multivalued Dependency - MVD</em>)</h3>
<p>Sebelum memahami 4NF, kita perlu mengenal <strong>MVD</strong>.</p>
<blockquote>
<strong>MVD</strong> <code>X →→ Y</code> terjadi ketika untuk setiap nilai X, terdapat <strong>himpunan nilai Y</strong> yang <strong>independen</strong> dari himpunan nilai atribut lainnya (Z).
</blockquote>

<h3>9.2 Syarat 4NF</h3>
<p>Sebuah tabel memenuhi <strong>4NF</strong> jika:</p>
<ul>
  <li>✅ Sudah memenuhi <strong>BCNF</strong></li>
  <li>✅ <strong>Tidak ada</strong> ketergantungan multivalue yang non-trivial</li>
</ul>

<h3>9.3 Contoh Pelanggaran 4NF</h3>
<p>Tabel <strong>DOSEN_KEAHLIAN</strong>:</p>

<table>
  <thead>
    <tr><th>NIP_Dosen</th><th>Keahlian</th><th>Sertifikasi</th></tr>
  </thead>
  <tbody>
    <tr><td>D01</td><td>Database</td><td>Oracle Certified</td></tr>
    <tr><td>D01</td><td>Database</td><td>AWS Certified</td></tr>
    <tr><td>D01</td><td>AI</td><td>TensorFlow Cert</td></tr>
    <tr><td>D02</td><td>Jaringan</td><td>CCNA</td></tr>
    <tr><td>D02</td><td>Keamanan</td><td>CEH</td></tr>
    <tr><td>D02</td><td>Keamanan</td><td>CompTIA Security+</td></tr>
  </tbody>
</table>

<p><strong>MVD yang ada:</strong></p>
<ul>
  <li><code>NIP_Dosen →→ Keahlian</code> (himpunan keahlian independen dari sertifikasi)</li>
  <li><code>NIP_Dosen →→ Sertifikasi</code> (himpunan sertifikasi independen dari keahlian)</li>
</ul>

<p><strong>Masalah:</strong> Tabel ini menghasilkan <strong>produk kartesian</strong> yang menghasilkan baris-baris redundan dan bisa menimbulkan data yang salah jika tidak dikelola dengan hati-hati.</p>

<h3>9.4 Proses Normalisasi ke 4NF</h3>

<p><strong>Tabel 1: DOSEN_KEAHLIAN</strong></p>
<table>
  <thead>
    <tr><th>NIP_Dosen</th><th>Keahlian</th></tr>
  </thead>
  <tbody>
    <tr><td>D01</td><td>Database</td></tr>
    <tr><td>D01</td><td>AI</td></tr>
    <tr><td>D02</td><td>Jaringan</td></tr>
    <tr><td>D02</td><td>Keamanan</td></tr>
  </tbody>
</table>

<p><strong>Tabel 2: DOSEN_SERTIFIKASI</strong></p>
<table>
  <thead>
    <tr><th>NIP_Dosen</th><th>Sertifikasi</th></tr>
  </thead>
  <tbody>
    <tr><td>D01</td><td>Oracle Certified</td></tr>
    <tr><td>D01</td><td>AWS Certified</td></tr>
    <tr><td>D01</td><td>TensorFlow Cert</td></tr>
    <tr><td>D02</td><td>CCNA</td></tr>
    <tr><td>D02</td><td>CEH</td></tr>
    <tr><td>D02</td><td>CompTIA Security+</td></tr>
  </tbody>
</table>

<p>✅ MVD sudah dihilangkan. 4NF terpenuhi.</p>

<hr>

<h2 id="bag10">10. Bentuk Normal Kelima (5NF)</h2>

<h3>10.1 Konsep <em>Join Dependency</em></h3>
<p>5NF berkaitan dengan <strong>ketergantungan join</strong> (<em>join dependency</em>). Sebuah relasi memenuhi 5NF jika tidak dapat didekomposisi lagi menjadi relasi-relasi yang lebih kecil tanpa kehilangan informasi (<em>lossless join</em>).</p>

<h3>10.2 Syarat 5NF</h3>
<p>Sebuah tabel memenuhi <strong>5NF</strong> (juga disebut <em>Project-Join Normal Form / PJNF</em>) jika:</p>
<ul>
  <li>✅ Sudah memenuhi <strong>4NF</strong></li>
  <li>✅ Setiap <em>join dependency</em> adalah <strong>implikasi</strong> dari <em>candidate key</em></li>
</ul>

<h3>10.3 Contoh Kasus 5NF (Kasus Klasik)</h3>
<p>Tabel <strong>PROYEK_KONSULTAN</strong> yang mencatat hubungan tiga arah:</p>

<table>
  <thead>
    <tr><th>Proyek</th><th>Konsultan</th><th>Keahlian</th></tr>
  </thead>
  <tbody>
    <tr><td>ProyekA</td><td>Andi</td><td>Database</td></tr>
    <tr><td>ProyekA</td><td>Andi</td><td>AI</td></tr>
    <tr><td>ProyekA</td><td>Siti</td><td>Database</td></tr>
    <tr><td>ProyekB</td><td>Andi</td><td>Database</td></tr>
    <tr><td>ProyekB</td><td>Doni</td><td>AI</td></tr>
  </tbody>
</table>

<p><strong>Aturan bisnis:</strong></p>
<ul>
  <li>Jika Andi bekerja di ProyekA dan Andi memiliki keahlian Database, dan ProyekA membutuhkan keahlian Database, maka Andi <strong>harus</strong> ditugaskan sebagai konsultan Database di ProyekA.</li>
</ul>

<p>Tabel ini <strong>bisa</strong> didekomposisi menjadi <strong>tiga</strong> tabel:</p>

<p><strong>Tabel 1: PROYEK_KONSULTAN</strong></p>
<table>
  <thead>
    <tr><th>Proyek</th><th>Konsultan</th></tr>
  </thead>
  <tbody>
    <tr><td>ProyekA</td><td>Andi</td></tr>
    <tr><td>ProyekA</td><td>Siti</td></tr>
    <tr><td>ProyekB</td><td>Andi</td></tr>
    <tr><td>ProyekB</td><td>Doni</td></tr>
  </tbody>
</table>

<p><strong>Tabel 2: KONSULTAN_KEAHLIAN</strong></p>
<table>
  <thead>
    <tr><th>Konsultan</th><th>Keahlian</th></tr>
  </thead>
  <tbody>
    <tr><td>Andi</td><td>Database</td></tr>
    <tr><td>Andi</td><td>AI</td></tr>
    <tr><td>Siti</td><td>Database</td></tr>
    <tr><td>Doni</td><td>AI</td></tr>
  </tbody>
</table>

<p><strong>Tabel 3: PROYEK_KEAHLIAN</strong></p>
<table>
  <thead>
    <tr><th>Proyek</th><th>Keahlian</th></tr>
  </thead>
  <tbody>
    <tr><td>ProyekA</td><td>Database</td></tr>
    <tr><td>ProyekA</td><td>AI</td></tr>
    <tr><td>ProyekB</td><td>Database</td></tr>
    <tr><td>ProyekB</td><td>AI</td></tr>
  </tbody>
</table>

<p>✅ Dengan melakukan <em>natural join</em> ketiga tabel ini, kita mendapatkan kembali data asli tanpa baris palsu. 5NF terpenuhi.</p>

<hr>

<h2 id="bag11">11. Ringkasan &amp; Perbandingan</h2>

<h3>11.1 Tabel Perbandingan Bentuk Normal</h3>

<table>
  <thead>
    <tr><th>Bentuk Normal</th><th>Syarat Utama</th><th>Masalah yang Diselesaikan</th></tr>
  </thead>
  <tbody>
    <tr><td><strong>1NF</strong></td><td>Nilai atomik, tidak ada repeating group</td><td>Data multivalue</td></tr>
    <tr><td><strong>2NF</strong></td><td>Tidak ada ketergantungan parsial</td><td>Redundansi akibat composite key</td></tr>
    <tr><td><strong>3NF</strong></td><td>Tidak ada ketergantungan transitif</td><td>Redundansi akibat atribut non-key</td></tr>
    <tr><td><strong>BCNF</strong></td><td>Setiap determinan adalah superkey</td><td>Kasus khusus overlapping candidate keys</td></tr>
    <tr><td><strong>4NF</strong></td><td>Tidak ada MVD non-trivial</td><td>Produk kartesian dari data independen</td></tr>
    <tr><td><strong>5NF</strong></td><td>Tidak ada join dependency non-trivial</td><td>Hubungan tiga arah yang kompleks</td></tr>
  </tbody>
</table>

<h3>11.2 Denormalisasi: Kapan Melanggar Aturan?</h3>
<p>Dalam praktik nyata, terkadang kita <strong>sengaja</strong> melakukan <strong>denormalisasi</strong> (mundur dari bentuk normal tinggi) dengan pertimbangan:</p>
<ul>
  <li><strong>Performa query:</strong> Terlalu banyak JOIN memperlambat query.</li>
  <li><strong>Data warehouse / OLAP:</strong> Lebih mengutamakan kecepatan baca.</li>
  <li><strong>Caching / materialized view:</strong> Data duplikat untuk akses cepat.</li>
</ul>

<blockquote><strong>Prinsip:</strong> <em>"Normalize until it hurts, denormalize until it works."</em> — Anonim</blockquote>

<hr>

<h2 id="bag12">12. Soal Latihan</h2>

<h3>Bagian A: Soal Pemahaman Konsep (Essay)</h3>

<div class="soal">
<p><strong>Soal 1.</strong></p>
<p>Jelaskan hubungan antara ERD dan normalisasi! Mengapa hasil transformasi langsung dari ERD ke tabel relasional belum tentu sudah ternormalisasi?</p>
</div>

<div class="soal">
<p><strong>Soal 2.</strong></p>
<p>Sebutkan dan jelaskan tiga jenis anomali data beserta contoh masing-masing!</p>
</div>

<div class="soal">
<p><strong>Soal 3.</strong></p>
<p>Apa perbedaan mendasar antara 3NF dan BCNF? Berikan contoh kasus di mana sebuah tabel memenuhi 3NF tetapi tidak memenuhi BCNF!</p>
</div>

<div class="soal">
<p><strong>Soal 4.</strong></p>
<p>Jelaskan apa yang dimaksud dengan ketergantungan fungsional penuh, parsial, dan transitif! Berikan contoh untuk masing-masing jenis!</p>
</div>

<div class="soal">
<p><strong>Soal 5.</strong></p>
<p>Mengapa dalam praktik industri, normalisasi biasanya hanya dilakukan sampai 3NF atau BCNF? Jelaskan alasan Anda!</p>
</div>

<hr>

<h3>Bagian B: Soal Studi Kasus Normalisasi</h3>

<div class="soal">
<p><strong>Soal 6. (Normalisasi 1NF → 2NF → 3NF)</strong></p>
<p>Diberikan tabel <strong>PEMESANAN</strong> berikut:</p>
<table>
  <thead>
    <tr><th>No_Pesanan</th><th>Tgl_Pesanan</th><th>ID_Pelanggan</th><th>Nama_Pelanggan</th><th>Kota</th><th>ID_Produk</th><th>Nama_Produk</th><th>Harga</th><th>Qty</th></tr>
  </thead>
  <tbody>
    <tr><td>P001</td><td>2025-01-15</td><td>C01</td><td>Tono</td><td>Jakarta</td><td>PR01</td><td>Laptop</td><td>10000000</td><td>1</td></tr>
    <tr><td>P001</td><td>2025-01-15</td><td>C01</td><td>Tono</td><td>Jakarta</td><td>PR02</td><td>Mouse</td><td>150000</td><td>2</td></tr>
    <tr><td>P002</td><td>2025-01-16</td><td>C02</td><td>Dewi</td><td>Bandung</td><td>PR01</td><td>Laptop</td><td>10000000</td><td>1</td></tr>
    <tr><td>P003</td><td>2025-01-17</td><td>C01</td><td>Tono</td><td>Jakarta</td><td>PR03</td><td>Keyboard</td><td>500000</td><td>1</td></tr>
  </tbody>
</table>
<p><strong>Pertanyaan:</strong></p>
<ol type="a">
  <li>Identifikasi <em>primary key</em> dari tabel di atas!</li>
  <li>Identifikasi semua ketergantungan fungsional!</li>
  <li>Normalisasi tabel tersebut hingga <strong>3NF</strong>. Tuliskan struktur setiap tabel hasil beserta PK dan FK-nya!</li>
</ol>
</div>

<div class="soal">
<p><strong>Soal 7. (Normalisasi dengan Atribut Multivalue)</strong></p>
<p>Diberikan tabel <strong>KARYAWAN</strong>:</p>
<table>
  <thead>
    <tr><th>NIK</th><th>Nama</th><th>Keahlian</th><th>Proyek</th></tr>
  </thead>
  <tbody>
    <tr><td>K01</td><td>Budi</td><td>Java, Python</td><td>ProyekA, ProyekB</td></tr>
    <tr><td>K02</td><td>Ani</td><td>PHP</td><td>ProyekC</td></tr>
    <tr><td>K03</td><td>Rudi</td><td>Java, C++</td><td>ProyekA</td></tr>
  </tbody>
</table>
<p><strong>Pertanyaan:</strong></p>
<ol type="a">
  <li>Apakah tabel di atas sudah memenuhi 1NF? Jelaskan!</li>
  <li>Lakukan normalisasi hingga 3NF!</li>
</ol>
</div>

<div class="soal">
<p><strong>Soal 8. (BCNF)</strong></p>
<p>Diberikan tabel <strong>BIMBINGAN_SKRIPSI</strong>:</p>
<table>
  <thead>
    <tr><th>Mahasiswa</th><th>Topik</th><th>Dosen_Pembimbing</th></tr>
  </thead>
  <tbody>
    <tr><td>Andi</td><td>Machine Learning</td><td>Dr. Susi</td></tr>
    <tr><td>Andi</td><td>Web Development</td><td>Dr. Budi</td></tr>
    <tr><td>Siti</td><td>Machine Learning</td><td>Dr. Susi</td></tr>
    <tr><td>Doni</td><td>Database</td><td>Dr. Rina</td></tr>
  </tbody>
</table>
<p><strong>Aturan bisnis:</strong></p>
<ul>
  <li>Setiap dosen hanya membimbing <strong>satu topik</strong> tertentu.</li>
  <li>Satu topik bisa dibimbing oleh <strong>beberapa dosen</strong>.</li>
  <li>Seorang mahasiswa bisa memiliki <strong>beberapa topik</strong> dengan <strong>beberapa dosen</strong>.</li>
</ul>
<p><strong>Pertanyaan:</strong></p>
<ol type="a">
  <li>Identifikasi semua FD dan candidate key!</li>
  <li>Apakah tabel tersebut sudah memenuhi BCNF? Jelaskan!</li>
  <li>Jika belum, lakukan normalisasi ke BCNF!</li>
</ol>
</div>

<div class="soal">
<p><strong>Soal 9. (4NF)</strong></p>
<p>Diberikan tabel <strong>DOSEN_AKTIVITAS</strong>:</p>
<table>
  <thead>
    <tr><th>NIP</th><th>Mata_Kuliah</th><th>Kegiatan_Penelitian</th></tr>
  </thead>
  <tbody>
    <tr><td>D01</td><td>Basis Data</td><td>Data Mining</td></tr>
    <tr><td>D01</td><td>Basis Data</td><td>Machine Learning</td></tr>
    <tr><td>D01</td><td>Statistik</td><td>Data Mining</td></tr>
    <tr><td>D01</td><td>Statistik</td><td>Machine Learning</td></tr>
    <tr><td>D02</td><td>Jaringan</td><td>IoT</td></tr>
    <tr><td>D02</td><td>Keamanan</td><td>IoT</td></tr>
  </tbody>
</table>
<p><strong>Pertanyaan:</strong></p>
<ol type="a">
  <li>Identifikasi MVD yang ada!</li>
  <li>Normalisasi tabel tersebut ke 4NF!</li>
</ol>
</div>

<div class="soal">
<p><strong>Soal 10. (Soal Komprehensif — Dari ERD ke 3NF)</strong></p>
<p>Sebuah universitas ingin membangun sistem informasi akademik. Dari hasil analisis, diperoleh ERD dengan entitas dan relasi berikut:</p>
<ul>
  <li><strong>Entitas:</strong> Mahasiswa (NIM, Nama, Tgl_Lahir, Alamat, Kode_Jurusan), MataKuliah (Kode_MK, Nama_MK, SKS), Dosen (NIP, Nama_Dosen, Kode_Jurusan)</li>
  <li><strong>Relasi:</strong> Mahasiswa <em>mengambil</em> MataKuliah (dengan atribut Nilai, Semester), Dosen <em>mengajar</em> MataKuliah</li>
</ul>
<p><strong>Pertanyaan:</strong></p>
<ol type="a">
  <li>Transformasikan ERD di atas menjadi tabel-tabel relasional!</li>
  <li>Periksa apakah tabel-tabel tersebut sudah memenuhi 3NF!</li>
  <li>Jika belum, lakukan normalisasi hingga 3NF!</li>
  <li>Tunjukkan bahwa hasil normalisasi Anda bebas dari anomali insert, update, dan delete!</li>
</ol>
</div>

<hr>

<h3>Bagian C: Soal Pilihan Ganda</h3>

<div class="soal">
<p><strong>Soal 11.</strong> Sebuah tabel dikatakan memenuhi 2NF jika...</p>
<ul>
  <li>A. Semua atribut bernilai atomik</li>
  <li>B. Tidak ada ketergantungan transitif</li>
  <li>C. Tidak ada ketergantungan parsial terhadap primary key</li>
  <li>D. Setiap determinan adalah superkey</li>
  <li>E. Tidak ada MVD non-trivial</li>
</ul>
</div>

<div class="soal">
<p><strong>Soal 12.</strong> Anomali yang terjadi ketika penghapusan satu baris menyebabkan hilangnya data lain yang masih diperlukan disebut...</p>
<ul>
  <li>A. Insertion anomaly</li>
  <li>B. Update anomaly</li>
  <li>C. Deletion anomaly</li>
  <li>D. Redundancy anomaly</li>
  <li>E. Consistency anomaly</li>
</ul>
</div>

<div class="soal">
<p><strong>Soal 13.</strong> Jika <code>A → B</code> dan <code>B → C</code>, maka <code>A → C</code> disebut ketergantungan...</p>
<ul>
  <li>A. Penuh</li>
  <li>B. Parsial</li>
  <li>C. Transitif</li>
  <li>D. Multivalue</li>
  <li>E. Join</li>
</ul>
</div>

<div class="soal">
<p><strong>Soal 14.</strong> BCNF lebih ketat dari 3NF karena...</p>
<ul>
  <li>A. BCNF melarang atribut multivalue</li>
  <li>B. BCNF mengharuskan setiap determinan merupakan superkey</li>
  <li>C. BCNF melarang composite key</li>
  <li>D. BCNF mengharuskan semua tabel memiliki satu kolom saja</li>
  <li>E. BCNF melarang foreign key</li>
</ul>
</div>

<div class="soal">
<p><strong>Soal 15.</strong> Bentuk normal yang menangani ketergantungan multivalue (MVD) adalah...</p>
<ul>
  <li>A. 1NF</li>
  <li>B. 2NF</li>
  <li>C. 3NF</li>
  <li>D. BCNF</li>
  <li>E. 4NF</li>
</ul>
</div>

<hr>

<h3>Kunci Jawaban Pilihan Ganda</h3>

<table class="kunci-jawaban">
  <thead>
    <tr><th>No</th><th>Jawaban</th></tr>
  </thead>
  <tbody>
    <tr><td>11</td><td>C</td></tr>
    <tr><td>12</td><td>C</td></tr>
    <tr><td>13</td><td>C</td></tr>
    <tr><td>14</td><td>B</td></tr>
    <tr><td>15</td><td>E</td></tr>
  </tbody>
</table>

<hr>

<h2>REFERENSI</h2>
<ol>
  <li>Codd, E.F. (1970). <em>A Relational Model of Data for Large Shared Data Banks</em>. Communications of the ACM.</li>
  <li>Elmasri, R. &amp; Navathe, S.B. (2016). <em>Fundamentals of Database Systems</em>, 7th Edition. Pearson.</li>
  <li>Silberschatz, A., Korth, H.F., &amp; Sudarshan, S. (2020). <em>Database System Concepts</em>, 7th Edition. McGraw-Hill.</li>
  <li>Connolly, T. &amp; Begg, C. (2015). <em>Database Systems: A Practical Approach to Design, Implementation, and Management</em>, 6th Edition. Pearson.</li>
</ol>

<hr>

<div class="note">
<strong>Catatan untuk mahasiswa:</strong> Kerjakan semua soal latihan di atas dan kumpulkan pada pertemuan berikutnya. Soal Bagian B (studi kasus) akan didiskusikan bersama di kelas. Pastikan Anda memahami <strong>proses</strong> normalisasi, bukan hanya menghafal definisi. Selamat belajar! 🎓
</div>

</body>
</html>
