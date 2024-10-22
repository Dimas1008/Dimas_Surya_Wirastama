# Laporan Proyek Machine Learning - Dimas Surya Wirastama

## Project Overview
Pariwisata merupakan sektor penting dalam perekonomian Indonesia. Namun, wisatawan sering kali kesulitan menemukan destinasi wisata yang sesuai dengan preferensi mereka di antara banyaknya pilihan yang tersedia. Proyek ini bertujuan untuk mengembangkan sistem rekomendasi wisata yang dapat membantu wisatawan menemukan destinasi yang sesuai dengan minat mereka, memanfaatkan data rating dan informasi destinasi wisata yang tersedia.

Proyek ini penting karena dapat:
1. Meningkatkan kepuasan wisatawan dengan rekomendasi yang personal
2. Membantu destinasi wisata yang kurang terkenal untuk lebih dikenal
3. Mendorong pemerataan kunjungan wisata di berbagai daerah di Indonesia

[Sistem Rekomendasi dalam Pariwisata](https://link.springer.com/referenceworkentry/10.1007/978-3-030-48652-5_26)

## Business Understanding
Pada bagian ini, Anda perlu menjelaskan proses klarifikasi masalah.
Bagian laporan ini mencakup:

### Problem Statements
Menjelaskan pernyataan masalah:
- Bagaimana cara memberikan rekomendasi wisata yang akurat dan personal kepada pengguna berdasarkan data rating dan informasi destinasi yang tersedia?
- Bagaimana memanfaatkan informasi lokasi (kota) dan rating wisata untuk menghasilkan rekomendasi yang relevan?
- Bagaimana mengimplementasikan dan membandingkan metode Content-Based Filtering dan Collaborative Filtering untuk sistem rekomendasi wisata?

### Goals
Menjelaskan tujuan proyek yang menjawab pernyataan masalah:
- Mengembangkan sistem rekomendasi wisata menggunakan metode Content-Based Filtering berdasarkan informasi kota dan nama tempat wisata.
- Mengimplementasikan sistem rekomendasi menggunakan Collaborative Filtering untuk memanfaatkan data rating pengguna.
- Membandingkan dan mengevaluasi efektivitas kedua metode dalam memberikan rekomendasi wisata yang akurat.

### Solution statements
- Menggunakan teknik Content-Based Filtering dengan TF-IDF dan Cosine Similarity untuk merekomendasikan tempat wisata berdasarkan kesamaan lokasi (kota).
- Mengimplementasikan Collaborative Filtering untuk menghasilkan rekomendasi berdasarkan pola rating pengguna terhadap tempat wisata.
- Mengevaluasi performa kedua metode menggunakan metrik yang sesuai dan membandingkan hasil rekomendasi yang dihasilkan.

Pendekatan ini memungkinkan sistem untuk memberikan rekomendasi yang personal berdasarkan konten (lokasi) dan juga pola preferensi pengguna, memanfaatkan data yang tersedia dalam dataset yang Anda gunakan.

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah "Indonesia Tourism Destination" yang tersedia di Kaggle. Dataset ini berisi informasi tentang destinasi wisata di Indonesia, termasuk rating dari pengunjung dan detail lokasi. Dataset dapat diunduh dari [Kaggle - Indonesia Tourism Destination](https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination).

Dataset ini terdiri dari beberapa file CSV, dengan total lebih dari 400 destinasi wisata dari berbagai provinsi di Indonesia. Berikut adalah rincian file dan variabel yang terdapat dalam dataset:

1. tourism_with_id.csv (437 entries):
    * Place_Id: ID unik untuk setiap tempat wisata
    * Place_Name: Nama tempat wisata
    * Description: Deskripsi singkat tentang tempat wisata
    * Category: Kategori tempat wisata (seperti taman hiburan, cagar alam, dll.)
    * City: Kota tempat wisata berada
    * Price: Harga tiket masuk (dalam Rupiah)
    * Rating: Rating rata-rata tempat wisata (skala 1-5)
    * Time_Minutes: Waktu yang dibutuhkan untuk mengunjungi tempat tersebut (dalam menit)
    * Coordinate: Koordinat geografis tempat wisata
    * Lat: Latitude tempat wisata
    * Long: Longitude tempat wisata
  
2. tourism_rating.csv (10000 entries):
    * User_Id: ID unik untuk setiap pengguna
    * Place_Id: ID tempat wisata yang diberi rating
    * Place_Ratings: Rating yang diberikan oleh pengguna (skala 1-5)

3. user.csv (300 entries):
    * User_Id: ID unik untuk setiap pengguna
    * Location: Lokasi asal pengguna
    * Age: Usia pengguna

4. package_tourism.csv (100 entries):
    * Package: Id Paket wisata
    * City: Kota wisata
    * Place_Tourism1: Tujuan wisata pertama
    * Place_Tourism2: Tujuan wisata kedua
    * Place_Tourism3: Tujuan wisata ketiga
    * Place_Tourism4: Tujuan wisata keempat
    * Place_Tourism5: Tujuan wisata kelima

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data beserta insight atau exploratory data analysis.


### Read data

menggunakan perintah berikut:
```sh
  user = pd.read_csv('user.csv')
  paket = pd.read_csv('package_tourism.csv')
  id_tourist = pd.read_csv('tourism_with_id.csv')
  rating = pd.read_csv('tourism_rating.csv')
  
  print('Jumlah data pengunjung: ', len(user.User_Id.unique()))
  print('Jumlah data tujuan pengunjung: ', len(id_tourist.Place_Id.unique()))
  print('Jumlah data penilaian yang diberikan pengguna: ', len(rating.Place_Id.unique()))
  print('Jumlah data penilaian wisata: ', len(rating.User_Id.unique()))
```

**output:**

![image](https://github.com/user-attachments/assets/0399a87a-3f96-4c2a-a897-ea4c20e584ef)

Kode ini bertujuan untuk memuat data dari beberapa file CSV dan menghitung jumlah unik pengguna, tujuan wisata, penilaian yang diberikan, serta jumlah pengguna yang memberikan penilaian. Hasilnya memberikan gambaran tentang ukuran dan keragaman data dalam konteks analisis wisata.

Variabel-variabel pada Wisata di Indonesia dataset adalah sebagai berikut:

* user : merupakan data pengunjung
* paket : merupakan data wisata dengan paket objek wisata
* id_tourist : merupakan data wisata pengunjung.
* rating : merupakan data kumpulan rating

#### Read Data user
```sh
  user.info()
```

**output:**

![image](https://github.com/user-attachments/assets/5939ba25-d75f-4793-b418-84a8962fb390)

* DataFrame user memiliki 300 baris dan 3 kolom: User _Id, Location, dan Age.
* Semua kolom memiliki nilai yang tidak kosong (non-null), yang berarti tidak ada data yang hilang.
* Tipe data untuk User _Id dan Age adalah integer, sedangkan Location adalah string (object).

#### Read Data paket
```sh
  paket.info()
```

**output:**

![image](https://github.com/user-attachments/assets/1be6d09c-c979-4441-bd38-e04d777d4cf6)

* DataFrame paket memiliki 100 baris dan 7 kolom: Package, City, Place_Tourism1, Place_Tourism2, Place_Tourism3, Place_Tourism4, dan Place_Tourism5.
* Kolom Package, City, Place_Tourism1, Place_Tourism2, dan Place_Tourism3 memiliki nilai yang tidak kosong (non-null) untuk semua entri, sedangkan Place_Tourism4 dan Place_Tourism5 memiliki beberapa nilai yang hilang (66 dan 39 non-null, masing-masing).
* Tipe data untuk kolom Package adalah integer, sedangkan kolom lainnya adalah string (object).

#### Read Data id_tourist
```sh
  id_tourist.info()
```

**output:**

![image](https://github.com/user-attachments/assets/12a2052b-87ad-463e-92ae-4ff60819c44b)

* DataFrame id_tourist memiliki 437 baris dan 13 kolom: Place_Id, Place_Name, Description, Category, City, Price, Rating, Time_Minutes, Coordinate, Lat, Long, Unnamed: 11, Unnamed: 12,
* Kolom Place_Id, Place_Name, Description, Category, City, Price, Rating, Coordinate, Lat, dan Long memiliki nilai yang tidak kosong (non-null) untuk semua entri, sedangkan Time_Minutes memiliki 205 nilai non-null dan Unnamed: 11 tidak memiliki nilai non-null.
* Tipe data untuk kolom Place_Id, Price, dan Unnamed: 12 adalah integer, sedangkan kolom Rating, Time_Minutes, Lat, dan Long adalah float, dan kolom lainnya adalah string (object).

#### Read Data rating
```sh
  rating.info()
```

**output:**

![image](https://github.com/user-attachments/assets/2b40b4ea-0d82-472e-81d3-603d58f34646)

* DataFrame rating memiliki 10.000 baris dan 3 kolom: User _Id, Place_Id, dan Place_Ratings.
* Semua kolom memiliki nilai yang tidak kosong (non-null) untuk semua entri, yang berarti tidak ada data yang hilang.
* Semua kolom memiliki tipe data integer (int64).

**Menjumlahkan rating menggunakan kode berikut:**
```sh
  rating.Place_Ratings.value_counts()
```

**output:**

![image](https://github.com/user-attachments/assets/1c72001b-e589-4a2c-bbff-b8d7be715b47)

* Total entri dalam kolom Place_Ratings adalah 10.000, dan jumlah kemunculan untuk setiap nilai menunjukkan bagaimana pengguna memberikan penilaian terhadap tempat yang ada.
* Nilai 4 adalah yang paling banyak muncul, diikuti oleh nilai 3 dan 2. Nilai 1 adalah yang paling sedikit muncul, menunjukkan bahwa penilaian tertinggi (5) dan terendah (1) memiliki frekuensi yang lebih rendah dibandingkan dengan nilai tengah (2, 3, dan 4).

**Mendeskripsikan rating:**
```sh
  rating.describe()
```

**output:**

![image](https://github.com/user-attachments/assets/f3732828-29ff-4ea1-8bdd-6ea61e95fe29)

- DataFrame ini memberikan gambaran umum tentang distribusi User _Id, Place_Id, dan Place_Ratings.
- Rata-rata Place_Ratings adalah 3.07, yang menunjukkan bahwa penilaian cenderung berada di sekitar nilai tengah.
- Standar deviasi yang relatif kecil pada Place_Ratings (1.38) menunjukkan bahwa penilaian tidak terlalu bervariasi.
- Nilai minimum dan maksimum menunjukkan rentang penilaian dari 1 hingga 5, sesuai dengan skala yang umum digunakan untuk penilaian.

**melihat berapa pengunjung yang memberikan rating**
```sh
  print('Jumlah userID: ', len(rating.User_Id.unique()))
  print('Jumlah placeID: ', len(rating.Place_Id.unique()))
  print('Jumlah data rating: ', len(rating))
```

**output:**

![image](https://github.com/user-attachments/assets/d92c59f6-aad2-46fe-9f62-2222ef131a47)

Kode diatas digunakan Untuk melihat berapa pengunjung yang memberikan rating berdasarkan User_Id dan Place_Id pada rating

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
