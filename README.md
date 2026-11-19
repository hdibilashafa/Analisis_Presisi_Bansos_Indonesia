# Analisis Presisi Bansos Ekonomi Indonesia berbasis Data Science

![Python](https://img.shields.io/badge/Python-3-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626)
![pandas](https://img.shields.io/badge/pandas-Data_Wrangling-150458)
![scikit-learn](https://img.shields.io/badge/scikit--learn-K--Means-F7931E)
![Folium](https://img.shields.io/badge/Folium-Peta_Interaktif-77B829)

Analisis untuk **mengelompokkan provinsi di Indonesia berdasarkan tingkat prioritas bantuan sosial (bansos)** menggunakan **K-Means Clustering** pada 10 indikator sosial-ekonomi. Hasilnya ditampilkan sebagai peta interaktif beserta rekomendasi program bansos untuk setiap kelompok.

---

## Tujuan

Bansos yang tepat sasaran tidak cukup dilihat dari angka kemiskinan saja. Proyek ini menggabungkan beberapa dimensi (kemiskinan, ketimpangan, pengangguran, kesehatan, layanan dasar, risiko bencana, dan ketahanan pangan) untuk memetakan provinsi mana yang membutuhkan intervensi paling mendesak dan jenis program apa yang cocok.

---

## Indikator yang Digunakan

Data per provinsi, tahun **2021**, terdiri dari 10 indikator:

| No | Indikator | Arah |
|---|---|---|
| 1 | Persentase penduduk miskin | Makin tinggi makin buruk |
| 2 | Jumlah penduduk miskin (ribu jiwa) | Makin tinggi makin buruk |
| 3 | Indeks kedalaman kemiskinan (P1) | Makin tinggi makin buruk |
| 4 | Gini ratio | Makin tinggi makin buruk |
| 5 | Tingkat pengangguran terbuka (TPT) | Makin tinggi makin buruk |
| 6 | Indeks Pembangunan Manusia (IPM) | Makin tinggi makin baik |
| 7 | Prevalensi balita stunting | Makin tinggi makin buruk |
| 8 | Indeks layanan dasar (komposit: air minum, sanitasi dasar, fasilitas kesehatan dasar) | Makin tinggi makin baik |
| 9 | Indeks risiko bencana | Makin tinggi makin buruk |
| 10 | Indeks ketahanan pangan | Makin tinggi makin baik |

---

## Alur Analisis

1. **Manajemen data**: tiap dataset dibersihkan (hapus kolom tidak perlu, ubah nama kolom, samakan penulisan nama provinsi)
2. **Audit ejaan provinsi**: memastikan nama provinsi konsisten di seluruh 10 file sebelum digabung
3. **Merge**: 10 tabel digabung menjadi satu master data per provinsi
4. **Inversi**: indikator yang "makin tinggi makin baik" (IPM, layanan dasar, ketahanan pangan) dibalik dengan `nilai maksimum − nilai`, sehingga semua indikator searah (nilai tinggi = kondisi buruk)
5. **Normalisasi**: Min-Max Scaling ke rentang 0–1
6. **Penentuan jumlah kluster**: Metode Elbow (K = 1–10), dipilih **K = 4**
7. **Clustering**: K-Means (`k-means++`, `n_init=10`, `random_state=42`)
8. **Visualisasi**: peta choropleth interaktif dengan Folium dan panel rekomendasi per kluster

---

## Hasil Pengelompokan

Empat kelompok prioritas bansos untuk 34 provinsi:

| Prioritas | Provinsi | Karakteristik utama | Rekomendasi program |
|---|---|---|---|
| **Sangat Tinggi** | Nusa Tenggara Timur, Maluku, Papua Barat, Papua | Skor tertinggi pada persentase dan kedalaman kemiskinan, stunting, serta ketertinggalan IPM; risiko bencana tinggi | PKH intensif + sembako, BLT darurat desa, program stunting 1000 HPK, air bersih dan sanitasi, bedah rumah, padat karya desa |
| **Tinggi** | Aceh, Sumatera Utara, Sumatera Barat, Riau, Jambi, Sumatera Selatan, Bengkulu, Lampung, Kep. Bangka Belitung, NTB, Kalimantan (Barat, Tengah, Selatan, Utara), Sulawesi (Tengah, Selatan, Tenggara, Barat), Gorontalo, Maluku Utara | Kemiskinan sedang, skor risiko bencana paling tinggi di antara semua kluster | PKH + BPNT, padat karya desa, bantuan benih dan pupuk, posyandu intensif |
| **Sedang** | Jawa Barat, Jawa Tengah, Jawa Timur | Jumlah penduduk miskin absolut tertinggi, ketimpangan dan pengangguran tinggi | PKH tepat sasaran, Prakerja dan UMKM, bedah rumah, urban farming |
| **Rendah** | Kepulauan Riau, DKI Jakarta, DI Yogyakarta, Banten, Bali, Kalimantan Timur, Sulawesi Utara | Skor kemiskinan terendah, namun ketimpangan dan pengangguran masih relatif tinggi | PKH ketat + graduasi, modal usaha, BPJS lengkap, asuransi iklim |

> Nilai pada kolom karakteristik adalah **skor hasil normalisasi 0–1**
---

## Cara Menjalankan

### 1. Clone repository

```bash
git clone https://github.com/hdibilashafa/Analisis_Presisi_Bansos_Indonesia.git
cd Analisis_Presisi_Bansos_Indonesia
```

### 2. Install dependensi

```bash
pip install pandas scikit-learn matplotlib folium geopandas jupyter
```

### 3. Siapkan dataset

Letakkan 10 file CSV berikut **di folder yang sama dengan notebook**:

```
(1) Persentase Penduduk Miskin.csv
(2) Jumlah Penduduk Miskin.csv
(3) Indeks Kedalaman Kemiskinan (P1).csv
(4) Gini Rasio.csv
(5) Tingkat Pengangguran Terbuka.csv
(6) Indeks Pembangunan Manusia.csv
(7) Balita Stunting.csv
(8) Akses Layanan Dasar.csv
(9) Indeks Risko Bencana.csv
(10) Indeks Ketahanan Pangan.csv
```

### 4. Jalankan notebook

```bash
jupyter notebook
```

Buka file `.ipynb` lalu jalankan semua sel secara berurutan (**Run All**). Sel terakhir membutuhkan koneksi internet untuk mengunduh data batas provinsi (GeoJSON) yang dipakai pada peta.

### Output yang dihasilkan

| File | Isi |
|---|---|
| `FIX (1) ... (10).csv` | Dataset yang sudah dibersihkan |
| `DATA_MASTER_FINAL.csv` | Gabungan 10 indikator per provinsi |
| `DATA_PREPROCESSED.csv` | Data setelah inversi dan normalisasi |
| `HASIL_KLUSTER_PROVINSI.csv` | Data master + label kluster |

---

## Tech Stack

Python · pandas · scikit-learn (K-Means, MinMaxScaler) · Matplotlib · GeoPandas · Folium · Jupyter Notebook

---

## Keterbatasan

- Data yang dipakai adalah tahun **2021**, sehingga hasil belum mencerminkan kondisi terbaru
- Empat provinsi baru di Papua (Papua Selatan, Papua Tengah, Papua Pegunungan, Papua Barat Daya) tidak dimasukkan karena datanya belum tersedia, sehingga analisis mencakup 34 provinsi
- Analisis dilakukan di tingkat provinsi, sedangkan kantong kemiskinan bisa berbeda jauh antar kabupaten/kota di dalam satu provinsi
- Semua indikator diberi bobot sama
- Hasil clustering adalah alat bantu untuk menentukan prioritas, bukan pengganti data penerima manfaat (DTKS/DTSEN)

---

## Author

**Nabilah Shafa**
Teknik Informatika, Universitas Komputer Indonesia (UNIKOM)
