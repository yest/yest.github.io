# 📚 MODUL MATA KULIAH BASIS DATA
## Topik: Normalisasi Basis Data

---

## 📑 Daftar Isi

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

- ❌ Mengandung **redundansi** (data berulang)
- ❌ Rentan terhadap **anomali** (kesalahan saat insert, update, delete)
- ❌ Memiliki **atribut yang tidak bergantung penuh** pada kunci utama
- ❌ Menyimpan **data campuran** yang seharusnya dipisah

> 💡 **Inilah alasan mengapa kita memerlukan NORMALISASI.**

### 1.4 Posisi Normalisasi dalam Siklus Pengembangan Basis Data
