# 📘 Materi Kuliah: Backup & Recovery

---

## 🎯 **Tujuan Pembelajaran**

Setelah pertemuan ini mahasiswa mampu:

1. Menjelaskan konsep backup & recovery dalam basis data.
2. Menyebutkan jenis-jenis backup dan strategi recovery.
3. Melakukan backup dan recovery sederhana dengan PostgreSQL (`pg_dump`, `pg_restore`).
4. Memahami dasar **Point-in-Time Recovery (PITR)** dan log-based recovery.

---

## 🕒 **Alokasi Waktu**

* **Pendahuluan & Motivasi** → 15 menit
* **Materi Teori** → 45 menit
* **Praktik Backup & Recovery** → 60 menit
* **Latihan Mahasiswa & Diskusi** → 30 menit
* **Kesimpulan & Penutup** → 15 menit

---

## 1. **Pendahuluan (15 menit)**

* Tanya jawab: *"Kalau data kampus hilang (nilai, data mahasiswa), apa yang terjadi?"*
* Motivasi: Data adalah **aset berharga**, kehilangan data = kerugian besar.
* Peran **DBA**: menjaga ketersediaan, keamanan, dan integritas data → salah satunya dengan **backup & recovery**.

---

## 2. **Materi Teori (45 menit)**

### 2.1 Konsep Backup

* **Backup** = salinan database untuk pemulihan saat ada masalah.
* **Jenis backup**:

  1. **Full Backup** → seluruh database (paling lengkap, tapi besar).
  2. **Incremental Backup** → hanya perubahan sejak backup terakhir (hemat ruang).
  3. **Differential Backup** → perubahan sejak backup full terakhir (kompromi).

### 2.2 Konsep Recovery

* **Recovery** = proses mengembalikan database dari backup.
* **Strategi utama**:

  * **Point-in-Time Recovery (PITR):** kembalikan ke waktu tertentu.
  * **Log-based Recovery:** gunakan transaction log (WAL/binlog/redo log).

### 2.3 Alat Bantu

* **PostgreSQL:** `pg_dump`, `pg_restore`, `pg_basebackup`, WAL archiving.
* **MySQL:** `mysqldump`, binlog.
* **Oracle:** **RMAN (Recovery Manager)**.

---

## 3. **Praktik Backup & Recovery (60 menit)**

### 3.1 Setup Awal

1. Buat database `kampusdb`:

```sql
CREATE DATABASE kampusdb;
\c kampusdb;

CREATE TABLE mahasiswa (
  nim VARCHAR(10) PRIMARY KEY,
  nama VARCHAR(100),
  jurusan VARCHAR(50)
);

INSERT INTO mahasiswa VALUES
('123', 'Budi', 'Informatika'),
('124', 'Ani', 'Sistem Informasi');
```

---

### 3.2 Full Backup dengan `pg_dump`

```bash
pg_dump -U postgres kampusdb > /backup/kampusdb_full.sql
```

📌 File `kampusdb_full.sql` berisi semua struktur & data database.

---

### 3.3 Recovery dengan `psql`

1. Drop database:

```sql
DROP DATABASE kampusdb;
```

2. Restore database:

```bash
createdb -U postgres kampusdb
psql -U postgres -d kampusdb < /backup/kampusdb_full.sql
```

📌 Database kembali seperti semula.

---

### 3.4 Simulasi Kehilangan Data + Recovery

1. Hapus data:

```sql
DELETE FROM mahasiswa WHERE nim='123';
```

2. Cek → data Budi hilang.
3. Lakukan restore dari backup → data kembali.

---

### 3.5 Pengenalan PITR (Point-in-Time Recovery)

* Aktifkan `archive_mode` di PostgreSQL.
* Backup menggunakan `pg_basebackup`.
* Simpan WAL logs.
* Konfigurasi `recovery_target_time` untuk mengembalikan data sebelum kesalahan.

(👉 Pada kelas, bisa dijelaskan konsep dulu. Praktik PITR biasanya butuh setup lebih panjang, bisa dijadikan **tugas**).

---

## 4. **Latihan Mahasiswa (30 menit)**

🔹 **Instruksi:**

1. Buat tabel `nilai (nim, matkul, nilai)`.
2. Masukkan 5 data dummy.
3. Lakukan backup dengan `pg_dump`.
4. Hapus tabel `nilai`.
5. Lakukan recovery dari backup.

🔹 **Target:**

* Mahasiswa membuktikan bahwa backup & recovery berhasil.

---

## 5. **Diskusi & Refleksi (15 menit)**

* Apa perbedaan **full, incremental, differential backup**?
* Mengapa backup saja tidak cukup → harus diuji dengan recovery?
* Bagaimana strategi backup yang baik untuk **kampus/instansi**?

---

## 6. **Kesimpulan**

* Backup & recovery adalah **nyawa database**.
* Jenis backup: Full, Incremental, Differential.
* Strategi recovery: PITR & log-based.
* Tool utama: `pg_dump`, `pg_restore`, WAL, RMAN.
* **Tanpa backup → DBA = gagal. Tanpa recovery → backup = sia-sia.**

---

📌 **Tugas Rumah:**

* Pelajari **Point-in-Time Recovery (PITR)** lebih lanjut.
* Buat laporan singkat: skenario kasus *"nilai salah update, kembalikan data dengan PITR"*.

---
