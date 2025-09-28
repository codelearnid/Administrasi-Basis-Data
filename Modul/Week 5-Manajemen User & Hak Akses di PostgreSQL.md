# 📘 Materi Pertemuan: **Manajemen User & Hak Akses dalam Database**

## 🎯 **Capaian Pembelajaran**

Setelah mengikuti pertemuan ini, mahasiswa mampu:

1. Membuat dan mengelola akun pengguna di PostgreSQL.
2. Menjelaskan pentingnya manajemen user dan hak akses pada DBMS.
3. Membuat dan mengelola akun pengguna.
4. Memberikan privilege sesuai kebutuhan (SELECT, INSERT, UPDATE, DELETE, EXECUTE).
5. Menggunakan **roles** untuk mengelompokkan hak akses.
6. Menerapkan prinsip **least privilege** agar keamanan database tetap terjaga.

---

## 🧩 **Pendahuluan**

* Database sering digunakan oleh banyak pihak: admin, dosen, mahasiswa, staf, dll.
* Tidak semua user boleh melakukan hal yang sama → **perlu pembatasan hak akses**.
* Tujuan manajemen user & hak akses:

  * **Keamanan data** (data tidak sembarangan diubah/dihapus).
  * **Kontrol akses** (siapa boleh melakukan apa).
  * **Audit & akuntabilitas** (setiap user bertanggung jawab atas tindakannya).

---


## 🧩 1. Persiapan

* DBMS: **PostgreSQL** (≥ v13).
* Database: `db_kuliah`.
* Tools: **psql** atau **pgAdmin**.

Buat database:

```sql
CREATE DATABASE db_kuliah;
\c db_kuliah;
```

---

## 🧩 2. Skema Database

Kita gunakan skema sederhana sistem perkuliahan:

```
mahasiswa(nim, nama, jurusan, angkatan)
dosen(nidn, nama, prodi)
matakuliah(kode_mk, nama_mk, sks)
perkuliahan(id, nim, kode_mk, nidn, nilai)
```

### SQL: Buat Tabel

```sql
-- Tabel Mahasiswa
CREATE TABLE mahasiswa (
    nim VARCHAR(10) PRIMARY KEY,
    nama VARCHAR(100),
    jurusan VARCHAR(50),
    angkatan INT
);

-- Tabel Dosen
CREATE TABLE dosen (
    nidn VARCHAR(10) PRIMARY KEY,
    nama VARCHAR(100),
    prodi VARCHAR(50)
);

-- Tabel Matakuliah
CREATE TABLE matakuliah (
    kode_mk VARCHAR(10) PRIMARY KEY,
    nama_mk VARCHAR(100),
    sks INT
);

-- Tabel Perkuliahan
CREATE TABLE perkuliahan (
    id SERIAL PRIMARY KEY,
    nim VARCHAR(10) REFERENCES mahasiswa(nim),
    kode_mk VARCHAR(10) REFERENCES matakuliah(kode_mk),
    nidn VARCHAR(10) REFERENCES dosen(nidn),
    nilai INT
);
```

---

## 🧩 3. Membuat User

Kita buat 3 jenis user sesuai kebutuhan:

```sql
-- User mahasiswa
CREATE USER mahasiswa WITH PASSWORD 'mhs123';

-- User dosen
CREATE USER dosen WITH PASSWORD 'dsn123';

-- User admin akademik
CREATE USER adminakademik WITH PASSWORD 'adm123';
```

---

## 🧩 4. Memberikan Privilege

### 4.1 User Mahasiswa (❌ Tidak boleh ubah data, hanya lihat nilai & matakuliah)

```sql
GRANT CONNECT ON DATABASE db_kuliah TO mahasiswa;
GRANT USAGE ON SCHEMA public TO mahasiswa;
GRANT SELECT (nim, nama, jurusan, angkatan) ON mahasiswa TO mahasiswa;
GRANT SELECT (kode_mk, nama_mk, sks) ON matakuliah TO mahasiswa;
GRANT SELECT (nim, kode_mk, nilai) ON perkuliahan TO mahasiswa;
```

### 4.2 User Dosen (✅ Boleh input & update nilai, lihat data mahasiswa)

```sql
GRANT CONNECT ON DATABASE db_kuliah TO dosen;
GRANT USAGE ON SCHEMA public TO dosen;
GRANT SELECT ON mahasiswa TO dosen;
GRANT SELECT ON matakuliah TO dosen;
GRANT SELECT, INSERT, UPDATE ON perkuliahan TO dosen;
```

### 4.3 User Admin Akademik (✅ Full access)

```sql
GRANT CONNECT ON DATABASE db_kuliah TO adminakademik;
GRANT USAGE ON SCHEMA public TO adminakademik;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO adminakademik;
```

---

## 🧩 5. Menggunakan Roles

Jika banyak user dengan peran sama, gunakan **roles**.

```sql
-- Role Mahasiswa
CREATE ROLE mahasiswa_role;
GRANT SELECT ON mahasiswa, matakuliah, perkuliahan TO mahasiswa_role;
GRANT mahasiswa_role TO mahasiswa;

-- Role Dosen
CREATE ROLE dosen_role;
GRANT SELECT ON mahasiswa, matakuliah TO dosen_role;
GRANT SELECT, INSERT, UPDATE ON perkuliahan TO dosen_role;
GRANT dosen_role TO dosen;

-- Role Admin Akademik
CREATE ROLE adminakademik_role;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO adminakademik_role;
GRANT adminakademik_role TO adminakademik;
```

---

## 🧩 6. Uji Coba Hak Akses

### 6.1 Login sebagai Mahasiswa

```bash
psql -U mahasiswa -d db_kuliah
```

Uji:

```sql
SELECT * FROM perkuliahan; -- ✅ Bisa
INSERT INTO perkuliahan(nim, kode_mk, nidn, nilai) VALUES ('2023001','MK001','D001',90); -- ❌ Ditolak
```

### 6.2 Login sebagai Dosen

```bash
psql -U dosen -d db_kuliah
```

Uji:

```sql
SELECT * FROM mahasiswa; -- ✅ Bisa
INSERT INTO perkuliahan(nim, kode_mk, nidn, nilai) VALUES ('2023001','MK001','D001',90); -- ✅ Bisa
DELETE FROM perkuliahan WHERE id=1; -- ❌ Ditolak
```

### 6.3 Login sebagai Admin Akademik

```bash
psql -U adminakademik -d db_kuliah
```

Uji:

```sql
INSERT INTO mahasiswa VALUES ('2023001','Budi','Informatika',2023); -- ✅ Bisa
DELETE FROM mahasiswa WHERE nim='2023001'; -- ✅ Bisa
```

---

## 🧩 7. Prinsip Least Privilege

* Mahasiswa → **hanya SELECT**.
* Dosen → **SELECT, INSERT, UPDATE** pada nilai.
* Admin Akademik → **full access**.
  ➡️ **Tidak ada user yang punya lebih banyak hak dari yang dibutuhkan**.

---

## 📌 Latihan Mandiri

1. Buat user baru: **operator** dengan hak akses **SELECT & INSERT** pada `mahasiswa`.
2. Buat role baru: **keuangan_role** dengan akses hanya ke tabel `mahasiswa` (untuk melihat angkatan).
3. Uji coba login sebagai operator, lalu coba `INSERT` dan `DELETE`.

---

## 🧩 8. Kesimpulan

* Manajemen user & hak akses penting untuk **keamanan database**.
* PostgreSQL mendukung **GRANT, REVOKE, ROLE** untuk kontrol akses.
* Prinsip **least privilege** memastikan keamanan & stabilitas sistem.

---