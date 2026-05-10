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

Proyek ini menggunakan dataset produksi harian dari Lapangan Volve yang telah melalui tahap pra-pemrosesan mandiri.

### Metadata & Panduan

* `Deskripsi Variabel.csv`: Penjelasan lengkap setiap atribut sensor dan satuannya.
* `Panduan Kualitas Data.csv`: Dokumentasi standar kebersihan dan integritas data yang digunakan.

### Dataset Utama

* **Data Train** (`volve_train_clean.csv`): Baris data dengan nilai *choke* yang tersedia, digunakan eksklusif untuk melatih dan menguji model ML.
* **Data Impute** (`volve_impute_clean.csv`): Baris data dengan nilai *choke* yang hilang (*null*), menjadi target akhir untuk dipulihkan oleh model.

### Pohon Direktori

```text
/
├── data/
│   ├── raw/           # Dataset mentah Lapangan Volve
│   ├── processed/     # Dataset bersih (volve_train_clean, volve_impute_clean)
│   └── metadata/      # File deskripsi variabel dan panduan kualitas data
├── notebooks/
│   ├── 01_EDA_and_Correlation.ipynb     # Statistik deskriptif & Matriks Korelasi Pearson
│   ├── 02_Modeling_and_Evaluation.ipynb # Pelatihan model & Evaluasi metrik
│   └── 03_Imputation_and_Export.ipynb   # Proses pengisian data hilang & Ekspor akhir
├── models/            # Penyimpanan model terbaik (.pkl) dan scaler
├── reports/           # Laporan PDF, Executive Summary, dan Poster
└── README.md
└── requirements.txt

```

## Alur Kerja (Workflow) PPDAC

1. **Fase Eksplorasi (Notebook 01):** Melakukan Analisis Data Eksploratif (EDA) untuk mencari pola dan hubungan logis keteknikan. Hasil utamanya adalah Matriks Korelasi Pearson (Heatmap) dan 8 visualisasi statistik yang membandingkan tekanan, volume, dan pengaturan *choke*.
2. **Fase Pelatihan Kecerdasan Buatan (Notebook 02):**
* **Pra-pemrosesan Lanjutan:** Menerapkan metode *Backward Fill* dan *Forward Fill* (`bfill.ffill`) untuk memastikan tidak ada *missing values* pada sensor fitur, serta menggunakan `MinMaxScaler` agar algoritma belajar tanpa bias skala angka.
* **Pemodelan & Evaluasi:** Membangun *Linear Regression*, *Random Forest*, dan *XGBoost*. Mengevaluasi performa ketiganya pada porsi data uji (20%) menggunakan metrik **RMSE, MAE, dan R-squared ($R^2$)**.
* **Feature Importance:** Mengekstraksi bobot setiap parameter untuk menjawab sensor mana yang paling berpengaruh terhadap perubahan *choke*.


3. **Fase Restorasi Data (Notebook 03):** Model dengan evaluasi terbaik dan alat normalisasi (*scaler*) dipanggil kembali untuk memprediksi dan mengisi nilai pada `AVG_CHOKE_SIZE_P` yang kosong. Dataset kemudian digabungkan utuh untuk dilaporkan.

## Hasil Utama (Key Findings)

*(Catatan: Bagian ini dapat Anda perbarui setelah *running* kode selesai)*

* **Model Terbaik:** [Misal: Random Forest / XGBoost] terbukti menjadi algoritma paling akurat untuk data sumur ini dengan nilai $R^2$ mencapai [Misal: 0.92].
* **Feature Importance:** Analisis menunjukkan bahwa parameter **[Misal: AVG_WELLHEAD_PRESSURE]** memiliki korelasi dan pengaruh paling dominan dalam penentuan bukaan *choke*.
* **Status Imputasi:** Sebanyak **[Isi jumlah baris]** baris data histori yang bolong telah berhasil direstorasi menjadi satu file utuh.

## Cara Penggunaan

1. Pastikan Anda memiliki lingkungan Python 3.8+ (seperti Anaconda, VS Code, atau Google Colab).
2. Lakukan instalasi seluruh pustaka yang diperlukan dengan menjalankan perintah:
```bash
pip install -r requirements.txt

```


*(Pustaka utama meliputi: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `xgboost`, `joblib`, `openpyxl`)*
3. Jalankan *file notebook* secara berurutan mulai dari `01` hingga `03` untuk mereproduksi hasil.

---

*Proyek ini merupakan bagian dari kurikulum akademik Program Studi Teknik Perminyakan, Institut Teknologi Bandung, tahun 2026.*
