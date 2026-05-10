# Imputasi Data Ukuran Choke (Choke Size) Menggunakan Machine Learning

**Studi Kasus: Lapangan Volve, Norwegia**

Repositori ini berisi implementasi teknis untuk **Tugas Besar Kelompok WI2002 Literasi Data dan Intelegensi Artifisial**. Proyek ini menerapkan siklus analisis data **PPDAC** (Problem, Plan, Data, Analysis, Conclusion) untuk memulihkan data operasional sumur yang hilang menggunakan algoritma *Machine Learning*.

## Tim Pengembang

* **Ahmad Rafli Ramadhan** – *12225123 - Machine Learning Engineer*
* **[Nama Anggota 2]** – *[NIM] - [Peran]*
* **[Nama Anggota 3]** – *[NIM] - [Peran]*
* **[Nama Anggota 4]** – *[NIM] - [Peran]*

## Konteks Keteknikan & Latar Belakang

Pada operasi hulu migas, ukuran *choke* (`AVG_CHOKE_SIZE_P`) merupakan parameter kontrol manual yang krusial untuk mengendalikan laju produksi dan tekanan reservoir. Tugas ini mensimulasikan skenario di mana data historis tersebut hilang sebesar **10-12%** akibat kegagalan sensor atau ketidakteraturan pelaporan.

Mengingat data ini bersifat wajib untuk pelaporan resmi ke otoritas seperti **NPD** (*Norwegian Petroleum Directorate*), proyek ini menggunakan *Machine Learning* murni sebagai alat **restorasi/imputasi data historis**, bukan untuk memprediksi strategi operasional di masa depan.

## Variabel Model (Fitur & Target)

Proyek ini mengandalkan data sensor dan produksi harian.

* **Target Prediksi (Y):** `AVG_CHOKE_SIZE_P` (Bukaan choke dalam persentase)
* **Fitur Independen (X):** * `BORE_OIL_VOL` (Volume Minyak)
* `BORE_GAS_VOL` (Volume Gas)
* `BORE_WAT_VOL` (Volume Air)
* `AVG_DOWNHOLE_PRESSURE` (Tekanan Dasar Sumur)
* `AVG_WELLHEAD_PRESSURE` (Tekanan Kepala Sumur)
* `AVG_TEMPERATURE` (Suhu Fluida)



## Struktur Data & Repositori

Proyek ini menggunakan dataset produksi harian dari Lapangan Volve yang diolah melalui 6 tahapan *notebook* yang merepresentasikan bab-bab dalam laporan akhir.

### Pohon Direktori

```text
/
├── data/
│   ├── raw/           
│   │   └── Volve Production Data_Final.xlsx     # Dataset mentah awal
│   ├── processed/     
│   │   ├── volve_train_clean.csv                # Data utuh hasil cleansing (Bahan AI)
│   │   ├── volve_impute_clean.csv               # Data bolong target imputasi
│   │   └── Volve_Dataset_Akhir_Sempurna.csv     # Hasil akhir restorasi
│   └── metadata/      
│       ├── Deskripsi Variabel.csv
│       └── Panduan Kualitas Data.csv
├── notebooks/
│   ├── 01_Data_Cleansing.ipynb                  # Penanganan anomali fisik & sensor
│   ├── 02_Statistik_Deskriptif.ipynb            # Perhitungan min, max, mean, std
│   ├── 03_Visualisasi_Data.ipynb                # Pembuatan 8 grafik (PPDAC)
│   ├── 04_Analisis_Korelasi.ipynb               # Heatmap korelasi Pearson
│   ├── 05_Transformasi_Data.ipynb               # Normalisasi dengan Min-Max Scaler
│   └── 06_Machine_Learning_dan_Imputasi.ipynb   # Training, Evaluasi & Target Imputasi
├── models/            
│   ├── best_rf_model.pkl                        # Model ML pemenang
│   └── scaler.pkl                               # Alat normalisasi data
├── reports/                                     # Dokumen Laporan, Executive Summary, Poster
├── README.md
└── requirements.txt

```

## Alur Kerja (Workflow) PPDAC

1. **Bab 5: Data Cleansing (Data):** Membersihkan data mentah dari *outlier*, *missing values*, duplikasi, dan inkonsistensi fisik keteknikan (misal: BHP < WHP). Dataset dipecah menjadi data *training* dan target *impute*.
2. **Bab 6: Statistik Deskriptif (Analysis):** Menjabarkan rentang nilai dan distribusi dasar setiap variabel produksi harian.
3. **Bab 7: Visualisasi Data (Analysis):** Menyajikan 8 grafik komprehensif yang mencakup hubungan (Scatter Plot), komparasi (Boxplot), tren (Time-Series), dan hierarki (Pie Chart) parameter operasional.
4. **Bab 8: Analisis Korelasi (Analysis):** Memvalidasi hubungan logis antar parameter sensor melalui *Heatmap* Matriks Korelasi Pearson.
5. **Bab 9: Transformasi Data (Preparation):** Menyetarakan skala antar variabel (normalisasi) menggunakan `MinMaxScaler` agar algoritma belajar secara optimal.
6. **Bab 10: Data Analytic & Imputasi (Conclusion):** * Melatih 3 model algoritma (*Linear Regression*, *Random Forest*, *XGBoost*).
* Mengevaluasi model menggunakan metrik **RMSE, MAE, dan R-squared ($R^2$)**.
* Mengekstrak *Feature Importance* untuk mengetahui parameter penentu bukaan *choke*.
* Mengeksekusi imputasi untuk menambal data yang hilang dan merekonstruksi dataset akhir.



## Hasil Utama (Key Findings)

*(Catatan: Bagian ini dapat Anda perbarui setelah seluruh eksekusi kode selesai)*

* **Model Terbaik:** [Misal: Random Forest / XGBoost] terbukti menjadi algoritma paling akurat untuk data sumur ini dengan nilai $R^2$ mencapai [Misal: 0.92].
* **Feature Importance:** Analisis menunjukkan bahwa parameter **[Misal: AVG_WELLHEAD_PRESSURE]** memiliki korelasi dan pengaruh paling dominan dalam penentuan bukaan *choke*.
* **Status Imputasi:** Sebanyak **[Isi jumlah baris]** baris data histori yang bolong telah berhasil direstorasi menjadi satu file utuh.

## Cara Penggunaan

1. Pastikan Anda memiliki lingkungan Python 3.8+ terinstal.
2. Lakukan instalasi seluruh pustaka yang diperlukan dengan menjalankan perintah di terminal:
```bash
pip install -r requirements.txt

```


3. Eksekusi *file Jupyter Notebook* secara berurutan mulai dari `01` hingga `06` untuk memproduksi hasil analisis dan memulihkan data.

---

*Proyek ini merupakan bagian dari kurikulum akademik Program Studi Teknik Perminyakan, Institut Teknologi Bandung, tahun 2026.*
