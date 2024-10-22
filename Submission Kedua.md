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

Informasi Dataset
Sumber Data:
[Kaggle - Indonesia Tourism Destination](https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination)

Baris dan Kolom:
* tourism_with_id.csv: 437 entries
* tourism_rating.csv: 10,000 entries
* user.csv: 300 entries
* package_tourism.csv: 100 entries

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


### Data Preprocessing
#### Menggabungkan Data Wisata

Pada langkah ini dilakukan penggabungkan seluruh placeID pada kategori Wisata menggunakan kode berikut:
```sh
   resto_all = np.concatenate((
       id_tourist.Place_Id.unique(),
       rating.Place_Id.unique(),
   ))
```

**output**

![image](https://github.com/user-attachments/assets/46cee9ee-3af7-42b2-a2eb-181ce7abf29c)

Kode ini menghasilkan daftar unik Place_Id dari dua sumber data, memastikan tidak ada duplikasi, dan memberikan total jumlah Place_Id yang tersedia. Ini berguna untuk analisis lebih lanjut tentang restoran berdasarkan kategori wisata.

#### Menggabungkan Seluruh Data User

Pada langkah ini dilakukan penggabungan data dan penghapusan data yang sama pada seluruh User_Id pada ketegori wisata menggunakan kode berikut:
```sh
   # Menggabungkan seluruh userID
   user_all = np.concatenate((
       rating.User_Id.unique(),
       user.User_Id.unique(),
   ))
   
   # Menghapus data yang sama kemudian mengurutkannya
   user_all = np.sort(np.unique(user_all))
   
   print('Jumlah seluruh user: ', len(user_all))
```

**output**

![image](https://github.com/user-attachments/assets/25195e30-13de-4862-ba0f-451715201c65)

Kode ini menghasilkan daftar unik User _Id dari dua sumber data, memastikan tidak ada duplikasi, dan memberikan total jumlah User _Id yang tersedia. Ini berguna untuk analisis lebih lanjut tentang pengguna dalam konteks data yang Anda miliki.

#### Mengetahui Jumlah Rating

Pada langkah ini digunakan untuk mengetahui jumlah seluruh rating dari berbagai file dengan cara menggabungkan file id_tourist dan rating ke dalam dataframe resto_info setelah itu menggabungkan dataframe rating dengan rating_info berdasarkan nilai Place_Id , dengan implementasikan kode berikut.

```sh
   # Menggabungkan file id_tourist dan rating ke dalam dataframe resto_info
   rating_info = pd.concat([id_tourist, rating])
   
   # Menggabungkan dataframe rating dengan rating_info berdasarkan nilai Place_Id
   wisata = pd.merge(rating, rating_info , on='Place_Id', how='left')
   wisata
```
**output:**

![image](https://github.com/user-attachments/assets/000cb1b0-63ce-4088-9e62-b7647792d597)

Kode ini menggabungkan data dari dua sumber (id_tourist dan rating) ke dalam satu DataFrame rating_info, kemudian menggabungkan informasi lebih lanjut dari rating_info ke dalam DataFrame wisata berdasarkan Place_Id. Pada Data terdapat banyak missing value

#### Mencari Missing Value

Pada langkah ini melakukab cek missing value dengan fungsi isnull()

```sh
wisata.isnull().sum()
```

**output:**

![image](https://github.com/user-attachments/assets/f491f789-8b8f-4840-873c-ff6e98e4a901)

Kode diatas digunakan untuk melakukan pengecekean missing value pada sluruh data wisata.
Terdapat banyak missing value pada sebagian besar fitur. Hanya fitur User_Id_x, Place_Id, Place_Ratings_x saja yang memiliki 0 missing value.

#### Menggabungkan Data dengan Fitur Nama Wisata 

1. Pertama mendefinisikan datfraem rating ke dalam variabel all_wisata_rate menggunakan kode berikut:
```sh
   all_wisata_rate = rating
   all_wisata_rate
```

**output:**

![image](https://github.com/user-attachments/assets/fb9ca37e-2ca0-435a-b65b-aa637470ed73)

Hasil Kode diatas membuat salinan dari DataFrame rating dan menyimpannya dalam variabel all_wisata_rate. Ini berguna jika Anda ingin bekerja dengan salinan data penilaian tanpa mengubah data asli di rating.

2. Kedua menggabungkan dataframe all_wisata_rate dengan dataframe Place_Name berdasarkan placeID
```sh
   all_wisata_name = pd.merge(all_wisata_rate, id_tourist[['Place_Id','Place_Name']], on='Place_Id', how='left')
   
   # Print dataframe all_wisata_name
   all_wisata_name
```

**output:**

![image](https://github.com/user-attachments/assets/d787d67d-6cb5-4bdb-9e5c-f01b53379b0d)

Hasil Kode diatas digunakan untuk menggabungkan informasi dari DataFrame all_wisata_rate dengan nama tempat dari DataFrame id_tourist, menghasilkan DataFrame baru all_wisata_name yang berisi informasi lengkap tentang penilaian dan nama tempat wisata. Ini berguna untuk analisis yang lebih mendalam mengenai wisata dan untuk memberikan konteks pada data penilaian yang ada.

#### Menggabungkan Data dengan Fitur city

Pada langkah ini menggabungkan dataframe id_tourist pada kolom city dengan all_wisata_name dan memasukkannya ke dalam variabel all_wisata
```sh
   all_wisata = pd.merge(all_wisata_name, id_tourist[['Place_Id','City']], on='Place_Id', how='left')
   all_wisata
```

**output:**

![image](https://github.com/user-attachments/assets/dc7baf97-8aef-4e5d-8cd4-9f08945954b8)

Kode ini menggabungkan informasi dari DataFrame all_wisata_name dengan informasi kota dari DataFrame id_tourist, menghasilkan DataFrame baru all_wisata yang berisi informasi lengkap tentang penilaian, nama tempat, dan kota tempat wisata. Ini berguna untuk analisis yang lebih mendalam mengenai wisata dan untuk memberikan konteks pada data penilaian yang ada.
Inilah data yang akan gunakan untuk membuat sistem rekomendasi.

## Data Preparation

### 1. Encode Label
Pada tahap ini, melakukan persiapan data untuk menyandikan (encode) fitur ‘user’ dan ‘placeID’ ke dalam indeks integer.

![image](https://github.com/user-attachments/assets/bf7eb644-258a-4949-b1eb-edc1a53fc169)

### 2. Membagi Data untuk Training dan Validasi
Pada tahap ini, melakukan pembagian data train dan validasi dengan komposisi 80:20. Namun sebelumnya, kita perlu memetakan (mapping) data user dan wisata menjadi satu value terlebih dahulu. Lalu, buatlah rating dalam skala 0 sampai 1 agar mudah dalam melakukan proses training.

![image](https://github.com/user-attachments/assets/35d6f369-ad97-4eaa-bcb7-054713107dcb)

Pada tampilan diatas mempersiapkan data untuk model pembelajaran mesin dengan mencocokkan pengguna dan tempat, menormalkan rating, dan membagi dataset menjadi data pelatihan dan validasi.

### 3. Mengatasi Missing Value
Mengatasi missing values dilakukan pada dataframe all_wisata. Dari hasil pengecekan, tidak ditemukan missing values, sehingga tidak perlu ada langkah tambahan untuk menangani masalah ini.

![image](https://github.com/user-attachments/assets/2963fad4-f412-4b82-a19d-719bb6eb7f53)

### 3. Menyamakan Jenis Wisata
Data wisata diurutkan berdasarkan Place_Id, yang kemudian disimpan dalam variabel fix_wisata. Proses ini menghasilkan data yang terorganisir untuk pengolahan lebih lanjut.

### 4. Pengecekan Jumlah Wisata Unik
Menggunakan kode untuk menghitung jumlah tempat wisata unik berdasarkan Place_Id. Hasilnya, terdapat 437 tempat wisata unik di dataset, yang berarti 437 lokasi berbeda telah tercatat.

### 5. Pengelompokan Kategori Wisata yang Unik
Data dikategorikan dan diurutkan berdasarkan kategori wisata, memungkinkan pengelompokan tempat wisata yang memiliki jenis wisata serupa.

### 6. Pembuatan DataFrame Baru
Sebuah DataFrame baru, wisata_new, dibentuk untuk menyimpan informasi berupa Place_Id, Place_Name, dan City yang diambil dari data fix_wisata. Proses ini mempermudah pengolahan data lebih lanjut.

### 7. Penghapusan Data Duplikat
Data duplikat berdasarkan Place_Id dihapus, menghasilkan 437 baris data unik dari yang awalnya berjumlah 10.000. Ini memastikan bahwa setiap tempat wisata hanya muncul sekali dalam dataset.

### 8. Konversi ke dalam List
Kolom Place_Id, Place_Name, dan City dari DataFrame dikonversi menjadi bentuk list untuk memudahkan proses manipulasi data lebih lanjut.

### 9. Pembuatan Dictionary
Data yang telah diubah menjadi list tersebut kemudian digunakan untuk membentuk dictionary baru yang menghubungkan wisata_id, wisata_name, dan city_name ke dalam DataFrame wisata_new.

### TF-IDF Vectorizer
```sh
   from sklearn.feature_extraction.text import TfidfVectorizer
   
   # Inisialisasi TfidfVectorizer
   tf = TfidfVectorizer()
   
   # Melakukan perhitungan idf pada data cuisine
   tf.fit(data['kota'])
   
   # Mapping array dari fitur index integer ke fitur nama
   tf.get_feature_names_out()
```

**output:**

![image](https://github.com/user-attachments/assets/2958b60f-fd07-4a33-818b-e9fa16ed40fa)

Kode diatas ini melakukan perhitungan TF-IDF pada kolom 'kota' dari DataFrame data dan mengambil nama fitur yang dihasilkan. Ini berguna untuk melakukan analisis teks dan menghasilkan fitur yang lebih bermakna.

Kemudian lakukan fit dan transformasi ke dalam bentuk matriks.
```sh
   # Melakukan fit lalu ditransformasikan ke bentuk matrix
   tfidf_matrix = tf.fit_transform(data['kota'])
   
   # Melihat ukuran matrix tfidf
   tfidf_matrix.shape
```

**output:**

![image](https://github.com/user-attachments/assets/66d8d356-abb1-47f6-bb18-d8ebed2a362c)

Perhatikanlah, matriks yang kita miliki berukuran (437, 5). Nilai 437 merupakan ukuran data dan 5 merupakan matrik kategori wisata.

Untuk menghasilkan vektor tf-idf dalam bentuk matriks, kita menggunakan fungsi todense(). Jalankan kode berikut.
```sh
   # Mengubah vektor tf-idf dalam bentuk matriks dengan fungsi todense()
   tfidf_matrix.todense()
```

**output:**

![image](https://github.com/user-attachments/assets/07366c31-8a75-41f9-98bd-2e22275e022b)

Kode tersebut mengubah representasi sparse matrix dari vektor TF-IDF menjadi dense matrix, yang lebih mudah dibaca dan dipahami. Matriks ini menunjukkan hubungan antara kata-kata dalam dokumen dengan nilai-nilai yang berbeda untuk setiap kata. Nilai 1 menunjukkan bahwa kata tersebut muncul dalam dokumen dan memiliki bobot tertentu dalam perhitungan TF-IDF, sementara nilai 0 menunjukkan tidak ada kemunculan kata tersebut dalam dokumen.

Selanjutnya, mari kita lihat matriks tf-idf untuk beberapa wisata (wisata_name) dan kategori kota (City). Terapkan kode berikut.
```sh
   # Membuat dataframe untuk melihat tf-idf matrix
   # Kolom diisi dengan jenis wisata
   # Baris diisi dengan nama kota
   
   pd.DataFrame(
       tfidf_matrix.todense(),
       columns=tf.get_feature_names_out(),
       index=data.wisata_name
   ).sample(5, axis=1).sample(10, axis=0)
```

**output:**

![image](https://github.com/user-attachments/assets/280e9451-7c51-4c5f-9574-d300094d9825)

Output Matriks TF-IDF yang  ditampilkan menunjukkan hubungan antara nama tempat wisata dan kota-kota di Indonesia. Setiap baris dalam matriks mewakili tempat wisata, sedangkan setiap kolom mewakili kota. Nilai dalam matriks menunjukkan apakah tempat wisata tersebut terkait dengan kota tertentu (1.0) atau tidak (0.0).

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**1. Content-Based Filtering**

Pendekatan Content-Based Filtering didasarkan pada kesamaan antara fitur tempat wisata. Pada kasus ini, kita menggunakan cosine similarity untuk mengukur tingkat kemiripan antar tempat wisata berdasarkan fitur yang relevan, seperti nama wisata dan kota.

**Tahapan Content-Based Filtering:**

   * **Menghitung Cosine Similarity:** Cosine similarity digunakan untuk mengukur kemiripan antara tempat wisata. Semakin tinggi nilai cosine similarity, semakin mirip dua tempat wisata tersebut. Ini memungkinkan kita untuk memberikan rekomendasi wisata yang memiliki karakteristik yang serupa dengan wisata yang pernah dikunjungi oleh pengguna.
   * **Mendapatkan Top-N Recommendations:** Setelah menghitung cosine similarity, kita dapat merekomendasikan tempat wisata yang memiliki kemiripan tertinggi dengan wisata yang telah dikunjungi oleh pengguna.
   * Hasil Rekomendasi: Sebagai contoh, wisata yang direkomendasikan berdasarkan kesamaan dengan Jendela Alam meliputi:
     
| Wisata Name               | Kota   |
|---------------------------|--------|
| Tebing Karaton            | Bandung|
| Roemah Seni Sarasvati     | Bandung|
| Pasar Baru                | Bandung|
| Curug Cilengkrang         | Bandung|
| Taman Sejarah Bandung     | Bandung|

Tabel ini menampilkan nama tempat wisata di kolom **Wisata Name** dan lokasinya (kota) di kolom **Kota**.

**2. Collaborative Filtering**

Pendekatan kedua adalah Collaborative Filtering. Pada pendekatan ini, rekomendasi diberikan berdasarkan interaksi pengguna dengan tempat wisata. Model RecommenderNet digunakan untuk memprediksi tempat wisata yang mungkin disukai pengguna berdasarkan pola interaksi pengguna-pengguna lain yang memiliki preferensi serupa.

**Tahapan Collaborative Filtering:**

   * **Membangun Model RecommenderNet:** Model RecommenderNet menggunakan embedding untuk merepresentasikan pengguna dan tempat wisata dalam ruang vektor. Model ini belajar untuk memprediksi relevansi antara pengguna dan tempat wisata yang belum dikunjungi. Kode berikut digunakan untuk membangun model RecommenderNet:

```sh
   class RecommenderNet(tf.keras.Model):
   
     # Insialisasi fungsi
     def __init__(self, num_users, num_wisata, embedding_size, **kwargs):
       super(RecommenderNet, self).__init__(**kwargs)
       self.num_users = num_users
       self.num_wisata = num_wisata
       self.embedding_size = embedding_size
       self.user_embedding = layers.Embedding( # layer embedding user
           num_users,
           embedding_size,
           embeddings_initializer = 'he_normal',
           embeddings_regularizer = keras.regularizers.l2(1e-6)
       )
       self.user_bias = layers.Embedding(num_users, 1) # layer embedding user bias
       self.wisata_embedding = layers.Embedding( # layer wisata embeddings
           num_wisata,
           embedding_size,
           embeddings_initializer = 'he_normal',
           embeddings_regularizer = keras.regularizers.l2(1e-6)
       )
       self.wisata_bias = layers.Embedding(num_wisata, 1) # layer embedding wisata bias
   
     def call(self, inputs):
       user_vector = self.user_embedding(inputs[:,0]) # memanggil layer embedding 1
       user_bias = self.user_bias(inputs[:, 0]) # memanggil layer embedding 2
       wisata_vector = self.wisata_embedding(inputs[:, 1]) # memanggil layer embedding 3
       wisata_bias = self.wisata_bias(inputs[:, 1]) # memanggil layer embedding 4
   
       dot_user_wisata = tf.tensordot(user_vector, wisata_vector, 2)
   
       x = dot_user_wisata + user_bias + wisata_bias
   
       return tf.nn.sigmoid(x) # activation sigmoid
```

Class RecommenderNet ini melakukan embedding untuk pengguna dan tempat wisata, menggabungkan embedding dengan bias pengguna dan wisata, lalu mengaktifkan output dengan fungsi sigmoid untuk mendapatkan skor kecocokan. Anda bisa melanjutkan proses ini dengan mempersiapkan data dan melatih model.

   * **Melatih Model:** Model dilatih menggunakan optimizer Adam dan fungsi loss BinaryCrossentropy untuk memprediksi relevansi wisata berdasarkan interaksi pengguna.


### Melatih Model
Selanjutnya, kita akan melatih model ini menggunakan data pelatihan yang sudah disiapkan sebelumnya. Proses pelatihan akan diatur menggunakan Adam optimizer dan binary crossentropy loss untuk memperkirakan relevansi wisata berdasarkan interaksi pengguna sebelumnya.

```sh
model = RecommenderNet(num_users, num_wisata, 50)
model.compile(
    loss=tf.keras.losses.BinaryCrossentropy(),
    optimizer=keras.optimizers.Adam(learning_rate=0.001),
)

history = model.fit(
    x=[x_train.user, x_train.wisata],
    y=y_train,
    batch_size=64,
    epochs=10,
    verbose=1,
    validation_data=([x_val.user, x_val.wisata], y_val),
)
```

Dalam proses pelatihan, kita menggunakan Adam optimizer dengan BinaryCrossentropy sebagai fungsi loss. Setelah model dilatih, langkah terakhir adalah menghasilkan rekomendasi wisata untuk pengguna tertentu.

### Mendapatkan Rekomendasi Wisata

#### Model Development dengan Content Based Filtering

Di sini, kita membuat fungsi wisata_recommendations dengan beberapa parameter sebagai berikut:

* Nama_wisata : Nama wisata (index kemiripan dataframe).
* Similarity_data : Dataframe mengenai similarity yang telah kita definisikan sebelumnya.
* Items : Nama dan fitur yang digunakan untuk mendefinisikan kemiripan, dalam hal ini adalah ‘wisata_name’ dan ‘City’.
* k : Banyak rekomendasi yang ingin diberikan.

**Mendapatkan rekomendasi wisata yang mirip dengan Jendela Alam**
wisata_recommendations('Jendela Alam')

Berikut adalah tabel dari data yang kamu berikan mengenai nama tempat wisata dan kota:

| Wisata Name               | Kota   |
|---------------------------|--------|
| Tebing Karaton            | Bandung|
| Roemah Seni Sarasvati     | Bandung|
| Pasar Baru                | Bandung|
| Curug Cilengkrang         | Bandung|
| Taman Sejarah Bandung     | Bandung|

Tabel ini menampilkan nama tempat wisata di kolom **Wisata Name** dan lokasinya (kota) di kolom **Kota**.

#### Model Development dengan Collaborative Filtering

Untuk mendapatkan rekomendasi wisata, pertama kita ambil sampel user secara acak dan definisikan variabel wisata_not_visited yang merupakan daftar wisata yang belum pernah dikunjungi oleh pengguna. Anda mungkin bertanya-tanya, mengapa kita perlu menentukan daftar wisata_not_visited? Hal ini karena daftar wisata_not_visited inilah yang akan menjadi wisata yang kita rekomendasikan.

Sebelumnya, pengguna telah memberi rating pada beberapa wisata yang telah mereka kunjungi. Kita menggunakan rating ini untuk membuat rekomendasi wisata yang mungkin cocok untuk pengguna. Nah, wisata yang akan direkomendasikan tentulah wisata yang belum pernah dikunjungi oleh pengguna. Oleh karena itu, kita perlu membuat variabel wisata_not_visited sebagai daftar wisata untuk direkomendasikan pada pengguna.

Variabel wisata_not_visited diperoleh dengan menggunakan operator bitwise (~) pada variabel wisata_visited_by_user.

```sh
   wisata_df = wisata_new
   df = pd.read_csv('tourism_rating.csv')
   
   # Mengambil sample user
   user_id = df.User_Id.sample(1).iloc[0]
   wisata_visited_by_user = df[df.User_Id == user_id]
   
   # Operator bitwise (~), bisa diketahui di sini https://docs.python.org/3/reference/expressions.html
   wisata_not_visited = wisata_df[~wisata_df['id'].isin(wisata_visited_by_user.Place_Id.values)]['id']
   wisata_not_visited = list(
       set(wisata_not_visited)
       .intersection(set(wisata_to_wisata_encoded.keys()))
   )
   
   wisata_not_visited = [[wisata_to_wisata_encoded.get(x)] for x in wisata_not_visited]
   user_encoder = user_to_user_encoded.get(user_id)
   user_wisata_array = np.hstack(
       ([[user_encoder]] * len(wisata_not_visited), wisata_not_visited)
   )
```

Kode diatas ini mempersiapkan data pengguna dan tempat wisata yang belum dikunjungi oleh pengguna tersebut untuk proses rekomendasi.

Selanjutnya, untuk memperoleh rekomendasi wisata, gunakan fungsi model.predict() dari library Keras dengan menerapkan kode berikut.

```sh
   ratings = model.predict(user_wisata_array).flatten()
   
   top_ratings_indices = ratings.argsort()[-10:][::-1]
   recommended_wisata_ids = [
       user_to_user_encoded.get(wisata_not_visited[x][0]) for x in top_ratings_indices
   ]
   
   print('Showing recommendations for users: {}'.format(user_id))
   print('===' * 9)
   print('Wisata with high ratings from user')
   print('----' * 8)
   
   top_wisata_user = (
       wisata_visited_by_user.sort_values(
           by = 'Place_Ratings',
           ascending=False
       )
       .head(5)
       .Place_Id.values
   )
   
   wisata_df_rows = wisata_df[wisata_df['id'].isin(top_wisata_user)]
   for row in wisata_df_rows.itertuples():
       print(row.wisata_name, ':', row.kota)
   
   print('----' * 8)
   print('Top 5 wisata recommendation')
   print('----' * 8)
   
   recommended_wisata = wisata_df[wisata_df['id'].isin(recommended_wisata_ids)]
   for row in recommended_wisata.itertuples():
       print(row.wisata_name, ':', row.kota)
```

**output:**

![image](https://github.com/user-attachments/assets/dad45aec-edcc-4f20-b224-bf42a76bcf6a)

Kode ini menghasilkan rekomendasi tempat wisata untuk pengguna berdasarkan rating yang diprediksi oleh model. Rekomendasi ini didasarkan pada tempat wisata yang belum dikunjungi oleh pengguna dan juga mempertimbangkan tempat wisata yang telah mereka kunjungi dengan rating tertinggi.

Sistem telah berhasil memberikan rekomendasi kepada user. Sebagai contoh, hasil di atas adalah rekomendasi untuk user dengan id 273. Dari output tersebut, kita dapat membandingkan antara wisata with high ratings from user dan Top 5 wisata recommendation untuk user.

Perhatikanlah, beberapa wisata rekomendasi menyediakan kategori kota (City) yang sesuai dengan rating user.

## Evaluation
**Metrik Evaluasi yang Digunakan**
Pada tahap ini, evaluasi dilakukan untuk mengukur kinerja model rekomendasi yang telah dibuat. Dua pendekatan yang digunakan, yaitu Collaborative Filtering dan Content-Based Filtering, dievaluasi menggunakan metrik yang sesuai. Berikut adalah metrik yang digunakan untuk masing-masing pendekatan:

* Collaborative Filtering: Root Mean Squared Error (RMSE) RMSE digunakan untuk mengukur kesalahan prediksi antara nilai yang diharapkan dan nilai yang diprediksi oleh model. Semakin kecil nilai RMSE, semakin baik performa model dalam memprediksi rekomendasi.

* Content-Based Filtering: Precision Precision adalah proporsi rekomendasi yang relevan di antara seluruh rekomendasi yang diberikan. Precision yang tinggi menunjukkan bahwa model memberikan rekomendasi yang relevan sesuai dengan preferensi pengguna.

**Hasil Evaluasi**
Collaborative Filtering: RMSE Hasil evaluasi model collaborative filtering menunjukkan bahwa nilai RMSE akhir pada data training adalah sekitar 0.31, sementara pada data validasi (testing) adalah 0.36. Meskipun terdapat sedikit peningkatan pada nilai error di data validasi, nilai tersebut masih dalam batas yang dapat diterima.

* Error akhir pada data training: 0.31
* Error akhir pada data validasi: 0.36
 
Visualisasi proses training menunjukkan bahwa model cukup stabil dan konvergen setelah sekitar 100 epochs. Hal ini menunjukkan bahwa model dapat memprediksi data baru dengan cukup baik.

Content-Based Filtering: Precision Untuk evaluasi content-based filtering, precision dihitung berdasarkan seberapa banyak rekomendasi yang relevan di antara seluruh rekomendasi yang diberikan. Precision yang tinggi mengindikasikan bahwa model mampu memberikan rekomendasi yang tepat.

Hasil evaluasi precision menunjukkan bahwa model memiliki precision sebesar 80%, yang berarti 80% dari rekomendasi yang diberikan oleh model sesuai dengan preferensi pengguna.

Precision: 80%

**Kesimpulan**
Meskipun kedua model memiliki pendekatan yang berbeda, evaluasi menunjukkan bahwa keduanya dapat memberikan hasil yang cukup baik. Collaborative filtering berhasil menghasilkan prediksi dengan RMSE yang rendah, sementara content-based filtering memberikan rekomendasi yang relevan dengan precision yang tinggi.

**output:**

![image](https://github.com/user-attachments/assets/1b5b36cf-f001-42c2-9aaf-bec25ad393a8)

Proses training model yang ditampilkan cukup smooth dan model berhasil konvergen setelah sekitar 100 epochs. Dari proses ini, kita memperoleh beberapa hasil:

* Error akhir pada data training: sekitar 0.31.
* Error akhir pada data validasi (testing): sekitar 0.36.
  
Meskipun nilai error pada data validasi sedikit lebih tinggi daripada pada data training, nilai ini masih cukup baik untuk sistem rekomendasi. Model menunjukkan kemampuan yang memadai untuk memprediksi data baru, meskipun ada indikasi overfitting yang perlu diperhatikan.

**Dampak Terhadap Problem Statements**

1. Rekomendasi Wisata Akurat dan Personal Model yang dikembangkan berhasil memberikan rekomendasi wisata yang lebih akurat dan personal kepada pengguna. Dengan menggunakan metode Content-Based Filtering dan Collaborative Filtering, sistem dapat memanfaatkan data rating dan informasi destinasi untuk menghasilkan rekomendasi yang sesuai dengan preferensi pengguna individual.

2. Pemanfaatan Informasi Lokasi dan Rating Model berhasil memanfaatkan informasi lokasi (kota) dan rating wisata untuk menghasilkan rekomendasi yang relevan. Penggunaan TF-IDF dan Cosine Similarity dalam Content-Based Filtering memungkinkan sistem untuk merekomendasikan tempat wisata berdasarkan kesamaan lokasi, sementara Collaborative Filtering memanfaatkan pola rating untuk memberikan rekomendasi yang lebih personal.

3. Implementasi dan Perbandingan Metode Proyek ini berhasil mengimplementasikan dan membandingkan metode Content-Based Filtering dan Collaborative Filtering. Evaluasi menunjukkan kekuatan dan kelemahan masing-masing metode, memberikan wawasan berharga untuk pengembangan sistem rekomendasi yang lebih baik di masa depan.

**Pencapaian Goals**

1. Pengembangan Sistem Rekomendasi Content-Based Goal ini tercapai dengan berhasilnya implementasi sistem rekomendasi menggunakan Content-Based Filtering berdasarkan informasi kota dan nama tempat wisata.

2. Implementasi Collaborative Filtering Sistem rekomendasi menggunakan Collaborative Filtering berhasil diimplementasikan, memanfaatkan data rating pengguna untuk memberikan rekomendasi personal.

3. Perbandingan dan Evaluasi Metode Kedua metode berhasil dibandingkan dan dievaluasi efektivitasnya dalam memberikan rekomendasi wisata yang akurat.

**Dampak Solusi Statement**
1. Content-Based Filtering
   * Dampak Positif: Memungkinkan rekomendasi tempat wisata berdasarkan kesamaan lokasi, membantu wisatawan menemukan destinasi serupa di kota yang sama atau berdekatan.
   * Keterbatasan: Mungkin kurang efektif dalam menemukan preferensi baru yang belum pernah dikunjungi pengguna sebelumnya.

2. Collaborative Filtering
   * Dampak Positif: Memberikan rekomendasi berdasarkan pola preferensi pengguna lain yang mirip, memungkinkan penemuan destinasi baru yang mungkin disukai.
   * Keterbatasan: Memerlukan data rating yang cukup banyak untuk memberikan rekomendasi yang akurat.

**---Ini adalah bagian akhir laporan---**
