# **MODUL PRAKTIKUM PERTEMUAN 1**
# **INSTALASI DAN KONFIGURASI AWAL POSTGRESQL**

---

## **📋 INFORMASI MODUL**

| **Item** | **Detail** |
|----------|------------|
| **Mata Kuliah** | Administrasi Basis Data |
| **Pertemuan** | 1 (Pertama) |
| **Durasi** | 3 x 50 menit |
| **Topik** | Instalasi dan Konfigurasi Awal PostgreSQL |
| **Platform** | Windows, macOS, Linux |

---

## **🎯 CAPAIAN PEMBELAJARAN**

Setelah menyelesaikan praktikum ini, mahasiswa diharapkan mampu:

1. **Menginstal PostgreSQL** di sistem operasi yang digunakan (C3)
2. **Mengkonfigurasi** pengaturan dasar PostgreSQL (C3)
3. **Menguji koneksi** dan operasi dasar database (C3)
4. **Menggunakan tools** administrasi PostgreSQL (pgAdmin, psql) (C3)
5. **Menerapkan keamanan dasar** pada instalasi PostgreSQL (C3)

---

## **📚 MATERI PRASYARAT**

- Pemahaman dasar sistem operasi (Windows/macOS/Linux)
- Konsep dasar database dan SQL
- Pemahaman tentang command line interface
- Hak akses administrator/root pada komputer

---

## **🛠️ TOOLS DAN SOFTWARE YANG DIBUTUHKAN**

### **Software Utama:**
- PostgreSQL Server (versi 15 atau terbaru)
- pgAdmin 4 (GUI Administration Tool)
- Command Line Interface (Terminal/Command Prompt)

### **Software Pendukung:**
- Text Editor (Notepad++, VS Code, atau sejenisnya)
- Web Browser (untuk mengakses pgAdmin)

---

## **📖 BAGIAN 1: TEORI SINGKAT**

### **Apa itu PostgreSQL?**

PostgreSQL adalah sistem manajemen basis data relasional (RDBMS) open-source yang powerful dan feature-rich. Dikenal sebagai "The World's Most Advanced Open Source Relational Database."

### **Keunggulan PostgreSQL:**
- **ACID Compliance**: Menjamin konsistensi data
- **Extensible**: Mendukung custom functions, data types, dan operators
- **Standards Compliant**: Mengikuti standar SQL
- **Robust Security**: Fitur keamanan yang comprehensive
- **Cross-Platform**: Berjalan di berbagai sistem operasi

### **Komponen Utama PostgreSQL:**
- **PostgreSQL Server**: Database engine utama
- **psql**: Command-line interface
- **pgAdmin**: Web-based administration tool
- **pg_dump/pg_restore**: Backup dan restore utilities

---

## **💻 BAGIAN 2: PRAKTIKUM INSTALASI**

### **🖥️ WINDOWS INSTALLATION**

#### **Langkah 1: Download PostgreSQL**

1. Buka browser dan kunjungi: https://www.postgresql.org/download/windows/
2. Klik **"Download the installer"**
3. Pilih versi PostgreSQL terbaru (PostgreSQL 15.x atau 16.x)
4. Download file installer (.exe)

#### **Langkah 2: Menjalankan Installer**

1. **Jalankan installer** sebagai Administrator
2. **Welcome Screen**: Klik "Next"
3. **Installation Directory**: 
   - Default: `C:\Program Files\PostgreSQL\15`
   - Klik "Next"
4. **Select Components**: Pastikan semua komponen tercentang
   - ✅ PostgreSQL Server
   - ✅ pgAdmin 4
   - ✅ Stack Builder
   - ✅ Command Line Tools

#### **Langkah 3: Konfigurasi Database**

1. **Data Directory**: 
   - Default: `C:\Program Files\PostgreSQL\15\data`
   - Klik "Next"
2. **Password untuk superuser (postgres)**:
   - Masukkan password yang kuat
   - **Ingat password ini!** 
   - Contoh: `PostgreSQL2024!`
3. **Port Number**:
   - Default: `5432`
   - Klik "Next"
4. **Advanced Options**:
   - Locale: Default
   - Klik "Next"

#### **Langkah 4: Instalasi**

1. **Pre Installation Summary**: Review pengaturan
2. Klik **"Next"** untuk memulai instalasi
3. Tunggu proses instalasi selesai (5-10 menit)
4. **Installation Complete**: Klik "Finish"

### **🍎 MACOS INSTALLATION**

#### **Metode 1: Menggunakan Official Installer**

1. **Download**: https://www.postgresql.org/download/macosx/
2. **Install**: Ikuti wizard installer seperti di Windows
3. **Password**: Set password untuk user `postgres`

#### **Metode 2: Menggunakan Homebrew (Recommended)**

```bash
# Install Homebrew jika belum ada
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install PostgreSQL
brew install postgresql@15

# Start PostgreSQL service
brew services start postgresql@15

# Create database user
createuser --interactive --pwprompt
```

### **🐧 LINUX INSTALLATION (Ubuntu/Debian)**

#### **Metode 1: Package Manager**

```bash
# Update package list
sudo apt update

# Install PostgreSQL
sudo apt install postgresql postgresql-contrib

# Install pgAdmin4
sudo apt install pgadmin4 pgadmin4-apache2

# Start PostgreSQL service
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

#### **Metode 2: Official PostgreSQL Repository**

```bash
# Create the file repository configuration
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# Import signing key
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

# Update package list
sudo apt-get update

# Install PostgreSQL
sudo apt-get install postgresql-15 postgresql-client-15
```

---

## **⚙️ BAGIAN 3: KONFIGURASI AWAL**

### **Langkah 1: Verifikasi Instalasi**

#### **Windows:**
```cmd
# Buka Command Prompt sebagai Administrator
cd "C:\Program Files\PostgreSQL\15\bin"

# Test koneksi
psql -U postgres -h localhost

# Jika berhasil, akan muncul prompt:
postgres=#
```

#### **macOS/Linux:**
```bash
# Test PostgreSQL service
sudo systemctl status postgresql

# Connect ke database
sudo -u postgres psql

# Atau
psql -U postgres -h localhost
```

### **Langkah 2: Konfigurasi User dan Database**

```sql
-- Di dalam psql prompt

-- 1. Lihat versi PostgreSQL
SELECT version();

-- 2. Lihat daftar database
\l

-- 3. Buat user baru untuk praktikum
CREATE USER mahasiswa WITH PASSWORD 'rahasia123';

-- 4. Buat database untuk praktikum
CREATE DATABASE praktikum_db OWNER mahasiswa;

-- 5. Berikan privileges
GRANT ALL PRIVILEGES ON DATABASE praktikum_db TO mahasiswa;

-- 6. Lihat daftar users
\du

-- 7. Keluar dari psql
\q
```

### **Langkah 3: Konfigurasi Akses Eksternal**

#### **Edit postgresql.conf:**

**Windows Location:**
```
C:\Program Files\PostgreSQL\15\data\postgresql.conf
```

**Linux Location:**
```
/etc/postgresql/15/main/postgresql.conf
```

**Pengaturan:**
```ini
# Ubah pengaturan berikut:
listen_addresses = 'localhost'          # Untuk lokal saja
# listen_addresses = '*'                # Untuk akses eksternal

port = 5432                             # Port default

# Memory settings (optional, sesuaikan dengan RAM)
shared_buffers = 128MB
effective_cache_size = 512MB
```

#### **Edit pg_hba.conf:**

**Lokasi sama dengan postgresql.conf**

**Tambahkan baris:**
```ini
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" untuk Unix domain socket connections only
local   all             all                                     peer

# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
host    all             all             ::1/128                 md5

# Untuk akses dari jaringan lokal (optional):
# host    all             all             192.168.1.0/24          md5
```

### **Langkah 4: Restart PostgreSQL Service**

#### **Windows:**
```cmd
# Via Services atau Command Prompt
net stop postgresql-x64-15
net start postgresql-x64-15
```

#### **Linux:**
```bash
sudo systemctl restart postgresql
```

#### **macOS:**
```bash
brew services restart postgresql@15
```

---

## **🔧 BAGIAN 4: INSTALASI DAN KONFIGURASI PGADMIN**

### **Langkah 1: Akses pgAdmin 4**

#### **Windows/macOS:**
- pgAdmin biasanya terinstall otomatis
- Cari "pgAdmin 4" di Start Menu/Applications
- Atau buka browser: `http://localhost:5050`

#### **Linux:**
```bash
# Jika menggunakan web version
sudo /usr/pgadmin4/bin/setup-web.sh

# Setup email dan password untuk pgAdmin
# Akses via browser: http://localhost/pgadmin4
```

### **Langkah 2: Konfigurasi Server Connection**

1. **Buka pgAdmin 4**
2. **Klik kanan pada "Servers"** → "Register" → "Server"
3. **General Tab:**
   - Name: `Local PostgreSQL`
4. **Connection Tab:**
   - Host: `localhost`
   - Port: `5432`
   - Username: `postgres`
   - Password: `[password yang dibuat saat instalasi]`
   - Save password: ✅
5. **Klik "Save"**

### **Langkah 3: Verifikasi Koneksi**

1. **Expand "Local PostgreSQL"** di sidebar kiri
2. **Lihat daftar databases:**
   - postgres (default)
   - praktikum_db (yang kita buat)
3. **Test Query:**
   - Tools → Query Tool
   - Jalankan query: `SELECT current_timestamp;`

---

## **🧪 BAGIAN 5: TESTING DAN VALIDASI**

### **Test 1: Koneksi via Command Line**

```bash
# Test koneksi dengan user mahasiswa
psql -U mahasiswa -d praktikum_db -h localhost

# Jika berhasil:
praktikum_db=>
```

### **Test 2: Operasi Database Dasar**

```sql
-- Di dalam psql atau pgAdmin Query Tool

-- 1. Buat tabel test
CREATE TABLE test_koneksi (
    id SERIAL PRIMARY KEY,
    nama VARCHAR(50),
    waktu_dibuat TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 2. Insert data
INSERT INTO test_koneksi (nama) VALUES 
('Test 1'),
('Test 2'),
('Test 3');

-- 3. Query data
SELECT * FROM test_koneksi;

-- 4. Lihat struktur tabel
\d test_koneksi

-- 5. Drop tabel
DROP TABLE test_koneksi;
```

### **Test 3: Performance Test Sederhana**

```sql
-- Test performa sederhana
EXPLAIN ANALYZE SELECT version();

-- Test memory allocation
SHOW shared_buffers;
SHOW work_mem;
```

---

## **❗ TROUBLESHOOTING**

### **Problem 1: Service PostgreSQL tidak bisa start**

**Symptoms:** Error saat start service

**Solutions:**
```bash
# Windows
# 1. Check Event Viewer for errors
# 2. Verify data directory permissions
# 3. Check port 5432 availability

# Linux
sudo systemctl status postgresql
sudo journalctl -u postgresql

# Check log files
tail -f /var/log/postgresql/postgresql-15-main.log
```

### **Problem 2: Cannot connect to server**

**Symptoms:** `psql: error: connection to server failed`

**Solutions:**
1. **Verify service is running:**
   ```bash
   # Windows
   sc query postgresql-x64-15
   
   # Linux
   sudo systemctl status postgresql
   ```

2. **Check port availability:**
   ```bash
   netstat -an | grep 5432
   # Atau
   telnet localhost 5432
   ```

3. **Verify pg_hba.conf configuration**

### **Problem 3: Password authentication failed**

**Solutions:**
1. **Reset postgres password:**
   ```bash
   # Linux
   sudo -u postgres psql
   ALTER USER postgres PASSWORD 'newpassword';
   ```

2. **Check pg_hba.conf method** (md5 vs peer vs trust)

### **Problem 4: pgAdmin connection error**

**Solutions:**
1. **Verify server registration settings**
2. **Check firewall settings**
3. **Ensure PostgreSQL service is running**

---

## **📝 LATIHAN DAN TUGAS**

### **Latihan 1: Instalasi Verification Checklist**

Buat screenshot dan dokumentasi untuk:

- [ ] PostgreSQL version information
- [ ] List of installed components
- [ ] pgAdmin 4 interface
- [ ] Successful database connection
- [ ] Created user 'mahasiswa'
- [ ] Created database 'praktikum_db'

### **Latihan 2: Basic Operations**

Lakukan operasi berikut dan dokumentasikan hasilnya:

```sql
-- 1. Buat schema baru
CREATE SCHEMA mahasiswa_schema;

-- 2. Buat tabel dalam schema tersebut
CREATE TABLE mahasiswa_schema.biodata (
    nim VARCHAR(10) PRIMARY KEY,
    nama VARCHAR(100) NOT NULL,
    jurusan VARCHAR(50),
    angkatan INTEGER,
    email VARCHAR(100)
);

-- 3. Insert data pribadi
INSERT INTO mahasiswa_schema.biodata VALUES 
('[NIM_ANDA]', '[NAMA_ANDA]', 'Teknik Informatika', 2024, '[EMAIL_ANDA]');

-- 4. Query dan tampilkan data
SELECT * FROM mahasiswa_schema.biodata;
```

### **Latihan 3: Configuration Analysis**

1. **Dokumentasikan lokasi file konfigurasi:**
   - postgresql.conf
   - pg_hba.conf
   - Data directory

2. **Analisis pengaturan memory:**
   ```sql
   SELECT name, setting, unit, context 
   FROM pg_settings 
   WHERE name IN ('shared_buffers', 'work_mem', 'effective_cache_size');
   ```

3. **Check active connections:**
   ```sql
   SELECT datname, usename, application_name, client_addr, state
   FROM pg_stat_activity;
   ```

---

## **📋 LAPORAN PRAKTIKUM**

### **Template Laporan:**

**1. Cover Page**
- Nama Mahasiswa
- NIM
- Kelas
- Tanggal Praktikum

**2. Tujuan Praktikum**

**3. Langkah Instalasi**
- Screenshot setiap langkah penting
- Dokumentasi konfigurasi yang dilakukan

**4. Testing Results**
- Screenshot hasil testing
- Query yang dijalankan dan hasilnya

**5. Troubleshooting** (jika ada)
- Problem yang dihadapi
- Solusi yang diterapkan

**6. Kesimpulan**
- Evaluasi proses instalasi
- Lessons learned

---

## **📚 REFERENSI DAN BACAAN LANJUTAN**

### **Dokumentasi Resmi:**
- [PostgreSQL Official Documentation](https://www.postgresql.org/docs/)
- [pgAdmin Documentation](https://www.pgadmin.org/docs/)

### **Tutorial Online:**
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)
- [W3Schools PostgreSQL](https://www.w3schools.com/postgresql/)

### **Books:**
- "PostgreSQL: Up and Running" by Regina Obe
- "PostgreSQL 14 Administration Cookbook" by Simon Riggs

---

## **✅ CHECKLIST KEBERHASILAN**

Praktikum dianggap berhasil jika:

- [ ] PostgreSQL terinstall dan service berjalan
- [ ] Dapat connect via psql command line
- [ ] pgAdmin 4 dapat mengakses database
- [ ] User 'mahasiswa' berhasil dibuat
- [ ] Database 'praktikum_db' berhasil dibuat
- [ ] Dapat menjalankan query SQL sederhana
- [ ] Dapat membuat dan manipulasi tabel
- [ ] File konfigurasi dapat diakses dan dipahami

---