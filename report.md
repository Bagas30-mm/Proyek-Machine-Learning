# Laporan Proyek Machine Learning - Fertilizer Recommendation System - Bagas Rizky Ramadhan

## Project Overview

Proyek ini mengimplementasikan sistem rekomendasi pupuk berbasis konten dengan menggunakan teknik machine learning. Sistem ini dibuat untuk memberikan rekomendasi pupuk yang tepat berdasarkan kondisi tanah, jenis tanaman, dan parameter lingkungan. Dengan menggunakan data 3.100 record yang mengandung informasi lingkungan dan tanah, sistem bertujuan untuk membantu petani dan profesional agrikultur dalam memilih pupuk yang sesuai.

**Mengapa Masalah Ini Penting?**

- Pemupukan yang tidak tepat dapat menurunkan produktivitas lahan.
- Meminimalkan penggunaan pupuk berlebih sehingga mendukung pertanian yang ramah lingkungan.
- Pendekatan berbasis data dapat meningkatkan akurasi rekomendasi dibanding metode konvensional.

**Referensi dan Riset Terkait:**

1. Aditya, S. K., & Suryawanshi, P. (2020). _Fertilizer Recommendation System for Precision Agriculture using Machine Learning_. International Journal of Computer Applications, 176(32), 1-5.
2. Manning, C. D., Raghavan, P., & Schütze, H. (2008). _Introduction to Information Retrieval_. Cambridge University Press.

## Business Understanding

**Problem Statements:**

1. Petani sering kesulitan menentukan jenis pupuk yang paling sesuai dengan kondisi tanah dan tanaman yang dimiliki.
2. Penggunaan pupuk yang tidak tepat dapat menyebabkan penurunan hasil panen dan kerusakan lingkungan akibat residu kimia.
3. Kurangnya sistem rekomendasi berbasis data yang mudah digunakan untuk membantu pengambilan keputusan pemupukan secara presisi.

**Goals:**

1. Mengembangkan sistem rekomendasi pupuk berbasis machine learning yang mempertimbangkan kondisi tanah, tanaman, dan lingkungan.
2. Meningkatkan akurasi pemilihan pupuk sehingga hasil panen optimal dan dampak negatif terhadap lingkungan dapat diminimalkan.
3. Menyediakan solusi rekomendasi yang mudah diakses dan digunakan oleh petani serta profesional agrikultur.

**Solution Approach:**

- **Content-Based Filtering:** Menggabungkan informasi tanah, tanaman, dan kondisi lingkungan ke dalam satu fitur string untuk representasi data.
- **TF-IDF Vectorization:** Mengubah feature string menjadi vektor numerik sehingga dapat dihitung kemiripannya menggunakan cosine similarity.
- **Ranking Rekomendasi:** Mengurutkan record berdasarkan nilai cosine similarity dan menampilkan 5 rekomendasi teratas.
- **Evaluasi Sistem:** Menggunakan metrik Precision@K, Mean Reciprocal Rank (MRR), dan Normalized Discounted Cumulative Gain (NDCG) untuk mengukur performa rekomendasi.

## Data Understanding

Dataset yang digunakan memiliki 3.100 record dengan delapan fitur numerik:

- **Temperature**: Suhu lingkungan pada saat pengambilan data, biasanya diukur dalam derajat Celsius.
- **Moisture**: Kadar kelembaban tanah, yang mempengaruhi pertumbuhan tanaman.
- **Rainfall**: Jumlah curah hujan yang diterima area tersebut, diukur dalam milimeter.
- **PH**: Tingkat keasaman atau kebasaan tanah, yang berpengaruh pada ketersediaan unsur hara.
- **Nitrogen**: Kandungan unsur nitrogen dalam tanah, penting untuk pertumbuhan daun dan batang.
- **Phosphorous**: Kandungan unsur fosfor dalam tanah, berperan dalam perkembangan akar dan pembungaan.
- **Potassium**: Kandungan unsur kalium dalam tanah, membantu ketahanan tanaman terhadap penyakit.
- **Carbon**: Kandungan karbon organik dalam tanah, berfungsi meningkatkan kesuburan dan struktur tanah.

Selain itu, terdapat fitur kategorikal seperti:

- **Soil:** Jenis tanah (contoh: Loamy Soil, Peaty Soil, Acidic Soil)
- **Crop:** Jenis tanaman (contoh: rice, wheat, corn)

Data awal telah diproses dengan menghapus kolom yang tidak diperlukan (misalnya, kolom 'Remark') dan penambahan kolom ID.

## Data Preparation

Proses persiapan data meliputi:

1. **Konversi Nilai Numerik ke Kategorikal:**  
   Setiap fitur numerik diubah menjadi tiga kategori (low, medium, high) untuk memudahkan user dengan menggunakan quantile (0.33 dan 0.67) sebagai batasan. Fungsi `categorize_value` melakukan proses ini untuk tiap kolom.
2. **Pembuatan Feature String:**  
   Menggabungkan informasi tentang `Soil`, `Crop`, dan nilai kategorikal masing-masing parameter lingkungan ke dalam satu string untuk memudahkan user dalam memilih jenis pupuk. Contohnya, informasi `high_temperature`, `low_moisture`, dan seterusnya digabung agar siap untuk diubah ke dalam representasi vektor menggunakan TF-IDF.

3. **TF-IDF Vectorization:**  
   Vektorisasi dilakukan dengan modul scikit-learn menggunakan `TfidfVectorizer` untuk mengubah feature string menjadi bentuk numerik, yang nantinya digunakan untuk menghitung cosine similarity.

## Modeling

Model rekomendasi dibangun dengan pendekatan content-based filtering. Berikut adalah langkah utamanya:

1. **Membangun Query:**  
   Fungsi `recommend_fertilizer` menerima parameter wajib seperti jenis tanah `soil` dan jenis tumbuhan `crop`, serta parameter opsional untuk kondisi lingkungan. Parameter yang diberikan akan diubah menjadi string query yang disesuaikan.
2. **Menghitung Cosine Similarity:**  
   Setelah mengubah query menjadi vektor menggunakan TF-IDF, nilai cosine similarity dihitung antara query pengguna dan semua record pada dataset vektor.
3. **Ranking dan Rekomendasi:**  
   Record diurutkan berdasarkan nilai cosine similarity yang dihitung. Fungsi ini kemudian mengembalikan 5 rekomendasi teratas, yang mencakup informasi seperti ID, nama pupuk, skor kemiripan, jenis tanah, dan jenis tanaman.

## Evaluation

Evaluasi sistem dilakukan menggunakan beberapa metrik:

- **Precision@5:** Rasio rekomendasi yang tepat pada 5 rekomendasi teratas.
- **Mean Reciprocal Rank (MRR):** Menilai seberapa cepat (peringkat ke berapa) rekomendasi yang benar muncul.
- **Normalized Discounted Cumulative Gain (NDCG):** Metrik yang mengukur kualitas ranking rekomendasi dengan memberikan bobot lebih pada peringkat yang lebih tinggi.

Proses evaluasi dilakukan dengan:

- Membuat beberapa test case yang diambil secara acak dari dataset.
- Menghitung metrik evaluasi untuk setiap test case.
- Menampilkan hasil evaluasi berupa DataFrame dan mencetak metrik secara keseluruhan.

Hasil evaluasi pada 500 sample:

- **Precision@5:** 0.9920
- **Mean Reciprocal Rank (MRR):** 0.9344
- **Normalized Discounted Cumulative Gain (NDCG):** 0.924

Contoh kode evaluasi terdapat pada fungsi `evaluate_recommendation_system`, dimana test case dibuat dengan memilih record acak dan menguji output sistem rekomendasi.

## Usage Example

Untuk menggunakan sistem rekomendasi, panggil fungsi `recommend_fertilizer` dengan parameter yang diinginkan. Misal:

```python
recommendations = recommend_fertilizer(
    soil="Loamy Soil",
    crop="rice",
    temperature_level="high",
    moisture_level="high"
)
```
