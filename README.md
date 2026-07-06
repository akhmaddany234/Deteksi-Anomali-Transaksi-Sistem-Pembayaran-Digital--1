# Deteksi Kecurangan Transaksi Keuangan Menggunakan Machine Learning

Proyek ini bertujuan untuk mendeteksi kecurangan (fraud) pada transaksi keuangan menggunakan algoritma Machine Learning, khususnya **XGBoost**. Proyek ini difokuskan pada dua jenis dataset transaksi yang berbeda:
1. **Dataset Kartu Kredit (Credit Card)**
2. **Dataset Mobile Money (PaySim)**

Proyek ini mencakup seluruh tahapan data science mulai dari Exploratory Data Analysis (EDA), Data Preprocessing, Feature Engineering, hingga pembuatan model, dan akhirnya di-deploy ke dalam sebuah web aplikasi interaktif berbasis **Streamlit**.

---

## 📁 Struktur Proyek

- **`data olah ipynb/`**
  Folder ini berisi Jupyter Notebook yang digunakan untuk proses analisis dan pembuatan model:
  - `data_preparation_CreditCard.ipynb`: Berisi proses EDA, preprocessing (seperti scaling dengan RobustScaler dan penanganan outlier/winsorization), dan pelatihan model XGBoost untuk dataset Credit Card.
  - `data_Preparation_paysim.ipynb`: Berisi proses EDA, feature engineering (seperti selisih saldo, rasio amount, dll.), dan pelatihan model XGBoost untuk dataset PaySim.
  
- **`app22.py`**
  File utama untuk menjalankan aplikasi web Streamlit. Aplikasi ini mengintegrasikan model Machine Learning yang telah dilatih untuk melakukan prediksi secara real-time maupun batch (via CSV).

- **`model di disimpan/`**
  Folder tempat model-model yang telah dilatih (file `.pkl`) disimpan, seperti:
  - `xgboost_fraud_model_CreditCard.pkl`
  - `xgboost_fraud_modep_paysim.pkl`
  - Scaler model (`scaler_ROBUS_amount.pkl`, `scaler_ROBUS_time.pkl`)

- **`requirements.txt`**
  File yang berisi daftar library atau dependensi Python yang dibutuhkan untuk menjalankan proyek ini.

- **`data mentah/` & `dataset/`**
  Folder yang menyimpan dataset mentah yang digunakan untuk pelatihan dan pengujian model.

---

## 🚀 Fitur Aplikasi (Streamlit)

Aplikasi web yang dibangun (pada file `app22.py`) memiliki beberapa fitur utama yang dapat diakses melalui menu navigasi di sidebar:

1. **🏠 Home**
   Halaman beranda yang memberikan ringkasan informasi mengenai model Credit Card dan PaySim yang digunakan, serta panduan cepat (quick start) cara menggunakan aplikasi.
   
2. **📊 Manual Prediction**
   Pengguna dapat memilih tipe dataset (Credit Card atau PaySim) dan memasukkan parameter transaksi secara manual. Sistem akan memproses input tersebut dan menampilkan:
   - *Risk Score* (Skor Risiko)
   - *Fraud Probability* (Probabilitas Kecurangan)
   - Keputusan akhir apakah transaksi tersebut tergolong normal atau **FRAUD**, lengkap dengan indikator analisis fitur.

3. **📈 Dashboard PaySim & 📉 Dashboard CreditCard**
   Halaman analitik yang menyajikan visualisasi data interaktif menggunakan Plotly. Pengguna dapat melihat tren, distribusi, dan metrik penting dari masing-masing dataset.

4. **📤 Upload CSV**
   Fitur yang memungkinkan pengguna untuk mengunggah dataset mereka sendiri dalam format CSV. Sistem akan melakukan analisis batch dan menampilkan preview data, statistik deskriptif, serta opsi untuk mengunduh hasil filter.

---

## 🧠 Detail Machine Learning

### 1. Model Credit Card
- **Algoritma:** XGBoost
- **Fitur:** Menggunakan 30 fitur termasuk fitur hasil reduksi dimensi (PCA) yaitu V1 hingga V28.
- **Preprocessing:** 
  - Waktu (`Time`) dan Nominal (`Amount`) di-scaling menggunakan **RobustScaler** untuk menangani nilai ekstrem (outlier).
  - Dilakukan *winsorization* pada fitur `Amount` untuk membatasi nilai outlier.

### 2. Model PaySim (Mobile Money)
- **Algoritma:** XGBoost
- **Fitur Engineering Ekstensif:** 
  - Selisih saldo pengirim dan penerima sebelum dan sesudah transaksi.
  - Rasio jumlah transaksi (`Amount`) terhadap saldo.
  - Identifikasi tipe transaksi berisiko tinggi (misalnya `TRANSFER` dan `CASH_OUT`).
  - Ekstraksi waktu (jam) dari fitur `step`.
- **Preprocessing:** One-hot encoding untuk fitur kategori tipe transaksi.

---

## ⚙️ Cara Menjalankan Aplikasi Secara Lokal

1. **Clone repositori ini atau unduh file proyek.**
2. **Pastikan Anda memiliki Python yang terinstal (disarankan versi 3.8+).**
3. **Buka terminal/command prompt dan arahkan ke direktori proyek ini.**
4. **Instal semua library yang dibutuhkan:**
   ```bash
   pip install -r requirements.txt
   ```
5. **Jalankan aplikasi Streamlit:**
   ```bash
   streamlit run app22.py
   ```
6. Aplikasi akan otomatis terbuka di browser default Anda (biasanya di `http://localhost:8501`).

---

## 📝 Catatan Tambahan
Pastikan seluruh file model `.pkl` (seperti `xgboost_fraud_model_CreditCard.pkl`, `xgboost_fraud_modep_paysim.pkl`, dll.) berada pada direktori yang sama dengan `app22.py` atau path yang disesuaikan dalam kode sebelum menjalankan aplikasi. Jika model tidak ditemukan, aplikasi akan memberikan peringatan pada halaman *Home* dan fitur prediksi manual tidak akan berfungsi.
