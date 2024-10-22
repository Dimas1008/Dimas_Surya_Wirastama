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
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

### Mengatasi Missing Value
Pertama lakukan pengecekan missing value pada dataframe all_wisata menggunakan perintah kode berikut:

```sh
   all_wisata.isnull().sum()
```

**output:**

![image](https://github.com/user-attachments/assets/c10a69ad-dc45-4b36-969f-46d5aa80ebcd)

Hasil kode diatas digunakan untuk mengidentifikasi dan menghitung jumlah missing values di dalam DataFrame all_wisata, yang penting untuk memastikan kualitas data sebelum analisis lebih lanjut.
Tetapi dari hasil diatas tidak ada data yang missing value jadi data tidak perlu dibersihkan untuk masalah Missing Value

### Menyamakan Jenis Wisata

Langkah ini digunakan untuk mengurutkan wisata berdasarkan PlaceID kemudian memasukkannya ke dalam variabel fix_wisata, lakukan dengan perintah berikut:

```sh
   fix_wisata = all_wisata_clean.sort_values('Place_Id', ascending=True)
   fix_wisata
```

**output:**

![image](https://github.com/user-attachments/assets/3bfbc9c6-d828-4543-8b64-f58bddcf44d8)

Pada hasil gambar diatas menunjukkan data penilaian wisata oleh pengguna, yang diurutkan berdasarkan Place_Id. Data ini memberikan informasi tentang tempat wisata yang dinilai di berbagai kota, seperti Jakarta dan Surabaya, serta nilai yang diberikan oleh pengguna terhadap tempat tersebut. Ada 10.000 baris data yang diolah dalam tabel ini.

Selanjutnya lakukan pengecekan berapa jumlah fix_wisata
```sh
   len(fix_wisata.Place_Id.unique())
```

**output:**
![image](https://github.com/user-attachments/assets/d13f63a4-927f-4e22-b57f-8b1181ccee58)

kode diatas digunakan untuk mengetahui jumlah total tempat wisata unik dalam dataset fix_wisata terdapat 437 data yang unik. Dengan menggunakan fungsi ini, kita bisa mendapatkan informasi mengenai berapa banyak lokasi wisata yang berbeda (berdasarkan Place_Id) di dalam data.

Selanjutnya, mari kita cek City (kategori wisata) yang unik dengan kode berikut.
```sh
   # Mengecek kategori wisata yang unik
   fix_wisata.City.unique()
```

**output:**
![image](https://github.com/user-attachments/assets/ccbb2284-3bdf-4835-b069-f0bec9a2ce13)

Dari output kode diatas terdapat 5 yang memiliki kategori unik

Selanjutnya membuat variabel preparation yang berisi dataframe fix_wisata kemudian mengurutkan berdasarkan placeID, menggunakan kode berikut:
```sh
preparation = fix_wisata
preparation.sort_values('Place_Id')
```

**output:**
![image](https://github.com/user-attachments/assets/de240e6f-4bd2-4703-826c-26e39527a8a9)

Kode diatas digunakan untuk membuat salinan dari DataFrame fix_wisata ke dalam variabel preparation dan mengurutkannya berdasarkan kolom Place_Id.

Selanjutnya membuang data duplikat pada variabel preparation yang telah dibuat tadi, menggunakan kode berikut:
```sh
preparation = preparation.drop_duplicates('Place_Id')
preparation
```

**output:**
![image](https://github.com/user-attachments/assets/81642886-70c5-43d5-94ea-e1ef79e36475)

Kode ini efektif untuk menghapus baris duplikat dari DataFrame preparation berdasarkan kolom Place_Id, memastikan bahwa setiap tempat hanya muncul sekali dalam DataFrame.
data yang awalnya 10000 sekarang menjadi 437 setelah data yang sama dihapus.

Selanjutnya mengonversi data series ‘Place_Id’, 'City’ dan ‘Place_Name’ menjadi dalam bentuk list
```sh
   wisata_id = preparation['Place_Id'].tolist()
   wisata_name = preparation['Place_Name'].tolist()
   city_name = preparation['City'].tolist()
   
   print(len(wisata_id))
   print(len(wisata_name))
   print(len(city_name))
```

**output:**
![image](https://github.com/user-attachments/assets/13505920-7da9-445b-8c72-cbd66da9833a)

Kode ini mengonversi kolom Place_Id, Place_Name, dan City dari DataFrame preparation menjadi list dan mencetak jumlah elemen dalam masing-masing list. Ini berguna untuk memudahkan manipulasi data lebih lanjut dalam bentuk list.

Tahap berikutnya, kita akan membuat dictionary untuk menentukan pasangan key-value pada data wisata_id, wisata_name, dan city_name yang telah disiapkan sebelumnya.

```sh
   # Membuat dictionary untuk data ‘wisata_id’, ‘wisata_name’, dan ‘city_name’
   wisata_new = pd.DataFrame({
       'id': wisata_id,
       'wisata_name': wisata_name,
       'kota': city_name
   })
   wisata_new
```

**output:**
![image](https://github.com/user-attachments/assets/088595a8-fb08-4772-ba7c-5eb6ae0d450c)

Kode ini membuat sebuah DataFrame baru yang berisi tiga kolom: list wisata_id, wisata_name, dan city_name yang telah dibuat sebelumnya. DataFrame ini menyatukan informasi dari tiga variabel terpisah (ID, nama wisata, dan kota) menjadi satu struktur data yang lebih mudah diolah atau dianalisis.

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

### Model Development dengan Content Based Filtering
Membuat dataframe bernama data yang berisi wisata_new
```sh
   data = wisata_new
   data.sample(5)
```

**output:**

![image](https://github.com/user-attachments/assets/9e428509-0115-4b69-8040-21b4945c48f0)

Kode diatas ini menampilkan 5 baris data secara acak dari DataFrame wisata_new, yang membantu dalam memeriksa distribusi atau melihat representasi acak dari data yang ada tanpa harus melihat seluruh DataFrame.

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

#### Cosine Similarity

menghitung derajat kesamaan (similarity degree) antar wisata dengan teknik cosine similarity. Dengan menggunakan fungsi cosine_similarity dari library sklearn.
```sh
   from sklearn.metrics.pairwise import cosine_similarity
   
   # Menghitung cosine similarity pada matrix tf-idf
   cosine_sim = cosine_similarity(tfidf_matrix)
   cosine_sim
```

**output:**

![image](https://github.com/user-attachments/assets/01122471-43f6-4fd0-bd1d-f09c8d685860)

Hasil dari kode ini adalah matriks kesamaan kosinus. Matriks ini menunjukkan kesamaan antara setiap dokumen dalam matriks tf-idf. Nilai 1 menunjukkan bahwa dua dokumen tersebut sangat mirip, sedangkan nilai 0 menunjukkan bahwa dua dokumen tersebut tidak mirip sama sekali.
```sh
   # Membuat dataframe dari variabel cosine_sim dengan baris dan kolom berupa nama wisata
   cosine_sim_df = pd.DataFrame(cosine_sim, index=data['wisata_name'], columns=data['wisata_name'])
   print('Shape:', cosine_sim_df.shape)
   
   # Melihat similarity matrix pada setiap wisata
   cosine_sim_df.sample(5, axis=1).sample(10, axis=0)
```

**output:**

![image](https://github.com/user-attachments/assets/1ff3a175-d632-4785-b5bc-00408e902f24)

Berdasarkan gambar yang ditampilkan, terlihat bahwa similarity matrix menggunakan Cosine Similarity dari sebuah dataset wisata.

Cosine Similarity Matrix telah membuat sebuah matriks kesamaan (similarity matrix) dari data wisata yang menunjukkan kesamaan antara berbagai tempat wisata. Setiap baris dan kolom merepresentasikan tempat wisata yang berbeda, dan nilai dalam matriks menunjukkan tingkat kesamaan antara dua tempat wisata. Nilai "1.0" menunjukkan kesamaan penuh antara tempat wisata yang sama (misalnya, Rumah Batik dengan Rumah Batik). Nilai "0.0" menandakan bahwa tidak ada kesamaan antara tempat wisata tersebut (misalnya, Rumah Batik dengan Patung Buddha Empat Rupa).
Nilai kesamaan yang lebih tinggi (mendekati 1) menunjukkan bahwa dua tempat wisata memiliki kemiripan yang lebih tinggi, sedangkan nilai yang lebih rendah (mendekati 0) menunjukkan bahwa dua tempat wisata tersebut tidak terlalu mirip.

#### Mendapatkan Rekomendasi
Sebelumnya, kita telah memiliki data similarity (kesamaan) antar wisata. Kini, tibalah saatnya  menghasilkan sejumlah wisata yang akan direkomendasikan kepada pengguna. Untuk lebih memahami bagaimana cara kerjanya, lihatlah kembali matriks similarity pada tahap sebelumnya. Sebagai gambaran, mari kita ambil satu contoh berikut.

Pengguna X pernah memesan wisata dari kota bandung. Kemudian, saat pengguna tersebut berencana untuk mengunjungi wisata lain, sistem akan merekomendasikan Pantai marina atau Masjid Istiqlal. Nah, rekomendasi kedua wisata ini berdasarkan kesamaan yang dihitung dengan cosine similarity pada tahap sebelumnya.

Di sini, kita membuat fungsi wisata_recommendations dengan beberapa parameter sebagai berikut:

* Nama_wisata : Nama wisata (index kemiripan dataframe).
* Similarity_data : Dataframe mengenai similarity yang telah kita definisikan sebelumnya.
* Items : Nama dan fitur yang digunakan untuk mendefinisikan kemiripan, dalam hal ini adalah ‘wisata_name’ dan ‘City’.
* k : Banyak rekomendasi yang ingin diberikan.

```sh
   def wisata_recommendations(name_wisata, similarity_data=cosine_sim_df, items=data[['wisata_name', 'kota']], k=5):
       """
       Rekomendasi wisata berdasarkan kemiripan dataframe
   
       Parameter:
       ---
       name_wisata : tipe data string (str)
                   Nama Wisata (index kemiripan dataframe)
       similarity_data : tipe data pd.DataFrame (object)
                         Kesamaan dataframe, simetrik, dengan wisata sebagai
                         indeks dan kolom
       items : tipe data pd.DataFrame (object)
               Mengandung kedua nama dan fitur lainnya yang digunakan untuk mendefinisikan kemiripan
       k : tipe data integer (int)
           Banyaknya jumlah rekomendasi yang diberikan
       ---
   
   
       Pada index ini, kita mengambil k dengan nilai similarity terbesar
       pada index matrix yang diberikan (i).
       """
   
   
       # Mengambil data dengan menggunakan argpartition untuk melakukan partisi secara tidak langsung sepanjang sumbu yang diberikan
       # Dataframe diubah menjadi numpy
       # Range(start, stop, step)
       index = similarity_data.loc[:,name_wisata].to_numpy().argpartition(
           range(-1, -k, -1))
   
       # Mengambil data dengan similarity terbesar dari index yang ada
       closest = similarity_data.columns[index[-1:-(k+2):-1]]
   
       # Drop nama_wisata agar nama wisata yang dicari tidak muncul dalam daftar rekomendasi
       closest = closest.drop(name_wisata, errors='ignore')
   
       return pd.DataFrame(closest).merge(items).head(k)
```

Perhatikanlah, dengan menggunakan argpartition, kita mengambil sejumlah nilai k tertinggi dari similarity data (dalam kasus ini: dataframe cosine_sim_df). Kemudian, kita mengambil data dari bobot (tingkat kesamaan) tertinggi ke terendah. Data ini dimasukkan ke dalam variabel closest. Berikutnya, kita perlu menghapus nama_wisata yang yang dicari agar tidak muncul dalam daftar rekomendasi. Dalam kasus ini, nanti kita akan mencari wisata yang mirip dengan Jendela Alam, sehingga kita perlu drop nama_wisata Jendela Alam agar tidak muncul dalam daftar rekomendasi yang diberikan nanti.  

Selanjutnya, mari kita terapkan kode di atas untuk menemukan rekomendasi wisata yang mirip dengan Jendela Alam. Terapkan kode berikut:
```sh
   data[data.wisata_name.eq('Jendela Alam')]
```

**output:**
![image](https://github.com/user-attachments/assets/d37745c2-777d-4ad0-9c0f-d35c93552373)

Kode diatas tampaknya menganalisis kesamaan antara berbagai objek wisata menggunakan teknik yang disebut cosine similarity. Matriks kesamaan yang dihasilkan dapat digunakan untuk tugas seperti merekomendasikan objek wisata yang serupa atau mengelompokkan objek wisata yang serupa berdasarkan karakteristiknya.

Selanjutnya kita coba untuk mendapatkan rekomendasi wisata yang mirip dengan Jendela Alam, menggunakan kode berikut:
```sh
   wisata_recommendations('Jendela Alam')
```

**output:**
![image](https://github.com/user-attachments/assets/be9f570a-424f-444f-a64a-906f91e82018)

Sistem dengan model development Content Based Filtering telah berhasil memberikan merekomendasikan 5 nama wisata dengan kategori 'kota' Bandung

### Model Development dengan Collaborative Filtering

Teknik ini merekomendasikan item yang mirip dengan preferensi pengguna di masa lalu. Pada tahap ini, akan menerapkan teknik collaborative filtering untuk membuat sistem rekomendasi. Teknik ini membutuhkan data rating dari user.

#### Data Understanding

Pertama kita membaca dataset rating
```sh
df = rating
df
```

**output:**
![image](https://github.com/user-attachments/assets/9a004f12-4d0f-4ca1-92a4-3a081720cb13)

Kode diatat digunakan untuk menyalin data rating yang dimasukkan ke df kemudian ditampilkan dengan memanggil df
data rating memiliki 10000 baris dan 3 kolom.

#### Data Preparation

Pada tahap ini, Anda perlu melakukan persiapan data untuk menyandikan (encode) fitur ‘user’ dan ‘placeID’ ke dalam indeks integer.

```sh
   # Mengubah userID menjadi list tanpa nilai yang sama
   user_ids = df['User_Id'].unique().tolist()
   print('list User_Id: ', user_ids)
   
   # Melakukan encoding userID
   user_to_user_encoded = {x: i for i, x in enumerate(user_ids)}
   print('encoded User_Id : ', user_to_user_encoded)
   
   # Melakukan proses encoding angka ke ke userID
   user_encoded_to_user = {i: x for i, x in enumerate(user_ids)}
   print('encoded angka ke User_Id: ', user_encoded_to_user)
```

**output:**
![image](https://github.com/user-attachments/assets/70862bba-db54-4ce4-a0f5-3ec551fbe521)

Kode ini mengubah kolom User _Id menjadi daftar unik, melakukan encoding dari User _Id ke angka, dan sebaliknya.
tahap ini melakukan encoding pada User_Id ke dalam bentuk indeks integer agar lebih efisien diproses oleh algoritma.
ini juga dapat menyiapkan mekanisme untuk mengembalikan hasil encoded tersebut kembali ke User_Id asli.
Dengan encoded dan decoded dictionary ini, Anda dapat dengan mudah beralih antara ID pengguna asli dan representasi integer-nya saat melakukan analisis atau pelatihan model.

Selanjutnya, lakukan hal yang sama pada fitur ‘placeID’.

```sh
   # Mengubah placeID menjadi list tanpa nilai yang sama
   wisata_ids = df['Place_Id'].unique().tolist()
   print('list Place_Id: ', wisata_ids)
   
   # Melakukan proses encoding placeID
   wisata_to_wisata_encoded = {x: i for i, x in enumerate(wisata_ids)}
   print('encoded Place_Id : ', wisata_to_wisata_encoded)
   
   # Melakukan proses encoding angka ke placeID
   wisata_encoded_to_wisata = {i: x for i, x in enumerate(wisata_ids)}
   print('encoded angka ke Place_Id: ', wisata_encoded_to_wisata)
```

**output:**
![image](https://github.com/user-attachments/assets/f17a09e0-514d-4190-bee5-8ea6c286dd1b)

Proses ini sama seperti pada User_Id untuk kolom Place_Id. Setiap Place_Id di-encode ke dalam bentuk indeks integer yang lebih mudah diproses oleh algoritma.
Ada mekanisme dekoding untuk mengubah kembali indeks integer menjadi ID tempat asli.

Berikutnya, petakan userID dan placeID ke dataframe yang berkaitan.

```sh
   # Mapping userID ke dataframe user
   df['user'] = df['User_Id'].map(user_to_user_encoded)
   
   # Mapping placeID ke dataframe wisata
   df['wisata'] = df['Place_Id'].map(wisata_to_wisata_encoded)
```

Kode diatas ini menambahkan dua kolom baru ke DataFrame df, yaitu user dan wisata, yang berisi ID pengguna dan ID tempat yang telah terencode.

Terakhir, cek beberapa hal dalam data seperti jumlah user, jumlah wisata, dan mengubah nilai rating menjadi float.
```sh
   # Mendapatkan jumlah user
   num_users = len(user_to_user_encoded)
   print(num_users)
   
   # Mendapatkan jumlah wisata
   num_wisata = len(wisata_encoded_to_wisata)
   print(num_wisata)
   
   # Mengubah rating menjadi nilai float
   df['rating'] = df['Place_Ratings'].values.astype(np.float32)
   
   # Nilai minimum rating
   min_rating = min(df['rating'])
   
   # Nilai maksimal rating
   max_rating = max(df['rating'])
   
   print('Number of User: {}, Number of Wisata: {}, Min Rating: {}, Max Rating: {}'.format(
       num_users, num_wisata, min_rating, max_rating
   ))
```

**output:**
![image](https://github.com/user-attachments/assets/045b2ec8-92f5-4f26-b2e6-6e5f9564f146)

Kode diatas menghitung jumlah pengguna (300), jumlah tempat wisata (437), serta mengonversi data rating tempat wisata menjadi tipe data float. Selain itu, kode juga menghitung dan menampilkan nilai rating minimum (1.0) dan maksimum (5.0).

Tahap persiapan telah selesai. Berikut adalah hal-hal yang telah kita lakukan pada tahap ini:

* Memahami data rating yang kita miliki.
* Menyandikan (encode) fitur ‘user’ dan ‘placeID’ ke dalam indeks integer.
* Memetakan ‘userID’ dan ‘placeID’ ke dataframe yang berkaitan.
* Mengecek beberapa hal dalam data seperti jumlah user, jumlah wisata, kemudian mengubah nilai rating menjadi float.

Tahap persiapan ini penting dilakukan agar data siap digunakan untuk pemodelan. Namun sebelumnya, kita perlu membagi data untuk training dan validasi terlebih dahulu yang akan kita pelajari di materi berikutnya. Selanjutnya kita akan melakukan pembagian model

#### Membagi Data untuk Training dan Validasi

```sh
   # Mengacak dataset
   df = df.sample(frac=1, random_state=42)
   df
```

**output:**
![image](https://github.com/user-attachments/assets/f7e5d66e-a95c-4739-b296-a3618d5ce01a)

kode ini mengacak urutan baris dalam DataFrame df, yang berguna untuk menghilangkan bias dalam analisis atau pelatihan model.

Selanjutnya, kita bagi data train dan validasi dengan komposisi 80:20. Namun sebelumnya, kita perlu memetakan (mapping) data user dan wisata menjadi satu value terlebih dahulu. Lalu, buatlah rating dalam skala 0 sampai 1 agar mudah dalam melakukan proses training.

```sh
   # Membuat variabel x untuk mencocokkan data user dan wisata menjadi satu value
   x = df[['user', 'wisata']].values
   
   # Membuat variabel y untuk membuat rating dari hasil
   y = df['rating'].apply(lambda x: (x - min_rating) / (max_rating - min_rating)).values
   
   # Membagi menjadi 80% data train dan 20% data validasi
   train_indices = int(0.8 * df.shape[0])
   x_train, x_val, y_train, y_val = (
       x[:train_indices],
       x[train_indices:],
       y[:train_indices],
       y[train_indices:]
   )
   
   print(x, y)
```

**output:**
![image](https://github.com/user-attachments/assets/f9fe2fb1-1541-44a8-ae8b-3c20959c7cbc)

Kode diatas ini mempersiapkan data untuk model pembelajaran mesin dengan mencocokkan pengguna dan tempat, menormalkan rating, dan membagi dataset menjadi data pelatihan dan validasi.

#### Proses Training
Pada tahap ini, model menghitung skor kecocokan antara pengunjung dan wisata dengan teknik embedding. Pertama, kita melakukan proses embedding terhadap data pengunjung dan wisata. Selanjutnya, lakukan operasi perkalian dot product antara embedding pengunjung dan wisata. Selain itu, kita juga dapat menambahkan bias untuk setiap pengunjung dan wisata. Skor kecocokan ditetapkan dalam skala [0,1] dengan fungsi aktivasi sigmoid.

Di sini, kita membuat class RecommenderNet dengan keras Model class. Kode class RecommenderNet ini terinspirasi dari tutorial dalam situs Keras dengan beberapa adaptasi sesuai kasus yang sedang kita selesaikan. Terapkan kode berikut.

```sh
   import tensorflow as tf
   from tensorflow import keras
   from tensorflow.keras import layers
   
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

Model ini menggunakan embedding untuk pengguna dan tempat wisata untuk memprediksi rating dengan cara yang efisien. Dengan menggunakan produk titik dan bias, model ini dapat menangkap interaksi antara pengguna dan tempat wisata.

Selanjutnya, lakukan proses compile terhadap model.


```sh
   model = RecommenderNet(num_users, num_wisata, 50) # inisialisasi model
   
   # model compile
   model.compile(
       loss = tf.keras.losses.BinaryCrossentropy(),
       optimizer = keras.optimizers.Adam(learning_rate=0.001),
       metrics=[tf.keras.metrics.RootMeanSquaredError()]
   )f
```

Model ini menggunakan Binary Crossentropy untuk menghitung loss function, Adam (Adaptive Moment Estimation) sebagai optimizer, dan root mean squared error (RMSE) sebagai metrics evaluation.

Langkah berikutnya, mulailah proses training.

```sh
   # Memulai training
   
   history = model.fit(
       x = x_train,
       y = y_train,
       batch_size = 8,
       epochs = 100,
       validation_data = (x_val, y_val)
   )
```

**output:**
![image](https://github.com/user-attachments/assets/55ce5da1-052e-4131-8fe3-3b823f01b8a4)

Analisis Hasil
* Perbandingan Loss:
   * Loss pelatihan (0.6450) lebih rendah dibandingkan dengan loss validasi (0.7193), yang menunjukkan bahwa model mungkin mengalami sedikit overfitting. Ini berarti model belajar dengan baik pada data pelatihan tetapi tidak sebaik itu pada data validasi.
* RMSE:
   * RMSE pelatihan (0.3087) juga lebih rendah dibandingkan dengan RMSE validasi (0.3609). Ini menunjukkan bahwa model lebih akurat dalam memprediksi rating pada data pelatihan dibandingkan dengan data validasi.

### Kekurangan dan Kelebihan Model

#### 1. Collaborative Filtering (CF)
Collaborative filtering adalah pendekatan umum dalam sistem rekomendasi yang menggunakan informasi dari interaksi pengguna (misalnya, rating, klik, pembelian) untuk memberikan rekomendasi.

**Pendekatan:**
* User-Based CF: Sistem memberikan rekomendasi berdasarkan kesamaan preferensi antar pengguna. Jika pengguna A memiliki pola rating yang mirip dengan pengguna B, maka tempat wisata yang disukai oleh A akan direkomendasikan kepada B.
* Item-Based CF: Sistem merekomendasikan tempat wisata berdasarkan kesamaan antar item. Misalnya, jika pengguna sering memberikan rating tinggi kepada dua tempat wisata yang serupa, maka tempat wisata lain yang mirip dengan kedua item tersebut akan direkomendasikan.

**Kelebihan:**
* User-Based CF sangat cocok jika dataset memiliki banyak informasi interaksi antar pengguna.
* Item-Based CF lebih stabil saat ada data pengguna baru (cold start), karena hanya membutuhkan informasi tentang item (wisata) untuk membuat rekomendasi.

**Kekurangan:**
* Cold Start untuk User-Based CF: Sulit memberikan rekomendasi jika ada pengguna baru yang belum pernah memberikan rating.
Memerlukan dataset yang besar agar akurat karena bergantung pada pola interaksi.
* Top-N Recommendation Output:
Misalnya, Anda dapat menghasilkan rekomendasi tempat wisata teratas untuk seorang pengguna dengan memanfaatkan k-nearest neighbors (KNN) atau Matrix Factorization.

#### 2. Content-Based Filtering (CB)
Content-based filtering memberikan rekomendasi berdasarkan kesamaan fitur antara item. Dalam kasus rekomendasi wisata, algoritma ini dapat mempertimbangkan atribut tempat wisata seperti kategori, kota, dan deskripsi untuk memberikan rekomendasi yang mirip dengan preferensi pengguna.

**Pendekatan:**
* Model akan mengekstraksi fitur dari tempat wisata seperti jenis wisata (alam, sejarah, religi, dll.), kota, atau kata-kata dalam deskripsi, kemudian mencocokkannya dengan preferensi pengguna yang sudah ada.

**Kelebihan:**
* Tidak terpengaruh oleh masalah cold start untuk pengguna baru, karena rekomendasi dapat diberikan berdasarkan deskripsi wisata yang pernah dikunjungi.
* Sangat cocok untuk tempat wisata baru yang belum memiliki banyak interaksi pengguna, karena model bisa menggunakan informasi atribut untuk membuat rekomendasi.

**Kekurangan:**
* Keterbatasan pada eksplorasi, karena sistem hanya akan merekomendasikan tempat yang mirip dengan yang sudah pernah dikunjungi pengguna. Tidak ada elemen "penemuan" seperti pada Collaborative Filtering.
* Membutuhkan data deskriptif yang lengkap dan kaya tentang tempat wisata.
  
**Top-N Recommendation Output:**
Untuk menghasilkan rekomendasi teratas, algoritma ini bisa menghitung kesamaan antar fitur (menggunakan cosine similarity atau TF-IDF) dan merekomendasikan tempat wisata yang paling mirip dengan preferensi pengguna.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

### **Visualisasi Metrik**

Untuk melihat visualisasi proses training, mari kita plot metrik evaluasi dengan matplotlib. Terapkan kode berikut.

```sh
   plt.plot(history.history['root_mean_squared_error'])
   plt.plot(history.history['val_root_mean_squared_error'])
   plt.title('model_metrics')
   plt.ylabel('root_mean_squared_error')
   plt.xlabel('epoch')
   plt.legend(['train', 'test'], loc='upper left')
   plt.show()
```

**output:**
![image](https://github.com/user-attachments/assets/1b5b36cf-f001-42c2-9aaf-bec25ad393a8)

Proses training model yang ditampilkan cukup smooth dan model berhasil konvergen setelah sekitar 100 epochs. Dari proses ini, kita memperoleh beberapa hasil:

* Error akhir pada data training: sekitar 0.31.
* Error akhir pada data validasi (testing): sekitar 0.36.
  
Meskipun nilai error pada data validasi sedikit lebih tinggi daripada pada data training, nilai ini masih cukup baik untuk sistem rekomendasi. Model menunjukkan kemampuan yang memadai untuk memprediksi data baru, meskipun ada indikasi overfitting yang perlu diperhatikan.

### Mendapatkan Rekomendasi Wisata
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

**---Ini adalah bagian akhir laporan---**
