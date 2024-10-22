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
