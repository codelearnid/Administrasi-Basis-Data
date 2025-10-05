#  Materi Perkuliahan: **Backup dan Recovery Database**

## **Capaian Pembelajaran**

Setelah mengikuti perkuliahan ini, mahasiswa mampu:

1. Menjelaskan konsep dasar backup dan recovery.
2. Menentukan strategi backup yang sesuai dengan kebutuhan organisasi.
3. Melakukan simulasi backup dan recovery pada sistem DBMS (misalnya PostgreSQL/MySQL).
4. Memahami perbedaan antara backup fisik dan logis.
5. Mengimplementasikan restore data dari hasil backup.

---

## **1. Pengantar Backup dan Recovery**

###  Apa itu Backup?

Backup adalah proses **menyalin data dari database utama ke media cadangan** untuk mencegah kehilangan data akibat:

* Kegagalan perangkat keras,
* Kesalahan manusia,
* Serangan siber,
* Bencana alam, dsb.

### Apa itu Recovery?

Recovery adalah proses **mengembalikan database** ke kondisi semula (normal) setelah terjadi kegagalan atau kehilangan data.

---

## **2. Jenis-Jenis Backup**

| Jenis Backup            | Penjelasan                                                     | Kelebihan                         | Kekurangan                         |
| ----------------------- | -------------------------------------------------------------- | --------------------------------- | ---------------------------------- |
| **Full Backup**         | Menyalin seluruh isi database.                                 | Sederhana, lengkap.               | Ukuran besar, waktu lama.          |
| **Incremental Backup**  | Menyimpan hanya perubahan sejak backup terakhir.               | Cepat, hemat ruang.               | Proses recovery lebih kompleks.    |
| **Differential Backup** | Menyimpan perubahan sejak backup penuh terakhir.               | Lebih cepat dari full.            | Ukuran bertambah seiring waktu.    |
| **Logical Backup**      | Menggunakan perintah SQL (misalnya `mysqldump`, `pg_dump`).    | Mudah dipindahkan ke server lain. | Tidak menyimpan konfigurasi fisik. |
| **Physical Backup**     | Menyalin file data dan log database langsung dari sistem file. | Cocok untuk recovery cepat.       | Tidak portabel antar platform.     |

---

## **3. Strategi Backup**

Beberapa strategi umum:

* **Cold Backup (Offline Backup):**

  * Dilakukan ketika database dimatikan.
  * Aman dari perubahan data, tetapi mengganggu layanan.

* **Hot Backup (Online Backup):**

  * Dilakukan saat database masih berjalan.
  * Cocok untuk sistem 24 jam, tapi butuh konfigurasi tambahan (misalnya WAL di PostgreSQL).

* **Scheduled Backup:**

  * Backup otomatis sesuai jadwal (cron job, task scheduler, dsb).

---

## **4. Recovery: Jenis dan Teknik**

| Jenis Recovery                    | Deskripsi                                                   | Contoh                            |
| --------------------------------- | ----------------------------------------------------------- | --------------------------------- |
| **Complete Recovery**             | Mengembalikan database ke keadaan sebelum crash.            | Restore full backup + log.        |
| **Point-in-Time Recovery (PITR)** | Mengembalikan database ke waktu tertentu sebelum kegagalan. | PostgreSQL WAL, Oracle Flashback. |
| **Media Recovery**                | Mengganti file data yang rusak dengan backup terbaru.       | Restore file *.dbf* atau *.mdf*.  |
| **Crash Recovery**                | Pemulihan otomatis setelah sistem restart.                  | Log-based recovery.               |

---

## **5. Implementasi Praktis (Contoh DBMS)**

### **MySQL / MariaDB**

```bash
# Backup database
mysqldump -u root -p nama_database > backup.sql

# Restore database
mysql -u root -p nama_database < backup.sql
```

### **PostgreSQL**

```bash
# Backup (logical)
pg_dump -U postgres -F c -b -v -f backup.dump nama_database

# Restore
pg_restore -U postgres -d nama_database -v backup.dump
```

---

## **6. Best Practices**

1. Gunakan **penamaan backup yang konsisten** (misal: `db_backup_YYYYMMDD.sql`).
2. Simpan backup di **lokasi berbeda** (server lain atau cloud).
3. Lakukan **pengujian recovery secara berkala**.
4. Gunakan **enkripsi** pada file backup.
5. Catat dan dokumentasikan **jadwal backup** dan **lokasi penyimpanan**.

---

## **7. Studi Kasus Diskusi Kelas**

> “Sebuah perusahaan e-commerce kehilangan data transaksi selama 12 jam terakhir karena crash pada server. Mereka memiliki backup full setiap minggu dan incremental setiap 6 jam.
> Bagaimana strategi recovery terbaik yang bisa digunakan?”

Mahasiswa bisa diajak menganalisis langkah-langkah pemulihan data dan menjelaskan konsekuensi tiap pilihan.

---

## **8. Praktikum (Tugas Mahasiswa)**

1. Lakukan backup dan recovery sederhana menggunakan MySQL atau PostgreSQL.
2. Simulasikan kehilangan tabel lalu pulihkan menggunakan hasil backup.
3. Dokumentasikan langkah-langkahnya dan waktu yang dibutuhkan.

---

## **9. Evaluasi Pembelajaran**

| Bentuk Evaluasi | Bobot | Contoh Soal                                             |
| --------------- | ----- | ------------------------------------------------------- |
| Quiz Teori      | 30%   | Jelaskan perbedaan incremental dan differential backup! |
| Praktikum       | 50%   | Lakukan backup & restore database menggunakan pg_dump!  |
| Diskusi Kasus   | 20%   | Tentukan strategi backup terbaik untuk sistem 24 jam.   |

---

