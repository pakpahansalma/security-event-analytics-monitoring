# Security Event Analytics & Monitoring

Security Monitoring Dashboard · Python, Elasticsearch & Kibana

**Analisis dan pemantauan peristiwa keamanan siber menggunakan Python, Elasticsearch, dan Kibana.**

🌐 Bahasa: [Indonesia](README.md) | [English](README_eng.md)

## 1. Gambaran Umum

Proyek ini membangun alur analisis dan pemantauan peristiwa keamanan siber (*cybersecurity events*) menggunakan dataset dari Kaggle. Proyek mencakup pemeriksaan kualitas data, data cleaning, transformasi data, penyimpanan terindeks di Elasticsearch, serta visualisasi dan pemantauan menggunakan Kibana.

Dashboard dirancang untuk memberikan gambaran mengenai tingkat keparahan peristiwa keamanan, jenis serangan, tindakan keamanan, protokol jaringan, skor anomali, dan aktivitas keamanan dari waktu ke waktu.

## 2. Tujuan Proyek

- Melakukan pemeriksaan kualitas dan persiapan data keamanan siber.
- Menerapkan proses data cleaning dan transformasi yang terdokumentasi.
- Menyiapkan struktur data yang sesuai untuk Elasticsearch.
- Memvisualisasikan pola dan distribusi peristiwa keamanan melalui Kibana.
- Menyediakan dashboard untuk eksplorasi dan pemantauan data keamanan.

## 3. Teknologi yang Digunakan

| Teknologi | Penggunaan |
|---|---|
| Python | Data preparation, cleaning, transformasi, dan validasi |
| Pandas | Manipulasi dan pemeriksaan dataset |
| Google Colab | Lingkungan pengembangan notebook |
| Elasticsearch | Penyimpanan dan pencarian dokumen keamanan |
| Kibana | Visualisasi data dan dashboard monitoring |
| Kaggle | Sumber dataset |

## 4. Dataset

Dataset yang digunakan adalah `cybersecurity_attacks`, yang diperoleh dari Kaggle.

Karakteristik dataset:

- Jumlah data awal: 40.000 baris.
- Jumlah kolom awal: 25.
- Kolom yang digunakan untuk indeks Elasticsearch: 16.
- Data merepresentasikan informasi terkait lalu lintas jaringan dan peristiwa keamanan siber.

Kolom yang digunakan dalam indeks utama meliputi timestamp, alamat IP sumber dan tujuan, port, protokol, panjang paket, jenis serangan, tingkat keparahan, tindakan keamanan, skor anomali, tanda tangan serangan, tipe paket, tipe lalu lintas, segmen jaringan, dan sumber log.

## 5. Alur Kerja Proyek

1. Dataset dikumpulkan dan dibaca menggunakan Python.
2. Struktur data dan kualitas dataset diperiksa.
3. Data cleaning dan pemeriksaan konsistensi dilakukan.
4. Kolom yang relevan dipilih dan disiapkan untuk Elasticsearch.
5. Tipe data dan nama kolom disesuaikan dengan skema indeks.
6. Data divalidasi sebelum proses indexing.
7. Data dikirim ke Elasticsearch.
8. Data dieksplorasi dan divisualisasikan melalui Kibana.

## 6. Data Preparation & Cleaning

Tahap persiapan data dilakukan sebelum proses indexing agar struktur data lebih konsisten dan sesuai dengan kebutuhan analisis keamanan.

### 6.1. Data Loading

Dataset dibaca menggunakan Pandas. Data awal disimpan dalam `df_raw`, sedangkan salinan kerja untuk proses pemeriksaan dan cleaning disimpan dalam `df_clean`.

Pemisahan ini membantu mempertahankan referensi terhadap dataset awal selama proses persiapan data.

### 6.2. Data Quality Assessment

Pemeriksaan kualitas data meliputi:

- Struktur dataset, jumlah baris, jumlah kolom, dan tipe data.
- Pemeriksaan baris duplikat.
- Distribusi kategori dan rentang nilai numerik.
- Validitas alamat IP.
- Rentang dan validitas timestamp.
- Distribusi port jaringan.
- Konsistensi antara `Traffic Type` dan `Protocol`.

Hasil pemeriksaan digunakan untuk memahami karakteristik data dan mengidentifikasi potensi masalah kualitas sebelum data diindeks.

### 6.3. Missing Value Analysis

Pemeriksaan nilai kosong dilakukan, termasuk pada kolom berikut:

- `Malware Indicators`
- `Alerts/Warnings`
- `Proxy Information`
- `Firewall Logs`
- `IDS/IPS Alerts`

Nilai kosong tidak langsung diimputasi atau diganti dengan nilai buatan. Keputusan ini membantu menghindari asumsi yang tidak didukung oleh informasi dalam dataset.

Kolom-kolom tersebut tidak dimasukkan ke dalam indeks utama Elasticsearch yang menggunakan 16 kolom terpilih. Oleh karena itu, hasil akhir `df_es` tidak memiliki missing values. Hal ini tidak berarti bahwa semua missing values dalam dataset mentah telah dihapus atau diperbaiki.

### 6.4. Timestamp Standardization

Kolom `Timestamp` dikonversi ke format datetime dengan standardisasi zona waktu UTC.

Validasi timestamp menunjukkan bahwa seluruh 40.000 nilai timestamp berhasil dikonversi. Rentang waktu ini kemudian digunakan sebagai dasar pemilihan rentang waktu pada dashboard Kibana.

### 6.5. Text dan Consistency Checks

Pemeriksaan tambahan mencakup whitespace, placeholder teks, serta konsistensi kategori.

Perubahan dilakukan secara hati-hati agar tidak mengubah makna data yang sudah tersedia. Berdasarkan hasil validasi notebook, perubahan pada tahap cleaning tercatat hanya pada kolom `Timestamp`.

### 6.6. Hasil Data Cleaning

Setelah proses cleaning:

- Jumlah baris tetap 40.000.
- Jumlah kolom tetap 25 pada `df_clean`.
- Tidak ditemukan baris duplikat.
- Seluruh timestamp berhasil dikonversi.
- Data dipertahankan tanpa penghapusan baris pada tahap cleaning yang terdokumentasi.

## 7. Data Transformation & Preparation for Elasticsearch

Setelah cleaning, kolom yang relevan dipilih untuk kebutuhan analisis dan dashboard.

Dataset hasil pemilihan kolom disiapkan dalam `df_index`, kemudian digunakan untuk membentuk `df_es`.

Transformasi yang dilakukan mencakup:

- Pemilihan 16 kolom dari 25 kolom awal.
- Standardisasi nama kolom menjadi `snake_case`, misalnya `Source IP Address` menjadi `source_ip`.
- Penyesuaian tipe data agar sesuai dengan mapping Elasticsearch.
- Pemeriksaan hasil akhir sebelum indexing.

### Kolom pada indeks utama

| Kolom | Tipe data Elasticsearch |
|---|---|
| `timestamp` | `date` |
| `source_ip` | `ip` |
| `destination_ip` | `ip` |
| `source_port` | `integer` |
| `destination_port` | `integer` |
| `protocol` | `keyword` |
| `packet_length` | `integer` |
| `attack_type` | `keyword` |
| `severity_level` | `keyword` |
| `action_taken` | `keyword` |
| `anomaly_score` | `float` |
| `attack_signature` | `keyword` |
| `packet_type` | `keyword` |
| `traffic_type` | `keyword` |
| `network_segment` | `keyword` |
| `log_source` | `keyword` |

## 8. Data Validation & Elasticsearch Ingestion

Sebelum dan sesudah indexing, jumlah data diperiksa untuk memastikan proses ingestion berjalan sesuai target.

| Pemeriksaan | Hasil |
|---|---:|
| Baris dataset awal | 40.000 |
| Kolom dataset awal | 25 |
| Baris `df_es` | 40.000 |
| Kolom `df_es` | 16 |
| Missing values pada `df_es` | 0 |
| Dokumen terindeks di Elasticsearch | 40.000 |

Indeks Elasticsearch yang digunakan adalah `security-events-test`.

Jumlah dokumen diverifikasi menggunakan Elasticsearch Count API. Hasilnya menunjukkan bahwa 40.000 dokumen berhasil diindeks.

## 9. Dashboard Kibana

Dashboard **Security Event Analytics & Monitoring** menampilkan beberapa aspek utama keamanan jaringan.

### KPI
- Total Security Events
- High Severity Events
- Blocked Events
- Average Anomaly Score

### Visualisasi
- Attack Types Distribution
- Security Events by Severity
- Security Actions Distribution
- Network Protocol Distribution
- Security Events Over Time
- Attack Type by Severity

### Detail Log
Tabel detail digunakan untuk menampilkan informasi seperti timestamp, severity level, source IP, destination IP, attack type, action taken, anomaly score, dan protocol.

Dashboard membantu pengguna mengeksplorasi distribusi dan tren peristiwa keamanan dalam dataset.

## 10. Struktur Repository

```text
security-event-analytics-monitoring/
├── data/
│   ├── README.md
│   └── .gitkeep
├── notebooks/
│   └── Project_Security_Log_Analytics.ipynb
├── screenshots/
│   └── .gitkeep
├── elasticsearch/
│   └── index_mapping.json
├── README.md
├── README_ID.md
└── requirements.txt
```

Struktur tersebut merupakan struktur repository yang direncanakan. File dimasukkan setelah dibuat dan diperiksa. Dataset mentah dapat dirujuk melalui sumber Kaggle apabila lisensinya tidak mengizinkan redistribusi.

## 11. Pengembangan Selanjutnya

- Menambahkan visualisasi yang lebih mendalam untuk pola serangan.
- Mengembangkan deteksi anomali dengan pendekatan statistik atau machine learning.
- Menambahkan alert untuk kondisi keamanan tertentu.
- Menyempurnakan dokumentasi reproduksibilitas pipeline data.
- Mengembangkan otomatisasi ingestion dan monitoring.

## 12. Disclaimer

Proyek ini dibuat untuk tujuan pembelajaran, eksplorasi data, dan portofolio analisis keamanan siber. Hasil analisis merepresentasikan dataset yang digunakan dan tidak secara otomatis mencerminkan kondisi jaringan produksi secara nyata.

## 13. Author

**Salma Cristianes Roito Pakpahan**

Bidang: Data Analytics | Cybersecurity Analytics | Python | Elasticsearch | Kibana
