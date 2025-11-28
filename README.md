# **Prediksi Emisi Tambang Menggunakan Stacking Ensemble**

## **1. Latar Belakang Project**
Industri pertambangan merupakan salah satu sektor dengan kontribusi emisi gas rumah kaca yang cukup besar. Dengan meningkatnya tuntutan terhadap pengurangan emisi dan kebutuhan pelaporan yang akurat, perusahaan tambang membutuhkan **model prediksi emisi CO₂e yang akurat, stabil, dan dapat diandalkan untuk data nyata (real-world data)**.

Dataset emisi tambang biasanya terdiri dari:

- Produksi ton batuan/ore  
- Konsumsi solar  
- Konsumsi listrik  
- Jam operasi  
- Jumlah alat berat  
- Emisi CO₂e  

Dataset ini memiliki karakteristik umum industri:

- Nilai **skewed**, tidak simetris  
- Banyak **outlier**  
- Multikolinearitas antar fitur  
- Missing values (NaN)  
- Hubungan non-linear antara fitur  

Sehingga dibutuhkan pendekatan machine learning yang:

✔ Robust  
✔ Tahan outlier  
✔ Dapat menangkap hubungan linear & non-linear  
✔ Stabil pada data unseen  
✔ Memiliki performa tinggi

---

## 🎯 **2. Tujuan & Objektif Project**

### **Tujuan Project**
Membangun model prediksi emisi CO₂e tambang yang:

- Akurat  
- Stabil  
- Dapat menggeneralisasi ke data baru  
- Siap digunakan untuk analisis emisi, compliance, dan carbon credit  

### **Objective Teknis**
1. Melakukan **EDA** untuk memahami karakteristik data.  
2. Menangani **missing value** dan outlier secara tepat.  
3. Menangani **multikolinearitas** (VIF & Heatmap).  
4. Melakukan **feature selection** yang relevan.  
5. Membangun **pipeline preprocessing + modeling**.  
6. Mencoba beberapa **baseline models**.  
7. Membangun model **Stacking Ensemble** sebagai model utama.  
8. Melakukan **evaluasi model lengkap (Train/Test + CV)**.  
9. Memilih model terbaik berdasarkan **R², MAE, RMSE, dan Cross Validation**.

---

## 🧪 **3. Exploratory Data Analysis (EDA)**

### **3.1. Cek Missing Value**
- Missing pada fitur input → diimputasi.  
- Missing pada target (emisi CO₂e) → **harus di-drop**  

### **3.2. Distribusi Fitur**
Dataset menunjukkan:

- Distribusi **tidak normal (skewed)**  
- Banyak nilai ekstrem (outlier)  

### **3.3. Heatmap Korelasi**
Menemukan korelasi ekstrim:

- `produksi_ton` ↔ `alat_berat` = **1.00**  
- Menandakan multikolinearitas ekstrem  

### **3.4. Multikolinearitas (VIF Test)**  
VIF sangat tinggi pada beberapa fitur → fitur dibuang:

- `alat_berat`  
- `jam_operasi`

---

## 🔧 **4. Train-Test Split Sebelum Imputasi & Scaling**
Dilakukan untuk mencegah **data leakage**, yaitu kondisi dimana model “melihat” informasi test saat preprocessing.  
Dengan split diawal:

✔ Preprocessor hanya belajar dari train set  
✔ Test tetap unseen  
✔ Evaluasi valid  

---

## 🧹 **5. Preprocessing Pipeline**
Pipeline digunakan agar proses:

- Konsisten  
- Bebas leakage  
- Siap deploy  
- Clean & reproducible  

### Pipeline berisi:
1. **Median Imputer**  
2. **RobustScaler** (tahan outlier)  
3. **Model**  

---

## 🤖 **6. Baseline Models**
Model baseline yang digunakan:

- Ridge Regression  
- ElasticNet  
- Huber Regressor  
- HistGradientBoostingRegressor (HGB)  

Dipilih karena mencakup **linear, robust, dan non-linear** model.

---

## 🧠 **7. Kenapa Stacking Ensemble?**
Karena setiap model punya kelebihan dan kekurangan.

Stacking menggabungkan kekuatan:

- Ridge → linear stabil  
- ElasticNet → linear fleksibel  
- Huber → tahan outlier  
- HGB → non-linear kuat  

Hasil ensemble lebih akurat dan stabil.

---

## 🧬 **8. Kenapa Final Estimator = Ridge?**
Prediksi base model cenderung kolinear.  
Ridge Regression sangat baik menangani collinearity.

Meta-model Ridge:

✔ Stabil  
✔ Tidak overfit  
✔ Pilihan terbaik untuk menggabungkan output model base  

---

## 📊 **9. Evaluasi Model**
Model Stacking menghasilkan:

- **R² Train:** 0.995  
- **R² Test:** 0.986  
- **MAE Test:** ~600  
- **RMSE Test:** ~900  

Sangat akurat & tidak overfit.

---

## 🔍 **10. Cross Validation (K-Fold)**

```
Stacking CV R2 = 0.9861 ± 0.00138
```

Interpretasi:

- Mean R² sangat tinggi  
- Variansi antar fold sangat kecil  

✔ Model stabil  
✔ Generalizable  
✔ Siap untuk data baru  

---

## 🏆 **11. Model Terbaik**
| Model | R² Test | CV Mean | CV Std |
|-------|---------|----------|--------|
| **Stacking Ensemble** | **0.986** | **0.9861** | **0.0013** |

🏅 **Pemenangnya adalah Stacking Ensemble**

---

## 🚀 **12. Conclusion**
Model prediksi emisi tambang berhasil dibangun dengan:

- Preprocessing yang tepat  
- Penghapusan multikolinearitas  
- RobustScaler + Median imputer  
- Baseline linear & non-linear  
- Stacking ensemble sebagai model akhir  

Model akurat, stabil, dan siap digunakan untuk:

✔ Carbon credit  
✔ Emission forecasting  
✔ Environmental reporting  
✔ Operational planning  
