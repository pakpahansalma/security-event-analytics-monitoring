# Security Event Analytics & Monitoring
Security Monitoring Dashboard · Python, Elasticsearch & Kibana

Proyek analitik keamanan siber yang mengolah data peristiwa keamanan jaringan menggunakan Python, menyimpan data terstruktur di Elasticsearch, dan memvisualisasikan aktivitas keamanan melalui dashboard interaktif Kibana.

## Gambaran Umum

Proyek ini mendemonstrasikan alur kerja analitik keamanan siber dari awal hingga akhir, mulai dari persiapan dataset mentah dan transformasi data, hingga penyimpanan, pencarian, dan visualisasi data.

Tujuan proyek ini adalah mengeksplorasi pola peristiwa keamanan, memantau tingkat keparahan, memeriksa tindakan penanganan, serta menganalisis tren peristiwa melalui dashboard terpusat.

## Tujuan Proyek

- Menyiapkan dan mentransformasi data peristiwa keamanan siber menggunakan Python.
- Menyusun struktur data agar dapat diindeks ke Elasticsearch.
- Mengeksplorasi jenis serangan, tingkat keparahan, dan tindakan penanganan keamanan.
- Memvisualisasikan distribusi peristiwa dan tren berdasarkan waktu menggunakan Kibana.
- Membangun dashboard untuk mendukung pemantauan keamanan dan investigasi peristiwa.

## Teknologi yang Digunakan

| Teknologi     | Kegunaan                                          |
| ------------- | ------------------------------------------------- |
| Python        | Persiapan dan transformasi data                   |
| Pandas        | Pembersihan dan manipulasi data                   |
| Google Colab  | Lingkungan pengembangan Python                    |
| Elasticsearch | Penyimpanan dan pencarian data peristiwa keamanan |
| Kibana        | Visualisasi data dan pembuatan dashboard          |

## Dataset

**Sumber:** Dataset *Cybersecurity Attacks* dari Kaggle.

Dataset ini memiliki 40.000 baris data dan 25 kolom asli. Sebanyak 16 field dipilih dan disiapkan untuk proses pengindeksan ke Elasticsearch.

Field yang digunakan meliputi:

- `timestamp`
- `source_ip` dan `destination_ip`
- `source_port` dan `destination_port`
- `protocol`
- `packet_length`
- `attack_type`
- `severity_level`
- `action_taken`
- `anomaly_score`
- `attack_signature`
- `packet_type`
- `traffic_type`
- `network_segment`
- `log_source`

Field tersebut mendukung analisis waktu kejadian, alamat IP sumber dan tujuan, kategori serangan, tingkat keparahan, serta tindakan penanganan yang tercatat.

## Alur Kerja Proyek

1. **Pengambilan Data Mentah**
   Mengunduh dataset keamanan siber dari Kaggle sebagai sumber data awal.

2. **Persiapan Data dengan Python**
   Membaca dataset, memilih kolom yang relevan, menyeragamkan nama field, dan menyiapkan data untuk proses pengindeksan.

3. **Pengindeksan ke Elasticsearch**
   Menentukan pemetaan tipe data (*field mapping*) dan memasukkan data peristiwa keamanan ke Elasticsearch.

4. **Validasi Data**
   Memastikan bahwa index Elasticsearch berisi 40.000 dokumen sesuai jumlah data yang diharapkan.

5. **Pembuatan Visualisasi di Kibana**
   Menghubungkan Kibana dengan data melalui Data View dan membuat visualisasi untuk analisis keamanan.

6. **Penyusunan Dashboard Pemantauan**
   Menggabungkan metrik KPI, grafik distribusi, analisis tren waktu, dan tabel detail peristiwa ke dalam satu dashboard.

## Dashboard Kibana

Dashboard bernama **Security Event Analytics & Monitoring** dirancang untuk memberikan gambaran umum mengenai peristiwa keamanan serta membantu proses investigasi lebih lanjut.

### 1. Indikator Kinerja Utama (KPI)

- Total Security Events
- High Severity Events
- Blocked Events
- Average Anomaly Score

### 2. Gambaran Serangan (*Attack Overview*)

- Attack Types Distribution
- Security Events by Severity

### 3. Aktivitas Keamanan (*Security Activity*)

- Security Actions Distribution
- Network Protocol Distribution

### 4. Tren dan Analisis (*Trend & Analysis*)

- Security Events Over Time
- Attack Type by Severity

### 5. Log Keamanan dan Detail Peristiwa

Tabel detail menampilkan catatan peristiwa individual dengan field seperti waktu kejadian, tingkat keparahan, IP sumber, IP tujuan, jenis serangan, tindakan penanganan, dan skor anomali.

## Validasi Data

Data hasil persiapan berhasil diindeks ke Elasticsearch.

- **Jumlah dokumen yang diharapkan:** 40.000
- **Jumlah dokumen yang terindeks:** 40.000
- **Jumlah field yang dipilih:** 16
- **Nilai kosong pada dataset hasil persiapan:** 0

## Struktur Repository

```text
security-event-analytics-monitoring/
├── data/
│   ├── README.md
│   └── .gitkeep
├── notebooks/
│   └── security_event_analysis.ipynb
├── screenshots/
│   └── .gitkeep
├── elasticsearch/
│   └── index_mapping.json
├── README.md
└── requirements.txt
```

*Struktur folder di atas merupakan rancangan repository. Setiap file akan ditambahkan setelah disiapkan dan diverifikasi.*

## Pengembangan Selanjutnya

- Melakukan analisis lebih mendalam terhadap pola serangan dan hubungan dengan tingkat keparahan.
- Menginvestigasi pola skor anomali yang tidak biasa.
- Menambahkan fitur penyaringan dan penelusuran detail peristiwa.
- Mendokumentasikan proses pemasukan data dan pemetaan index agar dapat direproduksi.
- Melengkapi dokumentasi dashboard dengan screenshot dan temuan analisis.

## Penafian

Proyek ini dibuat untuk tujuan pembelajaran dan portofolio. Dataset digunakan untuk mendemonstrasikan analitik peristiwa keamanan dan pengembangan dashboard, bukan sebagai sistem pemantauan keamanan produksi secara langsung.

## Tentang Penulis

**Salma Cristianes Roito Pakpahan**

Bidang: Data Analytics | Cybersecurity Analytics | Python | Elasticsearch | Kibana
