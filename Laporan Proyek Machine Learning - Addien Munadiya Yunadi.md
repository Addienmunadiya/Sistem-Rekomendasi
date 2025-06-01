# Laporan Proyek Machine Learning - Sistem Rekomendasi Film 
#### Oleh : Addien Munadiya Yunadi
![Ilustrasi Sistem Rekomendasi](https://lirp.cdn-website.com/e0446a5f/dms3rep/multi/opt/Best+of+2022+SQUARE-1920w.png)
## Project Overview

Dalam era digital, informasi yang melimpah seringkali membuat pengguna kesulitan dalam memilih konten yang relevan dan sesuai dengan preferensi mereka (Fajriyansyah, 2021). Sistem rekomendasi hadir sebagai solusi untuk mengatasi permasalahan ini, khususnya dalam industri hiburan seperti film dan musik (Arfisko, 2022). Netflix, Amazon Prime, dan Disney+ merupakan contoh platform yang sangat mengandalkan sistem rekomendasi untuk meningkatkan keterlibatan pengguna dan kepuasan pelanggan.

Proyek ini bertujuan untuk membangun sistem rekomendasi film menggunakan dua pendekatan yang umum digunakan:
1. **Content-Based Filtering**: menyarankan film yang mirip berdasarkan konten atau fitur seperti genre.
2. **User-Based Collaborative Filtering**: menyarankan film berdasarkan kemiripan preferensi pengguna lain.

Dataset yang digunakan bersumber dari [Kaggle - Movies and Ratings for Recommendation System](https://www.kaggle.com/datasets/nicoletacilibiu/movies-and-ratings-for-recommendation-system), yang berisi lebih dari 100.000 data rating dari sekitar 600 pengguna untuk lebih dari 9.000 film.

## Business Understanding

### Problem Statements
1. Bagaimana cara merekomendasikan film yang serupa dengan film yang disukai pengguna berdasarkan genre?
2. Bagaimana cara memberikan rekomendasi film berdasarkan preferensi pengguna lain yang memiliki selera serupa?

### Goals
1. Menghasilkan daftar film yang mirip secara konten (genre) dengan film yang telah dipilih pengguna.
2. Menghasilkan daftar film yang direkomendasikan untuk pengguna tertentu berdasarkan rating pengguna lain dengan pola kesukaan yang serupa.

### Solution Approach
Untuk mencapai tujuan tersebut, proyek ini menggunakan dua pendekatan:

- **Content-Based Filtering**:
  - Menggunakan *TF-IDF Vectorization* pada fitur genre untuk merepresentasikan film dalam bentuk vektor.
  - Menggunakan *Cosine Similarity* untuk menghitung kesamaan antar film berdasarkan genre.

- **User-Based Collaborative Filtering**:
  - Membuat *user-item matrix* dari data rating.
  - Menghitung *cosine similarity* antar pengguna.
  - Melakukan prediksi rating berdasarkan rata-rata tertimbang dari pengguna lain yang mirip.

## Data Understanding

### Dataset Description
Dataset terdiri dari dua file utama:

1. **movies.csv**
   - `movieId`: ID unik film.
   - `title`: Judul film.
   - `genres`: Genre film (dipisahkan oleh '|').

2. **ratings.csv**
   - `userId`: ID unik pengguna.
   - `movieId`: ID film.
   - `rating`: Nilai rating (1-5).
   - `timestamp`: Waktu rating diberikan.

### Ukuran Dataset
- Jumlah film: ±9.000
- Jumlah pengguna: ±600
- Jumlah rating: ±100.000

### Insight Awal
- Sebagian besar film memiliki lebih dari satu genre.
- Distribusi rating cenderung normal, dengan mayoritas rating berada pada angka 3 dan 4.

## Data Preparation

Langkah-langkah yang dilakukan dalam tahap persiapan data:

1. **Menggabungkan ratings.csv dan movies.csv** agar setiap rating memiliki informasi genre dan judul.
2. **Preprocessing genre**:
   - Mengganti simbol `|` menjadi spasi agar cocok diproses oleh TF-IDF.
3. **Membuat matriks user-item** menggunakan `pivot_table` untuk collaborative filtering.
4. **Mengisi missing value** dengan 0 agar bisa dihitung similarity.

Alasan tahapan ini dilakukan adalah untuk menyatukan fitur yang relevan dan menyiapkannya dalam format yang dibutuhkan oleh masing-masing algoritma.

## Modeling

### Content-Based Filtering

1. Mengubah genre film menjadi representasi numerik menggunakan TF-IDF Vectorizer.
2. Menghitung *cosine similarity* antar film.
3. Mendefinisikan fungsi `recommend_similar_movies(title)` untuk mengembalikan 10 film paling mirip berdasarkan film input.

**Contoh Output:**
- Input: "Toy Story (1995)"
- Output:
  - Toy Story 2 (1999)
  - Monsters, Inc. (2001)
  - Antz (1998)
  - Shrek the Third (2007)
  - Turbo (2013)

### User-Based Collaborative Filtering

1. Menggunakan matriks user-item dari data rating.
2. Menghitung *cosine similarity* antar pengguna.
3. Memprediksi rating berdasarkan rata-rata tertimbang dari pengguna yang mirip.
4. Mendefinisikan fungsi `recommend_for_user(user_id)` untuk memberikan top-5 film berdasarkan prediksi.

**Contoh Output:**
- Input: User ID 10
- Output:
  - Toy Story 3 (2010)
  - Inception (2010)
  - (500) Days of Summer (2009)

## Evaluation

### Metrik Evaluasi: RMSE (Root Mean Squared Error)
RMSE digunakan untuk mengukur akurasi prediksi rating dibandingkan dengan rating aktual.

**Formula:**
![rumus](https://coralogix.com/wp-content/uploads/2025/03/Blog-31-01.png)

**Hasil RMSE:**
- RMSE (User-Based Collaborative Filtering): **1.1629**

### Interpretasi
- Nilai RMSE sekitar 1.16 berarti bahwa rata-rata kesalahan prediksi model adalah ±1 poin pada skala rating 1–5.
- Nilai ini dapat diterima untuk baseline, namun masih bisa ditingkatkan dengan model lanjutan seperti matrix factorization atau hybrid model.

## Kesimpulan

Proyek ini berhasil membangun dua sistem rekomendasi berbasis:

1. **Content-Based Filtering**, yang berhasil menyarankan film-film mirip berdasarkan genre.
2. **User-Based Collaborative Filtering**, yang mampu memberikan saran film berdasarkan pola preferensi pengguna lain.

Hasil evaluasi menggunakan RMSE menunjukkan bahwa sistem mampu memprediksi rating dengan tingkat kesalahan yang moderat. Ke depannya, pengembangan dapat diarahkan ke model hybrid yang menggabungkan dua pendekatan ini untuk hasil yang lebih akurat dan personal.

Sistem ini dapat digunakan sebagai dasar untuk pengembangan lebih lanjut dalam membangun rekomendasi film otomatis untuk platform film atau layanan streaming.

---

##### Referensi : 
- Fajriansyah, M., Adikara, P. P., & Widodo, A. W. (2021). Sistem Rekomendasi Film Menggunakan Content Based Filtering. Jurnal Pengembangan Teknologi Informasi dan Ilmu Komputer, 5(6), 2188-2199.
- Arfisko, H. H., & Wibowo, A. T. (2022). Sistem Rekomendasi Film Menggunakan Metode Hybrid Collaborative Filtering Dan Content-Based Filtering. eProceedings of Engineering, 9(3).
