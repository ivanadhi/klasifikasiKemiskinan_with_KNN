# Proyek Akhir Data Mining: Klasifikasi Kemiskinan di Indonesia

## **Klasifikasi Kemiskinan Kabupaten/Kota di Indonesia Berdasarkan Indeks Kemiskinan Multidimensi (IKM) Menggunakan K-Nearest Neighbor (KNN)**

**Anggota Kelompok 3:**
- Adam Havenia Pratama (181221006)
- Ivan Adhipradana Putera (181221052)

---

## Deskripsi Proyek

Proyek ini bertujuan untuk membangun model klasifikasi untuk menentukan tingkat kemiskinan (Miskin atau Tidak Miskin) di berbagai kabupaten/kota di Indonesia. Model ini menggunakan algoritma **K-Nearest Neighbors (KNN)** yang diimplementasikan dari awal (tanpa *library*). Analisis didasarkan pada **Indeks Kemiskinan Multidimensi (IKM)** yang mencakup berbagai indikator sosial-ekonomi.

Tujuan utamanya adalah untuk mengidentifikasi faktor-faktor yang paling berpengaruh dalam menentukan status kemiskinan suatu wilayah.

## Sumber Data

Dataset yang digunakan merupakan gabungan dari dua sumber:
1.  **Kaggle**: [Klasifikasi Tingkat Kemiskinan di Indonesia](https://www.kaggle.com/datasets/ermila/klasifikasi-tingkat-kemiskinan-di-indonesia).
2.  **Badan Pusat Statistik (BPS)**: Data pendukung untuk Indeks Kemiskinan Multidimensi (IKM).

Indikator (fitur) yang digunakan dalam analisis ini antara lain:
- Persentase Penduduk Miskin (P0)
- Rata-rata Lama Sekolah Penduduk 15+
- Pengeluaran per Kapita
- Indeks Pembangunan Manusia (IPM)
- Umur Harapan Hidup
- Persentase Akses Sanitasi Layak
- Persentase Akses Air Minum Layak
- Tingkat Pengangguran Terbuka (TPT)
- Tingkat Partisipasi Angkatan Kerja (TPAK)
- PDRB Atas Dasar Harga Konstan (ADHK)

## Metodologi

Proyek ini dilaksanakan melalui beberapa tahapan utama, mulai dari pembersihan data hingga evaluasi model.

### 1. Eksplorasi dan Pembersihan Data (EDA)
- **Identifikasi Masalah**: Ditemukan bahwa banyak kolom numerik yang terbaca sebagai tipe data *object* karena penggunaan koma (`,`) sebagai pemisah desimal.
- **Penanganan Nilai Hilang**: Setiap kolom memiliki **485 nilai yang hilang** (*missing values*).
- **Penanganan Duplikasi**: Ditemukan **484 baris data duplikat**, yang kemungkinan besar berasal dari baris kosong.
- **Aksi**: Nilai yang hilang dan data duplikat dihapus menggunakan metode `dropna()`.

### 2. Preprocessing Data
- **Konversi Tipe Data**: Mengubah tipe data kolom yang relevan dari *object* menjadi *float* dengan mengganti koma menjadi titik.
- **Normalisasi**: Menerapkan **Min-Max Scaler** untuk mengubah semua nilai fitur ke dalam rentang [0, 1]. Ini dilakukan agar semua fitur memiliki skala yang seragam dan tidak mendominasi perhitungan jarak.
- **Encoding Fitur Kategorikal**: Mengubah fitur kategorikal (`Provinsi` dan `Kab/Kota`) menjadi representasi numerik menggunakan **One-Hot Encoding**.

### 3. Reduksi Dimensi
- **Principal Component Analysis (PCA)**: Karena jumlah fitur yang sangat banyak setelah proses encoding, PCA diterapkan untuk mengurangi dimensi data menjadi **10 komponen utama**. Langkah ini membantu menyederhanakan model dan mengurangi risiko *overfitting*.

### 4. Pemodelan
- **Algoritma**: Menggunakan **K-Nearest Neighbors (KNN)** yang dibangun secara manual untuk melakukan klasifikasi.
- **Pembagian Data**: Data dibagi menjadi data latih (80%) dan data uji (20%) dengan metode `train_test_split`. Pembagian dilakukan secara *stratified* untuk menjaga proporsi kelas target yang tidak seimbang.
- **Penentuan K Optimal**: Nilai `k` terbaik dicari menggunakan validasi silang manual (5-fold) pada rentang `k` dari 3 hingga 20. Hasilnya, **`k=3`** memberikan akurasi rata-rata tertinggi.

## Hasil dan Evaluasi

Model KNN dengan **k=3** menunjukkan performa yang **sempurna** pada data uji.

- **Akurasi**: 100.00%
- **Presisi**: 100.00%
- **Recall**: 100.00%
- **F1-Score**: 100.00%
- **ROC-AUC**: 100.00%

**Confusion Matrix:**
| 91 | 0 |
| :---| :---|
| 0 | 12 |

Matriks ini menunjukkan bahwa dari 103 data uji, model berhasil memprediksi semua kelas dengan benar tanpa ada kesalahan (baik *False Positive* maupun *False Negative*).

## Fitur Paling Berpengaruh

Untuk mengetahui fitur mana yang paling penting, analisis *feature importance* dilakukan menggunakan metode permutasi pada komponen utama (PC) hasil PCA.

| Fitur | Tingkat Kepentingan (Importance) |
| :---- | :------------------------------- |
| **PC1** | **0.194404** |
| PC2   | 0.006813                         |
| PC6   | 0.001946                         |
| PC5   | 0.000243                         |
| Lainnya | 0.000000                         |

Dari hasil di atas, terlihat jelas bahwa **PC1 (Komponen Utama 1)**, yang sebagian besar merepresentasikan **Persentase Kemiskinan**, adalah faktor yang paling dominan dalam menentukan klasifikasi. Fitur ekonomi seperti **PDRB (PC2)** dan **Pengeluaran per Kapita (PC6)** memiliki pengaruh sekunder yang jauh lebih kecil.

## Kesimpulan dan Rekomendasi

1.  **Faktor Utama Kemiskinan**: Tingkat kemiskinan itu sendiri (`Persentase Kemiskinan`) adalah prediktor paling kuat untuk klasifikasi status kemiskinan.
2.  **Faktor Pendukung**: Aspek ekonomi makro (`PDRB`) dan mikro (`Pengeluaran per Kapita`) memiliki peran pendukung namun tidak signifikan dibandingkan persentase kemiskinan.
3.  **Rekomendasi Kebijakan**: Kebijakan penanggulangan kemiskinan sebaiknya difokuskan pada program-program yang secara langsung menekan persentase penduduk miskin, didukung oleh upaya peningkatan PDRB dan daya beli masyarakat.
4.  **Analisis Lanjutan**: Disarankan untuk melakukan analisis korelasi antara fitur-fitur untuk memahami mengapa kontribusi beberapa fitur (seperti IPM, Pendidikan, dll.) sangat rendah setelah reduksi dimensi.
