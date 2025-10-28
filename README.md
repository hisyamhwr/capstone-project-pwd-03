# 🏠 California Housing Price Prediction  
**Capstone Project Module 3 — Machine Learning Regression**

Proyek ini bertujuan untuk mengembangkan model *machine learning* yang mampu memprediksi median harga rumah di California berdasarkan berbagai faktor demografis dan geografis.  
Dengan model ini, pelaku bisnis properti dapat melakukan estimasi harga secara lebih akurat, cepat, dan berbasis data — sehingga mendukung pengambilan keputusan strategis yang lebih efektif.

---

## 📂 **Daftar Isi**
1. [Business Understanding]
2. [Data Understanding]
3. [Data Preparation]
4. [Exploratory Data Analysis (EDA)]
5. [Modeling (Baseline Regression)]
6. [Modeling (Machine Learning)]
7. [Hyperparameter Tuning]
8. [Model Saving & Deployment]
9. [Perbandingan Sebelum dan Sesudah ML (Bisnis)]
10. [Kesimpulan dan Rekomendasi]

---

## 🧭 **1. Business Understanding**

### 🧩 Latar Belakang
Harga properti di California sangat bervariasi dan bergantung pada banyak faktor seperti lokasi geografis, pendapatan penduduk, usia bangunan, hingga kedekatan dengan pantai.  
Namun, kebanyakan perusahaan properti masih menggunakan metode konvensional berbasis perhitungan linier sederhana (misalnya regresi linier atau rata-rata historis) yang kurang mampu menangkap hubungan kompleks antar variabel.

Untuk itu, proyek ini dirancang guna mengembangkan model *machine learning regression* yang dapat **memprediksi harga median rumah di California secara lebih akurat dan adaptif.**

---

### **Rumusan Masalah**
1. Bagaimana memprediksi harga median rumah di California dengan memanfaatkan data demografis dan lokasi?  
2. Model *machine learning regression* apa yang menghasilkan akurasi terbaik?  
3. Bagaimana penerapan *hyperparameter tuning* memengaruhi performa model?  
4. Apa dampak penggunaan *machine learning* terhadap keputusan bisnis di sektor properti?

---

### 🎯 **Tujuan**
- Membangun model prediksi harga rumah berbasis *machine learning regression*.  
- Mengevaluasi performa beberapa algoritma (OLS, Decision Tree, Random Forest, KNN, XGBoost).  
- Meningkatkan performa model melalui *hyperparameter tuning*.  
- Memberikan insight bisnis berdasarkan hasil prediksi harga.

---

### 💼 **Manfaat Bisnis**
- Membantu **developer** menentukan harga jual yang realistis dan kompetitif.  
- Memberi **investor** alat bantu untuk menganalisis wilayah dengan potensi harga tertinggi.  
- Memungkinkan **pemerintah lokal** menggunakan data ini untuk kebijakan zonasi dan perencanaan kota berbasis data.

---

## 📊 **2. Data Understanding**

Dataset yang digunakan adalah **California Housing Dataset**, berisi data tentang harga rumah dan kondisi demografis di seluruh California.

| Atribut | Deskripsi |
|----------|------------|
| longitude | Koordinat geografis bujur |
| latitude | Koordinat geografis lintang |
| housing_median_age | Usia median bangunan |
| total_rooms | Total jumlah ruangan |
| total_bedrooms | Total kamar tidur |
| population | Jumlah populasi |
| households | Jumlah rumah tangga |
| median_income | Pendapatan median rumah tangga |
| ocean_proximity | Kedekatan dengan laut |
| median_house_value | Target variabel — harga median rumah |

**Jumlah data:** 20.640 baris  
**Jumlah fitur:** 10 kolom  

---

### ⚠️ **Permasalahan Awal**
- Missing value ditemukan pada kolom `total_bedrooms`.  
- Fitur `ocean_proximity` bertipe kategorikal → perlu di-*encode*.  
- Korelasi tinggi antara `total_rooms`, `total_bedrooms`, dan `households`.  
- Distribusi `median_income` dan `median_house_value` tidak normal (skewed).

---

## 🧹 **3. Data Preparation**

Tahapan utama:
1. Menghapus duplikat dan *outlier ekstrem*.  
2. Imputasi nilai kosong menggunakan median.  
3. Membuat fitur baru:
   - `rooms_per_household`  
   - `bedrooms_per_household`  
   - `population_per_household`  
4. Melakukan *One-Hot Encoding* untuk kolom kategorikal.  
5. Normalisasi fitur menggunakan `StandardScaler` agar semua variabel memiliki skala seragam.

---

### 🛠️ **Iterasi Permasalahan**
- Model awal gagal membaca `ocean_proximity` bertipe string → diselesaikan dengan encoding.  
- Multikolinearitas tinggi terdeteksi melalui *Variance Inflation Factor (VIF)* (>10) → fitur `longitude`, `latitude`, dan `rooms_per_household` dihapus.  

---

## 📈 **4. Exploratory Data Analysis (EDA)**

Hasil eksplorasi menunjukkan:
- Harga rumah tertinggi terkonsentrasi di area pesisir barat (latitude rendah, dekat laut).  
- `median_income` memiliki korelasi paling tinggi terhadap `median_house_value` (r = 0.68).  
- Usia rumah (`housing_median_age`) dan `population_per_household` juga berpengaruh, meski lebih kecil.  

Visualisasi menunjukkan hubungan non-linear antara fitur geografis dan harga rumah.

---

## ⚙️ **5. Modeling (Baseline Regression)**

### 🔹 **Model:** Ordinary Least Squares (OLS Regression)
Model baseline digunakan untuk memahami pengaruh linear antar variabel.

| Metrik | Nilai |
|--------|--------|
| **R²** | 0.614 |
| **RMSE** | 70,467 |
| **MAPE** | 0.300 |

### **Interpretasi:**
- Model mampu menjelaskan 61.4% variasi harga rumah.  
- Namun, error cukup tinggi (±70 ribu USD), menandakan model linear belum cukup representatif.  
- Terjadi *underfitting* karena hubungan antar fitur bersifat non-linear.

Model ini digunakan sebagai baseline sebelum beralih ke *machine learning models.*

---

## 🤖 **6. Modeling (Machine Learning)**

Beberapa algoritma diuji menggunakan *5-Fold Cross Validation*.

| Model | Mean RMSE | Mean MAPE | Mean R² |
|--------|------------|-----------|----------|
| **KNN** | **59,878** | **0.216** | **0.73** |
| Stacking (KNN Base) | 60,597 | 0.231 | 0.71 |
| Gradient Boosting | 60,184 | 0.242 | 0.70 |
| XGBoost | 61,520 | 0.252 | 0.66 |
| Random Forest | 65,909 | 0.271 | 0.64 |
| Linear Regression | 74,832 | 0.29 | 0.62 |

📈 **Model terbaik pada tahap ini:** KNN Regressor (RMSE 59,878, R² 0.73).

---

## 🧠 **7. Hyperparameter Tuning**

Dilakukan *GridSearchCV* pada model KNN dan Stacking-KNN untuk menemukan parameter optimal.

Kesimpulan sebelum memakai dan sesudah memakai machine learning
Sebelum Machine Learning, perusahaan hanya mengandalkan model linier sederhana dengan tingkat kesalahan hingga 30% dan penjelasan data 61%.
Setelah menerapkan Machine Learning (KNN), tingkat kesalahan menurun hingga 21% dan model mampu menjelaskan 74% variasi harga rumah.
Peningkatan akurasi ini berarti setiap estimasi harga kini lebih mendekati nilai pasar sebenarnya, menghemat biaya, dan meningkatkan potensi keuntungan secara signifikan.

Kesimpulan

KNN Regressor merupakan model terbaik dengan parameter optimal n_neighbors=7, weights='distance'.
Model ini mencapai performa tertinggi dengan R² = 0.742 dan MAPE = 0.21.
Model berhasil meningkatkan akurasi prediksi sebesar 18–21% dibandingkan pendekatan linier konvensional.

Rekomendasi

Tambahkan fitur baru seperti luas tanah, tipe bangunan, atau jarak ke pusat kota.
Gunakan ensemble learning seperti CatBoost untuk uji lanjutan.
Bangun dashboard prediksi harga rumah berbasis Streamlit/Flask untuk penggunaan bisnis.
