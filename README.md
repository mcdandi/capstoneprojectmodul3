# **Hotel Booking Cancellation Prediction**

## **Business Context**
Industri perhotelan menghadapi tantangan besar berupa tingginya tingkat pembatalan reservasi, yang berdampak negatif pada pendapatan dan efisiensi operasional. Pembatalan sering terjadi pada menit terakhir, sehingga sulit untuk menjual kembali kamar, terutama di musim puncak (_peak season_). Selain itu, kebijakan _free cancellation_ semakin memperburuk situasi.

Solusi berbasis data diperlukan untuk memprediksi pembatalan ini, sehingga hotel dapat:
- Mengelola kamar secara lebih efektif.
- Menerapkan kebijakan deposit yang tepat.
- Memberikan insentif kepada pelanggan dengan komitmen tinggi.
- Meningkatkan pengalaman pelanggan secara keseluruhan.

## **Problem Statement**
### **a) Pemangku Kepentingan**
- **Manajemen Hotel:** Mengelola pendapatan dan efisiensi operasional.
- **Tim Pemasaran:** Mengoptimalkan kampanye pemasaran berdasarkan pola pemesanan.
- **Tim Pengelola Pendapatan:** Menganalisis dan menyesuaikan strategi untuk memaksimalkan pendapatan.

### **b) Tantangan**
- Tingkat pembatalan tinggi menyebabkan kerugian pendapatan dan operasional.
- Kesulitan memanfaatkan data untuk memprediksi pembatalan secara akurat.
- Ketidakseimbangan antara strategi harga yang kompetitif dengan kebijakan fleksibel pelanggan.

### **c) Pentingnya Penyelesaian**
Mengatasi masalah pembatalan sangat penting untuk:
- **Meningkatkan Pendapatan:** Prediksi yang akurat membantu mengisi kamar kosong.
- **Efisiensi Operasional:** Memahami pola pembatalan untuk mengelola sumber daya.
- **Kepuasan Tamu:** Kebijakan berbasis prediksi meningkatkan pengalaman pelanggan.
- **Daya Saing Pasar:** Solusi berbasis data meningkatkan daya saing.

## **Goals**
Membangun model prediksi pembatalan berbasis _machine learning_ yang dapat:
1. Mengidentifikasi pemesanan dengan risiko tinggi dibatalkan.
2. Mengoptimalkan strategi pengelolaan kamar dan penetapan harga.
3. Meningkatkan efisiensi operasional sekaligus menjaga kepuasan pelanggan.

## **Analytics Approach**
### **Solusi yang Diusulkan**
Menggunakan analitik prediktif dan model _machine learning_ terawasi untuk memprediksi pembatalan berdasarkan data historis pemesanan.

### **Implementasi**
Model diintegrasikan ke dalam sistem manajemen hotel, memungkinkan identifikasi pemesanan berisiko tinggi dan langkah preventif seperti promosi atau fleksibilitas pemesanan.

### **Metric Evaluation**
Pendekatan klasifikasi dengan target:
- **0:** Tidak dibatalkan.
- **1:** Dibatalkan.

Kesalahan klasifikasi:
- **False Negative:** Kamar kosong tanpa penghasilan.
- **False Positive:** Kamar _overbooked_, menurunkan kepuasan pelanggan.

| Aktual \ Prediksi | Negatif (0/Tidak Cancel) | Positif (1/Cancel) |
| --- | --- | --- |
| Negatif (0/Tidak Cancel) | Aktual tidak cancel, Prediksi tidak cancel (TN)  | Aktual tidak cancel, Prediksi cancel (FP) |
| Positif (1/Cancel) | Aktual cancel, Prediksi tidak cancel (FN) | Aktual tidak cancel, Prediksi tidak cancel (TP) |

## **Business Calculation**

### **Kondisi Terburuk:**
Jika kita asumsikan ada 200 booking, dengan prediksi yang seimbang antara **cancel** dan **no cancel** (100 prediksi cancel dan 100 prediksi no cancel) dan kondisi aktual yang 100% no cancel (semua booking tidak dibatalkan), maka:

|                           | **Prediksi Book**     | **Prediksi Cancel** |
|---------------------------|-----------------------|---------------------|
| **Aktual Book**            | 100 x 100 USD         | 100 x (-75 USD)     |
| **Aktual Cancel**          | 0 x (-50 USD)         | 0 x 100 USD         |

**Income** = (100 x 100 USD) - (100 x 75 USD) = **2.500 USD**

### **Kondisi Ideal:**
Pada kondisi ideal, di mana prediksi dan kondisi aktualnya sama (100 cancel dan 100 no cancel), maka:

|                           | **Prediksi Book**     | **Prediksi Cancel** |
|---------------------------|-----------------------|---------------------|
| **Aktual Book**            | 100 x 100 USD         | 0 x (-75 USD)       |
| **Aktual Cancel**          | 0 x (-50 USD)         | 100 x 100 USD       |

**Income** = (100 x 100 USD) + (100 x 100 USD) = **20.000 USD**

---

## **Hasil Model:**

Berdasarkan perhitungan dengan model yang sudah dibangun, kita memperoleh hasil sebagai berikut:

|                           | **Prediksi Book**     | **Prediksi Cancel** |
|---------------------------|-----------------------|---------------------|
| **Aktual Book**            | 82 x 100 USD          | 18 x (-75 USD)      |
| **Aktual Cancel**          | 24 x (-50 USD)        | 76 x 100 USD        |

**Income** = (82 x 100 USD) - (18 x 75 USD) - (24 x 50 USD) + (76 x 100 USD) = **13.250 USD**

---

## **Case Sensitivity:**

Jika kita mempertimbangkan komposisi recall yang berbeda, berikut beberapa contoh kasus:

- **Recall 0 (No Cancel) = 83** dan **Recall 1 (Cancel) = 75**:

  **Income** = (83 x 100 USD) - (17 x 75 USD) - (23 x 50 USD) + (75 x 100 USD) = **13.375 USD**

- **Recall 0 (No Cancel) = 100** dan **Recall 1 (Cancel) = 58**:

  **Income** = (100 x 100 USD) - (0 x 75 USD) - (42 x 50 USD) + (58 x 100 USD) = **14.200 USD**

---
Sehingga apabila ada evaluation metric recall dengan jumlah yang sama, maka komposisi recall klasifikasi 0 (no cancel) akan membuat kita mendapatkan income lebih banyak. Hal ini juga agak kontradiktif dengan tujuan dibuat model yaitu untuk mendeteksi atau memprediksi cancel booking dengan benar. Hal ini bisa menjadi salah satu bukti pada riset awal yang mengatakan biasanya biaya kamar kosong akan lebih besar dibandingkan dengan overbook karena selaras dengan tujuan dibuat model yaitu memprediksi cancel booking.

**Hasil Model:**
Dengan _Random Forest Tuned SMOTE_, model mencapai:
- **Precision:** 71%.
- **Recall:** 76%.
- **False Positive Rate:** 18%.
- **False Negative Rate:** 24%.

## **Kesimpulan**
Model ini membantu hotel mengurangi pembatalan, meningkatkan pendapatan, dan efisiensi operasional. Namun, ada beberapa batasan seperti waktu komputasi untuk _hyperparameter tuning_ dan ketidakseimbangan data.

## **Rekomendasi**
1. **Kumpulkan data tambahan:** Data berkualitas tinggi akan meningkatkan performa model.
2. **Tambahkan fitur baru:** Fitur seperti _Average Daily Rate_ (ADR) atau jumlah hari antara pemesanan dan kedatangan.
3. **Coba model dan teknik lain:** Misalnya, Naive Bayes, CatBoost, atau ADASYN untuk resampling.
4. **Tingkatkan perhitungan bisnis:** Bandingkan biaya kamar kosong dengan _overbooking_ untuk keputusan strategis yang lebih baik.

---
