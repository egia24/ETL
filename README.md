# ETL Pipeline: CNN News Data Warehouse

Project ini adalah **ETL pipeline** untuk mengambil, membersihkan, dan memuat data berita dari API **CNN News Indonesia** ke dalam **Data Warehouse MySQL/PostgreSQL**, lengkap dengan logging untuk monitoring.

---

## Dataset
- **Source API:** [CNN News Indonesia API](https://berita-indo-api.vercel.app/v1/cnn-news)  
- **Data yang diambil:**  
  - Judul berita  
  - Link berita  
  - Ringkasan berita  
  - Tanggal publikasi  

---

## Proses ETL

### 1. Extract
- Mengambil data dari API menggunakan Python dan menyimpannya dalam **Pandas DataFrame**.  
- Memastikan data mencakup kolom yang dibutuhkan untuk proses transformasi dan load.  

**Goal:** Data dapat diambil dengan benar untuk analisis lebih lanjut.

---

### 2. Transform
- Pilih kolom relevan dan rename:  
  - `title` → `news_title`  
  - `link` → `news_link`  
  - `contentSnippet` → `news_summary`  
  - `isoDate` → `published_at`  
- Hapus duplikasi berdasarkan `news_link`.  
- Cek missing values, drop atau isi sesuai kebutuhan.  
- Normalisasi teks:  
  - `news_title` → Title Case  
  - `news_summary` → lowercase  
- Tambahkan kolom turunan:  
  - `title_length` → jumlah karakter judul  
  - `word_count` → jumlah kata ringkasan  
- Filter berita dengan `word_count > 5`.  

**Goal:** Data bersih, konsisten, dan siap dimuat ke Data Warehouse.

---

### 3. Load
- Buat tabel staging `stg_news` di MySQL/PostgreSQL.  
- Gunakan **truncate-insert** untuk mencegah duplikasi.  
- Masukkan data hasil transformasi ke tabel staging.

**Goal:** Data terbaru tersimpan rapi dan siap untuk analisis/reporting.

---

### 4. Logging
- Gunakan modul `logging` Python untuk mencatat event ke file `etl_log.txt`.  
- Informasi yang dicatat:  
  - Waktu mulai dan selesai ETL  
  - Jumlah record hasil extract  
  - Jumlah record setelah transform  
  - Jumlah record yang berhasil load  
  - Log error jika terjadi kegagalan di extract, transform, atau load  

**Goal:** Memastikan pipeline ETL bisa ditelusuri, memudahkan debugging, dan menjaga transparansi.

---

## Tools & Teknologi
- **Python** (Pandas, Requests, logging)  
- **MySQL/PostgreSQL**  
- **ETL & Data Warehouse**  
