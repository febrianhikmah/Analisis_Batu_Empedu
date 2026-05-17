# Analisis Batu Empedu dengan Regresi Logistik

[![Open in Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1QUMRB6sHhw7-GsBh0n-b9-qGJntTw3V1?usp=sharing)

Project ini menganalisis faktor yang berhubungan dengan status batu empedu (`Gallstone Status`) menggunakan pendekatan statistik dan machine learning. Fokus utama project adalah membangun model regresi logistik yang dapat diinterpretasikan, sekaligus mengevaluasi performa prediksi secara lebih stabil.

## Ringkasan Project

Dataset berisi data pasien dengan target biner:

- `0`: tidak memiliki batu empedu
- `1`: memiliki batu empedu

Analisis dilakukan melalui beberapa tahap:

1. Pemeriksaan kualitas data
2. Statistik deskriptif
3. Uji bivariat
4. Deteksi multikolinearitas
5. Regresi logistik inferensial
6. Evaluasi model prediksi
7. Cross-validation
8. Interpretasi fitur

## Struktur File

| File | Deskripsi |
| --- | --- |
| `Analisis_batu_ampedu_REGLOG.ipynb` | Notebook utama analisis |
| `batu empedu.csv` | Dataset yang digunakan |
| `Statistika deskripsi.csv` | Ringkasan statistik deskriptif |
| `Uji Chi-Square Kategorikal.csv` | Hasil uji Chi-Square untuk variabel kategorikal |
| `Uji Mann-Whitney Numerik.csv` | Hasil uji Mann-Whitney untuk variabel numerik |
| `Korelasi Tinggi.csv` | Pasangan fitur dengan korelasi tinggi |
| `VIF Semua Fitur.csv` | Hasil Variance Inflation Factor untuk semua fitur |
| `Hasil Regresi Logistik Inferensial.csv` | Koefisien, p-value, odds ratio, dan confidence interval |
| `Uji Serentak Model Inferensial.csv` | Hasil Likelihood Ratio Test |
| `Uji Hosmer-Lemeshow.csv` | Uji kalibrasi model regresi logistik |
| `Cross Validation Logistic Regression.csv` | Evaluasi 5-fold cross-validation |
| `Feature Importance Logistic Regression.csv` | Koefisien fitur dari model prediksi |

## Dataset

Dataset terdiri dari:

- 319 baris
- 24 kolom
- Target: `Gallstone Status`
- Missing value: 0
- Duplikat: 0

Distribusi target relatif seimbang:

| Kelas | Jumlah |
| --- | ---: |
| 0 | 161 |
| 1 | 158 |

## Metodologi

### 1. Uji Bivariat

Variabel kategorikal diuji menggunakan Chi-Square. Variabel numerik diuji menggunakan Mann-Whitney U karena beberapa fitur klinis memiliki outlier dan tidak selalu berdistribusi normal.

### 2. Multikolinearitas

Multikolinearitas dicek menggunakan korelasi antarfitur dan Variance Inflation Factor (VIF). Beberapa fitur komposisi tubuh memiliki VIF sangat tinggi, sehingga model penuh tidak digunakan sebagai model inferensial utama.

Contoh fitur dengan VIF tinggi:

| Fitur | VIF |
| --- | ---: |
| Lean Mass (LM) (%) | 97.54 |
| Total Body Fat Ratio (TBFR) (%) | 92.29 |
| Visceral Fat Rating (VFR) | 18.42 |
| Visceral Fat Area (VFA) | 16.65 |
| Muscle Mass (MM) | 11.19 |
| Body Mass Index (BMI) | 10.08 |

### 3. Model Inferensial

Model inferensial menggunakan regresi logistik dengan fitur yang lebih ringkas agar estimasi lebih stabil dan model dapat konvergen.

Fitur yang digunakan:

- `Age`
- `Comorbidity`
- `Diabetes Mellitus (DM)`
- `Visceral Fat Rating (VFR)`
- `Vitamin D`

### 4. Model Prediksi

Untuk evaluasi prediksi, digunakan `Pipeline` dari scikit-learn agar preprocessing tidak menyebabkan data leakage. Data dibagi menggunakan stratified train-test split dan dievaluasi juga dengan 5-fold cross-validation.

## Hasil Utama

### Regresi Logistik Inferensial

Model inferensial berhasil konvergen.

Hasil Likelihood Ratio Test:

| Statistik | Nilai |
| --- | ---: |
| Likelihood Ratio Statistic | 53.146 |
| p-value | 3.139e-10 |
| Pseudo R2 | 0.120 |

Hasil Hosmer-Lemeshow:

| Statistik | Nilai |
| --- | ---: |
| Hosmer-Lemeshow Statistic | 3.207 |
| p-value | 0.921 |

Interpretasi:

- Secara bersama-sama, fitur dalam model berhubungan signifikan dengan status batu empedu.
- Nilai Hosmer-Lemeshow p-value > 0.05 menunjukkan tidak ada bukti kuat bahwa model tidak fit.
- Pseudo R2 sebesar 0.120 menunjukkan model menjelaskan sebagian variasi status batu empedu, tetapi masih ada faktor lain di luar dataset.

### Variabel Signifikan

| Variabel | p-value | Odds Ratio |
| --- | ---: | ---: |
| Vitamin D | 1.168e-09 | 0.434 |
| Diabetes Mellitus (DM) | 0.00498 | 3.601 |
| Comorbidity | 0.0282 | 0.489 |

Interpretasi ringkas:

- Vitamin D yang lebih tinggi berasosiasi dengan odds batu empedu yang lebih rendah.
- Diabetes Mellitus berasosiasi dengan odds batu empedu yang lebih tinggi.
- `Comorbidity` signifikan, tetapi arah hubungan perlu dibaca hati-hati sesuai definisi encoding variabel.

## Performa Prediksi

Hasil 5-fold cross-validation Logistic Regression:

| Metrik | Mean | Std |
| --- | ---: | ---: |
| Accuracy | 0.687 | 0.024 |
| F1-score | 0.674 | 0.035 |
| ROC-AUC | 0.761 | 0.031 |

Model memiliki performa klasifikasi sedang. ROC-AUC sekitar 0.76 menunjukkan model cukup mampu membedakan kelas positif dan negatif, tetapi belum cukup kuat untuk digunakan sebagai alat keputusan klinis langsung.

## Kesimpulan

Project ini lebih kuat sebagai analisis eksploratif dan inferensial untuk memahami faktor yang berasosiasi dengan batu empedu. Logistic Regression memberikan model yang mudah diinterpretasikan dan cukup stabil setelah masalah multikolinearitas ditangani.

Kesimpulan utama:

- Dataset bersih dan target seimbang.
- Model penuh memiliki masalah multikolinearitas, sehingga model inferensial dibuat lebih ringkas.
- Vitamin D dan Diabetes Mellitus menjadi variabel yang paling konsisten berhubungan dengan status batu empedu.
- Performa prediksi berada pada tingkat sedang.
- Model sebaiknya digunakan sebagai alat analisis awal, bukan sebagai dasar keputusan klinis langsung.

## Cara Menjalankan

Install library yang dibutuhkan:

```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels scikit-learn
```

Buka notebook:

```bash
jupyter notebook Analisis_batu_ampedu_REGLOG.ipynb
```

Atau jalankan melalui VS Code/Jupyter Lab, lalu pilih `Run All`.

## Catatan

Hasil analisis bersifat asosiasi, bukan kausalitas. Interpretasi medis perlu mempertimbangkan konteks klinis, definisi encoding variabel, dan validasi pada dataset eksternal.
