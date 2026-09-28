---
title: Normalisasi
permalink: /normalisasi/
---

# MODUL MATA KULIAH BASIS DATA
## Topik: Normalisasi Basis Data

---

## Daftar Isi

1. [Pengantar & Hubungan dengan ERD](#1-pengantar--hubungan-dengan-erd)
2. [Konsep Dasar Normalisasi](#2-konsep-dasar-normalisasi)
3. [Anomali Data](#3-anomali-data)
4. [Ketergantungan Fungsional](#4-ketergantungan-fungsional-functional-dependency)
5. [Bentuk Normal Pertama (1NF)](#5-bentuk-normal-pertama-1nf)
6. [Bentuk Normal Kedua (2NF)](#6-bentuk-normal-kedua-2nf)
7. [Bentuk Normal Ketiga (3NF)](#7-bentuk-normal-ketiga-3nf)
8. [Bentuk Normal Boyce-Codd (BCNF)](#8-bentuk-normal-boyce-codd-bcnf)
9. [Bentuk Normal Keempat (4NF)](#9-bentuk-normal-keempat-4nf)
10. [Bentuk Normal Kelima (5NF)](#10-bentuk-normal-kelima-5nf)
11. [Ringkasan & Perbandingan](#11-ringkasan--perbandingan)

---

## 1. Pengantar & Hubungan dengan ERD

### 1.1 Kilas Balik: Entity-Relationship Diagram (ERD)

Pada pertemuan-pertemuan sebelumnya, kita telah mempelajari **Entity-Relationship Diagram (ERD)** sebagai alat pemodelan data secara **konseptual**. ERD membantu kita mengidentifikasi:

- **Entitas** (objek-objek utama dalam sistem, misalnya: Mahasiswa, Mata Kuliah, Dosen)
- **Atribut** (karakteristik dari setiap entitas, misalnya: NIM, Nama, Alamat)
- **Relasi** (hubungan antar entitas, misalnya: Mahasiswa *mengambil* Mata Kuliah)
- **Kardinalitas** (1:1, 1:N, M:N)

### 1.2 Dari ERD ke Tabel Relasional

Setelah ERD selesai dirancang, langkah selanjutnya adalah **mentransformasikan** model konseptual (ERD) menjadi **model logis**, yaitu sekumpulan **tabel relasional**. Proses transformasi umum meliputi:

| Komponen ERD | Hasil Transformasi |
|---|---|
| Entitas kuat | Tabel terpisah |
| Entitas lemah | Tabel dengan *foreign key* ke entitas induk |
| Atribut sederhana | Kolom dalam tabel |
| Atribut multivalue | Tabel terpisah |
| Relasi 1:1 | *Foreign key* di salah satu tabel |
| Relasi 1:N | *Foreign key* di sisi "N" |
| Relasi M:N | Tabel baru (tabel junction/associative) |

### 1.3 Mengapa ERD Saja Tidak Cukup?

Meskipun ERD sudah memberikan gambaran struktur data, **hasil transformasi langsung dari ERD ke tabel belum tentu optimal**. Sering kali kita mendapatkan tabel-tabel yang:

- Mengandung **redundansi** (data berulang)
- Rentan terhadap **anomali** (kesalahan saat insert, update, delete)
- Memiliki **atribut yang tidak bergantung penuh** pada kunci utama
- Menyimpan **data campuran** yang seharusnya dipisah

> **Inilah alasan mengapa kita memerlukan NORMALISASI.**

### 1.4 Posisi Normalisasi dalam Siklus Pengembangan Basis Data

```
┌─────────────────────────────────────────────────────────┐
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
│  Testing & Deployment                                   │
└─────────────────────────────────────────────────────────┘
```

**Normalisasi** adalah proses **evaluasi dan perbaikan** struktur tabel hasil transformasi ERD agar memenuhi standar kualitas tertentu yang disebut **bentuk normal (normal form)**.

---

## 2. Konsep Dasar Normalisasi

### 2.1 Definisi

> **Normalisasi** adalah proses pengorganisasian data dalam basis data dengan tujuan:
> 1. **Mengeliminasi redundansi** (pengulangan data yang tidak perlu)
> 2. **Mengeliminasi anomali** (ketidakkonsistenan data saat operasi insert, update, delete)
> 3. **Memastikan ketergantungan data** masuk akal (data disimpan di tempat yang tepat)

### 2.2 Sejarah Singkat

Konsep normalisasi pertama kali diperkenalkan oleh **Edgar F. Codd** pada tahun **1970** dalam makalahnya yang revolusioner *"A Relational Model of Data for Large Shared Data Banks"*. Codd mendefinisikan **Bentuk Normal Pertama (1NF)**. Selanjutnya, Codd bersama **Raymond F. Boyce** mengembangkan **BCNF** pada tahun 1974. Bentuk normal yang lebih tinggi (4NF, 5NF) dikembangkan oleh **Ronald Fagin** pada tahun 1977 dan 1979.

### 2.3 Prinsip Utama

Normalisasi bekerja dengan prinsip **dekomposisi** (pemecahan):

- Satu tabel besar yang bermasalah dipecah menjadi **beberapa tabel kecil** yang lebih terstruktur.
- Tabel-tabel kecil tersebut tetap dapat **dihubungkan kembali** melalui *primary key* dan *foreign key* (sifat **lossless join**).

### 2.4 Tingkatan Bentuk Normal

```
  1NF  →  2NF  →  3NF  →  BCNF  →  4NF  →  5NF
  ──────────────────────────────────────────────►
  Semakin tinggi tingkat normalisasi,
  semakin ketat aturannya,
  semakin sedikit redundansi.
```

> **Catatan penting:** Dalam praktik industri, normalisasi hingga **3NF atau BCNF** umumnya sudah dianggap **cukup**. Bentuk normal 4NF dan 5NF lebih bersifat akademis dan digunakan pada kasus-kasus khusus.

---

## 3. Anomali Data

Sebelum masuk ke bentuk-bentuk normal, kita perlu memahami **mengapa** normalisasi diperlukan. Jawabannya terletak pada **anomali data**.

### 3.1 Contoh Tabel yang Belum Dinormalisasi

Perhatikan tabel **KULIAH_MAHASISWA** berikut yang merupakan hasil transformasi langsung dari ERD tanpa normalisasi:

| NIM | Nama_Mhs | Alamat | Kode_MK | Nama_MK | SKS | NIP_Dosen | Nama_Dosen | Nilai |
|---|---|---|---|---|---|---|---|---|
| 101 | Andi | Jakarta | MK01 | Basis Data | 3 | D01 | Prof. Susi | A |
| 101 | Andi | Jakarta | MK02 | Pemrograman | 4 | D02 | Dr. Budi | B |
| 102 | Siti | Bandung | MK01 | Basis Data | 3 | D01 | Prof. Susi | A |
| 102 | Siti | Bandung | MK03 | Jaringan | 3 | D03 | Dr. Rina | C |
| 103 | Doni | Surabaya | MK02 | Pemrograman | 4 | D02 | Dr. Budi | A |

### 3.2 Tiga Jenis Anomali

#### a) Anomali Penyisipan (*Insertion Anomaly*)

**Masalah:** Kita **tidak bisa** menambahkan data baru tanpa data lain yang menyertainya.

> **Contoh:** Kita ingin menambahkan mata kuliah baru "Kecerdasan Buatan" (MK04, 3 SKS) yang diajarkan oleh Dr. Rina. Namun, karena belum ada mahasiswa yang mengambil mata kuliah tersebut, kita **tidak bisa** menyisipkan baris baru karena kolom NIM (sebagai bagian dari *primary key*) tidak boleh NULL.

#### b) Anomali Pembaruan (*Update Anomaly*)

**Masalah:** Perubahan satu data mengharuskan kita mengubah **banyak baris** secara konsisten.

> **Contoh:** Jika Prof. Susi pindah alamat kantor dan namanya berubah menjadi "Prof. Susi Hartono", kita harus mengubah **semua baris** yang mengandung NIP_Dosen = D01 (baris 1 dan 3). Jika kita lupa mengubah salah satu, data menjadi **tidak konsisten**.

#### c) Anomali Penghapusan (*Deletion Anomaly*)

**Masalah:** Menghapus satu data menyebabkan **hilangnya data lain** yang sebenarnya masih diperlukan.

> **Contoh:** Jika mahasiswa Doni (NIM 103) keluar dari universitas dan kita menghapus barisnya, maka informasi bahwa mata kuliah "Pemrograman" diajarkan oleh Dr. Budi juga **ikut hilang** (karena hanya Doni satu-satunya yang mengambil MK02 di data saat ini).

---

## 4. Ketergantungan Fungsional (*Functional Dependency*)

### 4.1 Definisi

> **Ketergantungan Fungsional (FD)** adalah hubungan antara dua himpunan atribut dalam sebuah relasi, di mana nilai dari satu himpunan atribut **menentukan secara unik** nilai dari himpunan atribut lainnya.

**Notasi:** `X → Y` dibaca "X menentukan Y secara fungsional" atau "Y bergantung secara fungsional pada X".

Artinya: Untuk setiap pasangan baris dalam tabel, jika nilai X sama, maka nilai Y **pasti** sama.

### 4.2 Contoh Ketergantungan Fungsional

Dari tabel KULIAH_MAHASISWA di atas, kita dapat mengidentifikasi FD berikut:

| No | FD | Penjelasan |
|---|---|---|
| 1 | `NIM → Nama_Mhs, Alamat` | Setiap NIM menentukan satu nama dan alamat mahasiswa |
| 2 | `Kode_MK → Nama_MK, SKS` | Setiap kode MK menentukan nama dan SKS mata kuliah |
| 3 | `NIP_Dosen → Nama_Dosen` | Setiap NIP menentukan satu nama dosen |
| 4 | `NIM, Kode_MK → Nilai` | Kombinasi NIM dan Kode MK menentukan nilai |
| 5 | `Kode_MK → NIP_Dosen, Nama_Dosen` | Setiap MK diajarkan oleh satu dosen tertentu |

### 4.3 Jenis-Jenis Ketergantungan Fungsional

#### a) Ketergantungan Fungsional Penuh (*Full Functional Dependency*)

`X → Y` adalah ketergantungan penuh jika Y bergantung pada **seluruh** atribut dalam X, bukan hanya sebagian.

> **Contoh:** `{NIM, Kode_MK} → Nilai` adalah ketergantungan **penuh** karena Nilai tidak bisa ditentukan hanya oleh NIM saja atau Kode_MK saja.

#### b) Ketergantungan Fungsional Parsial (*Partial Functional Dependency*)

`X → Y` adalah ketergantungan parsial jika Y hanya bergantung pada **sebagian** dari X.

> **Contoh:** `{NIM, Kode_MK} → Nama_Mhs` adalah ketergantungan **parsial** karena Nama_Mhs hanya bergantung pada NIM saja, tidak perlu Kode_MK.

#### c) Ketergantungan Fungsional Transitif (*Transitive Functional Dependency*)

`X → Z` adalah ketergantungan transitif jika terdapat atribut Y sehingga `X → Y` dan `Y → Z`, di mana Y **bukan** bagian dari X dan X **tidak** bergantung pada Y.

> **Contoh:** `NIM → Jurusan` dan `Jurusan → Dekan`, maka `NIM → Dekan` adalah ketergantungan **transitif** melalui Jurusan.

---

## 5. Bentuk Normal Pertama (1NF)

### 5.1 Syarat 1NF

Sebuah tabel memenuhi **1NF** jika:

- Setiap kolom berisi **nilai atomik** (tidak dapat dipecah lagi)
- **Tidak ada** kelompok atribut yang berulang (*repeating groups*)
- Setiap baris bersifat **unik** (terdapat *primary key*)
- Setiap kolom memiliki **satu tipe data** yang konsisten

### 5.2 Contoh Pelanggaran 1NF

Perhatikan tabel **MAHASISWA** berikut:

| NIM | Nama | Telepon | Mata_Kuliah |
|---|---|---|---|
| 101 | Andi | 081111, 082222 | Basis Data, Pemrograman |
| 102 | Siti | 083333 | Jaringan |
| 103 | Doni | 084444, 085555, 086666 | Basis Data, Jaringan, AI |

**Pelanggaran:**
- Kolom **Telepon** berisi banyak nilai (multivalue) → tidak atomik
- Kolom **Mata_Kuliah** berisi banyak nilai → *repeating group*

### 5.3 Proses Normalisasi ke 1NF

**Langkah 1:** Pisahkan nilai-nilai multivalue menjadi baris-baris terpisah.

**Hasil 1NF:**

| NIM | Nama | Telepon | Mata_Kuliah |
|---|---|---|---|
| 101 | Andi | 081111 | Basis Data |
| 101 | Andi | 082222 | Basis Data |
| 101 | Andi | 081111 | Pemrograman |
| 101 | Andi | 082222 | Pemrograman |
| 102 | Siti | 083333 | Jaringan |
| 103 | Doni | 084444 | Basis Data |
| 103 | Doni | 085555 | Basis Data |
| 103 | Doni | 086666 | Basis Data |
| 103 | Doni | 084444 | Jaringan |
| 103 | Doni | 085555 | Jaringan |
| 103 | Doni | 086666 | Jaringan |

**Primary Key:** `{NIM, Telepon, Mata_Kuliah}` (kombinasi tiga atribut)

> **Perhatikan:** Tabel sudah memenuhi 1NF, tetapi **redundansi sangat tinggi**! Data nama "Andi" diulang 4 kali. Ini akan ditangani di 2NF.

---

## 6. Bentuk Normal Kedua (2NF)

### 6.1 Syarat 2NF

Sebuah tabel memenuhi **2NF** jika:

- Sudah memenuhi **1NF**
- **Tidak ada** ketergantungan fungsional **parsial** terhadap *primary key*
  - Artinya: Setiap atribut non-key harus bergantung **penuh** pada **seluruh** *primary key*, bukan hanya sebagian.

> **Catatan:** Jika *primary key* hanya terdiri dari **satu atribut**, maka tabel **otomatis** memenuhi 2NF (karena tidak mungkin ada ketergantungan parsial).

### 6.2 Contoh Pelanggaran 2NF

Gunakan tabel hasil 1NF di atas dengan **Primary Key = {NIM, Telepon, Mata_Kuliah}**.

Ketergantungan fungsional yang teridentifikasi:
- `NIM → Nama` ← **PARSIAL!** (Nama hanya bergantung pada NIM, bukan pada keseluruhan PK)
- `NIM → Telepon` ← PARSIAL

### 6.3 Proses Normalisasi ke 2NF

**Langkah:** Pisahkan atribut-atribut yang bergantung parsial ke tabel tersendiri.

**Tabel 1: MAHASISWA_TELEPON**

| NIM | Nama | Telepon |
|---|---|---|
| 101 | Andi | 081111 |
| 101 | Andi | 082222 |
| 102 | Siti | 083333 |
| 103 | Doni | 084444 |
| 103 | Doni | 085555 |
| 103 | Doni | 086666 |

**PK:** `{NIM, Telepon}`

**Tabel 2: MAHASISWA_MK**

| NIM | Mata_Kuliah |
|---|---|
| 101 | Basis Data |
| 101 | Pemrograman |
| 102 | Jaringan |
| 103 | Basis Data |
| 103 | Jaringan |

**PK:** `{NIM, Mata_Kuliah}`

Sekarang tidak ada lagi ketergantungan parsial. Setiap atribut non-key bergantung penuh pada PK-nya masing-masing.

---

## 7. Bentuk Normal Ketiga (3NF)

### 7.1 Syarat 3NF

Sebuah tabel memenuhi **3NF** jika:

- Sudah memenuhi **2NF**
- **Tidak ada** ketergantungan fungsional **transitif** terhadap *primary key*
  - Artinya: Setiap atribut non-key harus bergantung **langsung** pada *primary key*, **bukan** melalui atribut non-key lainnya.

> **Mnemonic populer:** *"Every non-key attribute must provide a fact about the key, the whole key, and nothing but the key."* — Bill Kent

### 7.2 Contoh Pelanggaran 3NF

Perhatikan tabel **MAHASISWA_JURUSAN** berikut (sudah 2NF):

| NIM | Nama | Kode_Jurusan | Nama_Jurusan | Dekan |
|---|---|---|---|---|
| 101 | Andi | J01 | Teknik Informatika | Dr. Ahmad |
| 102 | Siti | J02 | Sistem Informasi | Dr. Lina |
| 103 | Doni | J01 | Teknik Informatika | Dr. Ahmad |
| 104 | Rina | J03 | Ilmu Komputer | Dr. Hadi |

**Primary Key:** `NIM`

**Ketergantungan Fungsional:**
- `NIM → Nama, Kode_Jurusan` (langsung ke PK)
- `Kode_Jurusan → Nama_Jurusan, Dekan` (transitif!)
- `NIM → Nama_Jurusan, Dekan` (melalui Kode_Jurusan → **TRANSITIF**)

**Masalah:**
- **Anomali Update:** Jika Dekan Teknik Informatika berganti, kita harus update semua baris dengan Kode_Jurusan = J01.
- **Anomali Insert:** Tidak bisa menambahkan jurusan baru tanpa ada mahasiswa.
- **Anomali Delete:** Jika semua mahasiswa TI dihapus, info jurusan TI ikut hilang.

### 7.3 Proses Normalisasi ke 3NF

**Langkah:** Pisahkan atribut yang bergantung transitif ke tabel tersendiri.

**Tabel 1: MAHASISWA**

| NIM | Nama | Kode_Jurusan |
|---|---|---|
| 101 | Andi | J01 |
| 102 | Siti | J02 |
| 103 | Doni | J01 |
| 104 | Rina | J03 |

**PK:** `NIM` | **FK:** `Kode_Jurusan → JURUSAN(Kode_Jurusan)`

**Tabel 2: JURUSAN**

| Kode_Jurusan | Nama_Jurusan | Dekan |
|---|---|---|
| J01 | Teknik Informatika | Dr. Ahmad |
| J02 | Sistem Informasi | Dr. Lina |
| J03 | Ilmu Komputer | Dr. Hadi |

**PK:** `Kode_Jurusan`

Sekarang semua atribut non-key bergantung **langsung** pada PK-nya masing-masing. Tidak ada lagi ketergantungan transitif.

---

## 8. Bentuk Normal Boyce-Codd (BCNF)

### 8.1 Syarat BCNF

Sebuah tabel memenuhi **BCNF** jika:

- Sudah memenuhi **3NF**
- Untuk **setiap** ketergantungan fungsional `X → Y` yang non-trivial (Y ⊄ X), **X harus merupakan *superkey***.

> **Perbedaan dengan 3NF:** 3NF masih mengizinkan ketergantungan `X → Y` di mana X bukan superkey **asalkan** Y adalah bagian dari *candidate key*. BCNF **tidak** mengizinkan pengecualian ini. BCNF lebih ketat dari 3NF.

### 8.2 Kapan 3NF ≠ BCNF?

3NF dan BCNF **selalu sama** kecuali dalam kondisi khusus:
- Tabel memiliki **lebih dari satu *candidate key***
- *Candidate key* tersebut **beririsan** (overlapping)
- Terdapat FD dari atribut non-key ke sebagian *candidate key*

### 8.3 Contoh Kasus BCNF

Perhatikan tabel **JADWAL_KULIAH**:

| Mahasiswa | Mata_Kuliah | Dosen |
|---|---|---|
| Andi | Basis Data | Prof. Susi |
| Andi | Pemrograman | Dr. Budi |
| Siti | Basis Data | Prof. Susi |
| Doni | Pemrograman | Dr. Budi |
| Doni | Basis Data | Dr. Rina |

**Aturan bisnis:**
1. Setiap mahasiswa mengambil beberapa mata kuliah dari beberapa dosen.
2. Setiap dosen hanya mengajar **satu** mata kuliah tertentu.
3. Setiap mata kuliah bisa diajar oleh **beberapa** dosen.

**Ketergantungan Fungsional:**
- `{Mahasiswa, Mata_Kuliah} → Dosen` (mahasiswa + MK menentukan dosen)
- `Dosen → Mata_Kuliah` (setiap dosen hanya mengajar 1 MK)

**Candidate Keys:**
- `{Mahasiswa, Mata_Kuliah}` 
- `{Mahasiswa, Dosen}` (karena Dosen → Mata_Kuliah)

**Analisis:**
- FD `Dosen → Mata_Kuliah`: Dosen **bukan** superkey, tetapi Mata_Kuliah adalah bagian dari candidate key → **3NF terpenuhi** (pengecualian 3NF), tetapi **BCNF TIDAK terpenuhi**.

### 8.4 Proses Normalisasi ke BCNF

**Tabel 1: DOSEN_MK**

| Dosen | Mata_Kuliah |
|---|---|
| Prof. Susi | Basis Data |
| Dr. Budi | Pemrograman |
| Dr. Rina | Basis Data |

**PK:** `Dosen`

**Tabel 2: MAHASISWA_DOSEN**

| Mahasiswa | Dosen |
|---|---|
| Andi | Prof. Susi |
| Andi | Dr. Budi |
| Siti | Prof. Susi |
| Doni | Dr. Budi |
| Doni | Dr. Rina |

**PK:** `{Mahasiswa, Dosen}`

Sekarang setiap FD memiliki determinan yang merupakan superkey. BCNF terpenuhi.

---

## 9. Bentuk Normal Keempat (4NF)

### 9.1 Konsep Ketergantungan Multivalue (*Multivalued Dependency - MVD*)

Sebelum memahami 4NF, kita perlu mengenal **MVD**.

> **MVD** `X →→ Y` terjadi ketika untuk setiap nilai X, terdapat **himpunan nilai Y** yang **independen** dari himpunan nilai atribut lainnya (Z).

### 9.2 Syarat 4NF

Sebuah tabel memenuhi **4NF** jika:

- Sudah memenuhi **BCNF**
- **Tidak ada** ketergantungan multivalue yang non-trivial

### 9.3 Contoh Pelanggaran 4NF

Tabel **DOSEN_KEAHLIAN**:

| NIP_Dosen | Keahlian | Sertifikasi |
|---|---|---|
| D01 | Database | Oracle Certified |
| D01 | Database | AWS Certified |
| D01 | AI | TensorFlow Cert |
| D02 | Jaringan | CCNA |
| D02 | Keamanan | CEH |
| D02 | Keamanan | CompTIA Security+ |

**MVD yang ada:**
- `NIP_Dosen →→ Keahlian` (himpunan keahlian independen dari sertifikasi)
- `NIP_Dosen →→ Sertifikasi` (himpunan sertifikasi independen dari keahlian)

**Masalah:** Tabel ini menghasilkan **produk kartesian** yang menghasilkan baris-baris redundan dan bisa menimbulkan data yang salah jika tidak dikelola dengan hati-hati.

### 9.4 Proses Normalisasi ke 4NF

**Tabel 1: DOSEN_KEAHLIAN**

| NIP_Dosen | Keahlian |
|---|---|
| D01 | Database |
| D01 | AI |
| D02 | Jaringan |
| D02 | Keamanan |

**Tabel 2: DOSEN_SERTIFIKASI**

| NIP_Dosen | Sertifikasi |
|---|---|
| D01 | Oracle Certified |
| D01 | AWS Certified |
| D01 | TensorFlow Cert |
| D02 | CCNA |
| D02 | CEH |
| D02 | CompTIA Security+ |

MVD sudah dihilangkan. 4NF terpenuhi.

---

## 10. Bentuk Normal Kelima (5NF)

### 10.1 Konsep *Join Dependency*

5NF berkaitan dengan **ketergantungan join** (*join dependency*). Sebuah relasi memenuhi 5NF jika tidak dapat didekomposisi lagi menjadi relasi-relasi yang lebih kecil tanpa kehilangan informasi (*lossless join*).

### 10.2 Syarat 5NF

Sebuah tabel memenuhi **5NF** (juga disebut *Project-Join Normal Form / PJNF*) jika:

- Sudah memenuhi **4NF**
- Setiap *join dependency* adalah **implikasi** dari *candidate key*

### 10.3 Contoh Kasus 5NF (Kasus Klasik)

Tabel **PROYEK_KONSULTAN** yang mencatat hubungan tiga arah:

| Proyek | Konsultan | Keahlian |
|---|---|---|
| ProyekA | Andi | Database |
| ProyekA | Andi | AI |
| ProyekA | Siti | Database |
| ProyekB | Andi | Database |
| ProyekB | Doni | AI |

**Aturan bisnis:**
- Jika Andi bekerja di ProyekA dan Andi memiliki keahlian Database, dan ProyekA membutuhkan keahlian Database, maka Andi **harus** ditugaskan sebagai konsultan Database di ProyekA.

Tabel ini **bisa** didekomposisi menjadi **tiga** tabel:

**Tabel 1: PROYEK_KONSULTAN**

| Proyek | Konsultan |
|---|---|
| ProyekA | Andi |
| ProyekA | Siti |
| ProyekB | Andi |
| ProyekB | Doni |

**Tabel 2: KONSULTAN_KEAHLIAN**

| Konsultan | Keahlian |
|---|---|
| Andi | Database |
| Andi | AI |
| Siti | Database |
| Doni | AI |

**Tabel 3: PROYEK_KEAHLIAN**

| Proyek | Keahlian |
|---|---|
| ProyekA | Database |
| ProyekA | AI |
| ProyekB | Database |
| ProyekB | AI |

Dengan melakukan *natural join* ketiga tabel ini, kita mendapatkan kembali data asli tanpa baris palsu. 5NF terpenuhi.

---

## 11. Ringkasan & Perbandingan

### 11.1 Tabel Perbandingan Bentuk Normal

| Bentuk Normal | Syarat Utama | Masalah yang Diselesaikan |
|---|---|---|
| **1NF** | Nilai atomik, tidak ada repeating group | Data multivalue |
| **2NF** | Tidak ada ketergantungan parsial | Redundansi akibat composite key |
| **3NF** | Tidak ada ketergantungan transitif | Redundansi akibat atribut non-key |
| **BCNF** | Setiap determinan adalah superkey | Kasus khusus overlapping candidate keys |
| **4NF** | Tidak ada MVD non-trivial | Produk kartesian dari data independen |
| **5NF** | Tidak ada join dependency non-trivial | Hubungan tiga arah yang kompleks |

### 11.2 Denormalisasi: Kapan Melanggar Aturan?

Dalam praktik nyata, terkadang kita **sengaja** melakukan **denormalisasi** (mundur dari bentuk normal tinggi) dengan pertimbangan:

- **Performa query:** Terlalu banyak JOIN memperlambat query.
- **Data warehouse / OLAP:** Lebih mengutamakan kecepatan baca.
- **Caching / materialized view:** Data duplikat untuk akses cepat.

> **Prinsip:** *"Normalize until it hurts, denormalize until it works."* — Anonim

---

## Referensi

1. Codd, E.F. (1970). *A Relational Model of Data for Large Shared Data Banks*. Communications of the ACM.
2. Elmasri, R. & Navathe, S.B. (2016). *Fundamentals of Database Systems*, 7th Edition. Pearson.
3. Silberschatz, A., Korth, H.F., & Sudarshan, S. (2020). *Database System Concepts*, 7th Edition. McGraw-Hill.
4. Connolly, T. & Begg, C. (2015). *Database Systems: A Practical Approach to Design, Implementation, and Management*, 6th Edition. Pearson.

---

> **Catatan untuk mahasiswa:** Pastikan Anda memahami **proses** normalisasi, bukan hanya menghafal definisi. Selamat belajar!

---
