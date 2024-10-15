# Laporan Proyek Machine Learning - Dimas Surya Wirastama
## _Proyek Pertama_

# Domain Proyek
### Latar Belakang
enyakit jantung merupakan salah satu penyebab utama kematian di seluruh dunia. Setiap tahunnya, jutaan orang mengalami gangguan kesehatan serius yang berkaitan dengan jantung, seperti serangan jantung dan gagal jantung. Di Indonesia, penyakit jantung menjadi salah satu penyebab kematian tertinggi, terutama pada kelompok usia lanjut, yang menunjukkan urgensi deteksi dini untuk faktor-faktor risiko penyakit jantung.

Faktor risiko seperti usia, tekanan darah, kadar kolesterol, kebiasaan merokok, indeks massa tubuh (BMI), dan riwayat diabetes, memiliki peran penting dalam memprediksi potensi terjadinya penyakit jantung. Mengingat banyaknya faktor risiko yang harus dipertimbangkan, memprediksi penyakit jantung secara manual menjadi tugas yang rumit dan rentan terhadap kesalahan.

Dengan kemajuan teknologi dalam kecerdasan buatan (AI) dan pembelajaran mesin (ML), terdapat peluang untuk meningkatkan kemampuan prediksi risiko penyakit jantung secara lebih cepat dan akurat. Penggunaan model machine learning dapat membantu tenaga medis dalam memberikan diagnosis yang lebih baik berdasarkan data kesehatan pasien, yang pada akhirnya dapat meningkatkan upaya pencegahan dan pengobatan.

Dalam proyek ini, dikembangkan sistem prediksi risiko penyakit jantung berbasis machine learning menggunakan berbagai model seperti K-Nearest Neighbor (KNN), Random Forest, Gradient Boosting, dan Support Vector Machine (SVM). Dengan memanfaatkan data medis seperti tekanan darah, kadar kolesterol, dan riwayat kesehatan pasien, sistem ini diharapkan dapat membantu mengidentifikasi risiko penyakit jantung secara lebih efektif, sehingga dapat mengurangi angka kematian dan meningkatkan kualitas hidup pasien melalui deteksi dan intervensi dini.

# Business Understanding
### Problem Statements
- Pernyataan Masalah 1: 
Meningkatnya jumlah kasus stroke membuat prediksi risiko stroke menjadi sangat penting untuk pencegahan dini.
- Pernyataan Masalah 2: 
Ada banyak faktor kesehatan yang mempengaruhi risiko stroke, seperti tekanan darah, kolesterol, dan usia, namun identifikasi faktor paling signifikan sulit dilakukan secara manual.
- Pernyataan Masalah 3:
Tingginya variasi dalam data kesehatan pasien seringkali menyebabkan kesalahan prediksi, sehingga diperlukan pendekatan machine learning untuk menghasilkan prediksi yang lebih akurat dan tepat waktu.
- Pernyataan Masalah 4:
Kebutuhan untuk membandingkan performa berbagai model machine learning seperti KNN, Random Forest, Adaboost, dan SVM guna menemukan model yang paling optimal dalam memprediksi risiko stroke secara akurat.

### Goals
- Jawaban Pernyataan Masalah 1: 
Mengembangkan model prediksi risiko stroke untuk membantu pencegahan dini dan penanganan yang lebih efektif berdasarkan data kesehatan pasien.
- Jawaban Pernyataan Masalah 2:
Menganalisis berbagai faktor kesehatan seperti tekanan darah, kolesterol, usia, dan faktor lainnya guna mengidentifikasi variabel paling signifikan yang mempengaruhi risiko stroke.
- Jawaban Pernyataan Masalah 3:
Menerapkan teknik machine learning untuk mengolah data kesehatan pasien yang bervariasi, dengan tujuan meningkatkan akurasi prediksi risiko stroke.
- Jawaban Pernyataan Masalah 4:
Membandingkan performa model prediksi yang dibuat dengan algoritma KNN, Random Forest, Adaboost, dan SVM, untuk menemukan model terbaik yang dapat memberikan hasil prediksi yang paling akurat dan andal.

### Solution Statements
- Solution 1: 
Menggunakan Beberapa Algoritma untuk Membuat Model Prediksi Untuk mencapai prediksi risiko stroke yang akurat, solusi pertama adalah menggunakan dan membandingkan empat algoritma berbeda: K-Nearest Neighbors (KNN), Random Forest, AdaBoost, dan Support Vector Machine (SVM). Masing-masing algoritma memiliki pendekatan unik untuk memprediksi hasil berdasarkan fitur kesehatan, sehingga memungkinkan untuk menemukan model terbaik yang dapat menghasilkan prediksi akurat.
    - KNN: Algoritma sederhana yang efektif untuk masalah klasifikasi, cocok untuk dataset yang memiliki hubungan non-linear.
    - Random Forest: Algoritma berbasis pohon keputusan yang kuat untuk mengatasi overfitting dan bekerja baik dengan dataset yang memiliki banyak fitur.
    - AdaBoost: Metode boosting yang memperkuat model lemah menjadi model yang lebih kuat, ideal untuk menangani data yang tidak seimbang.
    - SVM: Model klasifikasi yang bekerja baik dalam ruang dimensi tinggi dan digunakan untuk dataset yang kompleks dengan kelas yang dapat dipisahkan.

- Solution 2: 
Hyperparameter Tuning untuk Peningkatan Model
Setelah membandingkan performa awal dari empat algoritma di atas, langkah selanjutnya adalah melakukan hyperparameter tuning untuk meningkatkan kinerja model terbaik. Hyperparameter tuning dilakukan dengan menggunakan metode seperti GridSearchCV atau RandomizedSearchCV, untuk mencari kombinasi parameter optimal yang menghasilkan model dengan performa terbaik.
    - Random Forest, tuning dapat dilakukan pada jumlah estimators, max depth, dan minimum samples per leaf.
    - SVM, tuning dapat dilakukan pada kernel (linear, RBF), gamma, dan C.
    - KNN, tuning dilakukan pada jumlah tetangga (K).
    - AdaBoost, tuning dilakukan pada learning rate dan jumlah estimators.

# Data Understanding
Dataset ini dapat diakses menggunakan [Kaggle](https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction)

Variabel-variabel pada dataset Heart Disease adalah sebagai berikut:
1. Age: Usia individu dalam tahun.
2. Sex: Jenis kelamin individu (M: Male, F: Female).
3. ChestPainType: Jenis rasa sakit di dada yang dialami individu (TA: Typical Angina, ATA: Atypical Angina, NAP: Non-Anginal Pain, ASY: Asymptomatic).
4. RestingBP: Tekanan darah saat istirahat (mm Hg).
5. Cholesterol: Kadar kolesterol serum dalam mg/dl.
6. FastingBS: Kadar gula darah puasa (1: jika FastingBS > 120 mg/dl, 0: jika tidak).
7. RestingECG: Hasil elektrokardiografi saat istirahat (Normal, ST: kelainan ST-T wave, LVH: menunjukkan kemungkinan hipertrofi ventrikel kiri).
8. MaxHR: Denyut jantung maksimum yang dicapai (bpm).
9. ExerciseAngina: Apakah individu mengalami angina saat berolahraga (Y: Ya, N: Tidak).
10. Oldpeak: Depresi ST yang disebabkan oleh olahraga relatif terhadap istirahat.
11. ST_Slope: Kemiringan segmen ST saat puncak latihan (Up: meningkat, Flat: datar, Down: menurun).
12. eartDisease: Apakah individu menderita penyakit jantung (1: Ya, 0: Tidak).

### Data Loading
Data_Jantung = pd.read_csv perintah ini digunakan untuk membaca data dengan format csv. kemudian ditampikan dengan memanggil kelas Data_Jantung, juga bisa menggunakan Data_Jantung.head() jika hanya ingin menampilkan 5 baris data

![Screenshot 2024-10-15 001931](https://github.com/user-attachments/assets/60592abd-bb34-453c-8e2e-1657b3557dcc)

### Menampilkan informasi dari dataset

Pada Fungsi info() di pandas yang digunakan untuk menampilkan informasi dari dataset seperti jenis data tipe datanya

![Screenshot 2024-10-15 002725](https://github.com/user-attachments/assets/d0cd8d6b-e982-403b-8b6a-86fe633d6a57)

Pada fungsi describe() yang berfungsi untuk menampilkan statistik dari dataset dan deskripsi pada dataset

![Screenshot 2024-10-15 002913](https://github.com/user-attachments/assets/0bccff20-d548-4259-89ea-f1cf509c755e)

Fungsi describe() memberikan informasi statistik pada masing-masing kolom, antara lain:
* Count  adalah jumlah sampel pada data.
* Mean adalah nilai rata-rata 
* Std adalah standar deviasi
* Min yaitu nilai minimum setiap kolom
* 25% adalah kuartil pertama. Kuartil adalah nilai yang menandai batas interval dalam empat bagian sebaran yang sama.
* 50% adalah kuartil kedua, atau biasa juga disebut median (nilai tengah).
* 75% adalah kuartil ketiga
* Max adalah nilai maksimum.

### Mencari missing value
Pada proyek ini  digunakan fungsi isnull().sum() yang berfungsi untuk menemukan nilai _missing value_ di masing  masing kolom dataset. _missing value_ sendiri dapat diartikan sebagai nilai atribut yang kosong pada objek data.

![image](https://github.com/user-attachments/assets/0c6fb89a-94b2-4bb9-af5b-8754c8fc7f7b)

### Visuali Data
Visualisai ini digunakan untuk melihat apakah ada data yang terdapat indikasi outlier atau tidak

![image](https://github.com/user-attachments/assets/100a161b-9a67-467d-a7d9-5a85b6673b42)

menggunakan perintah berikut:
```sh
df_outlier=data_jantung.select_dtypes(exclude=['object'])
for column in df_outlier:
        plt.figure()
        sns.boxplot(data=df_outlier, x=column)
```

# Data Preparation
### Tahap Preparation :
* menangani dataset categorical dengan data yang dapat dimengerti oleh mesin yaitu angka menggunakan teknik One-Hot-Encoding pada dataset categorical yaitu Sex, ChestPainType, RestingECG, ExerciseAngina dan ST_Slope 
* Melakukan Reduksi menggunakan teknik PCA 
* Melakukan normalisasi data numerical sehingga memiliki mean 0 dan standard deviation 1

Untuk melakukan proses encoding fitur kategori, salah satu teknik yang umum dilakukan adalah teknik one-hot-encoding.

**One Hot Encoding**

```sh
from sklearn.preprocessing import OneHotEncoder
# Melakukan One-Hot Encoding pada kolom 'Sex'
data_jantung = pd.concat([data_jantung, pd.get_dummies(data_jantung['Sex'], prefix='Sex')], axis=1)

# Melakukan One-Hot Encoding pada kolom 'ChestPainType'
data_jantung = pd.concat([data_jantung, pd.get_dummies(data_jantung['ChestPainType'], prefix='ChestPainType')], axis=1)

# Melakukan One-Hot Encoding pada kolom 'RestingECG'
data_jantung = pd.concat([data_jantung, pd.get_dummies(data_jantung['RestingECG'], prefix='RestingECG')], axis=1)

# Melakukan One-Hot Encoding pada kolom 'ExerciseAngina'
data_jantung = pd.concat([data_jantung, pd.get_dummies(data_jantung['ExerciseAngina'], prefix='ExerciseAngina')], axis=1)

# Melakukan One-Hot Encoding pada kolom 'ST_Slope'
data_jantung = pd.concat([data_jantung, pd.get_dummies(data_jantung['ST_Slope'], prefix='ST_Slope')], axis=1)

# Menghapus kolom asli yang sudah di-encode
data_jantung.drop(['Sex', 'ChestPainType', 'RestingECG', 'ExerciseAngina', 'ST_Slope'], axis=1, inplace=True)

# Melihat data setelah encoding
data_jantung.head()
```


| Age | RestingBP | Cholesterol | Oldpeak | HeartDisease | Sex_F | Sex_M | ChestPainType_ASY  | ChestPainType_ATA  | ChestPainType_NAP  | ChestPainType_TA | RestingECG_LVH | RestingECG_Normal | RestingECG_ST | ExerciseAngina_N  | ExerciseAngina_Y | ST_Slope_Down  | ST_Slope_Flat  | ST_Slope_Up |
|-----|-----------|-------------|---------|--------------|-------|-------|--------------------|--------------------|--------------------|------------------|----------------|-------------------|---------------|-------------------|------------------|----------------|----------------|-------------|
|  40 |       140 |         289 |     0.0 |            0 | False |  True |              False |               True |              False |            False |          False |             False |          True |            False  |             True |          False |          False |        True |
|  49 |       160 |         180 |     1.0 |            1 |  True | False |              False |              False |               True |            False |          False |             False |          True |            False  |             True |          False |           True |       False |
|  37 |       130 |         283 |     0.0 |            0 | False |  True |              False |               True |              False |            False |          False |              True |         False |             True  |            False |          False |          False |        True |
|  48 |       138 |         214 |     1.5 |            1 |  True | False |               True |              False |              False |            False |          False |             False |          True |            False  |             True |           True |          False |       False |
|  54 |       150 |         195 |     0.0 |            0 | False |  True |              False |              False |               True |            False |          False |              True |         False |             True  |            False |          False |          False |        True |

**TEKNIK PCA**
PCA bekerja menggunakan metode aljabar linier. Ia mengasumsikan bahwa sekumpulan data pada arah dengan varians terbesar merupakan yang paling penting (utama). PCA umumnya digunakan ketika variabel dalam data memiliki korelasi yang tinggi. Korelasi tinggi ini menunjukkan data yang berulang atau redundant. 

Berikut penjelasan untuk masing-masing komponen utama (PC):
* PC pertama mewakili arah varians maksimum dalam data. Ia paling banyak menangkap informasi dari semua fitur dalam data. 
* PC kedua menangkap sebagian besar informasi yang tersisa setelah PC pertama. 
* PC ketiga menangkap sebagian besar informasi yang tersisa setelah PC pertama, PC kedua, dst.

Pertama cek menggunakan fungsi pairplot 

![download (1)](https://github.com/user-attachments/assets/29490898-68c8-4dac-9f40-5baaf2859ff8)

aplikasikan class PCA dari library scikit learn 

```sh
from sklearn.decomposition import PCA

pca = PCA(n_components=3, random_state=123)
pca.fit(data_jantung[['RestingBP', 'Cholesterol', 'Oldpeak']])
princ_comp = pca.transform(data_jantung[['RestingBP', 'Cholesterol', 'Oldpeak']])
```

proporsi informasi dari ketiga komponen yang dimasukkan

![image](https://github.com/user-attachments/assets/0cbca08d-8dc3-4daa-8ea6-852f54fda091)

Membuat fitur baru :
* Gunakan n_component = 1, karena kali ini, jumlah komponen kita hanya satu.
* Fit model dengan data masukan.
* Tambahkan fitur baru ke dataset dengan nama 'dimension' dan lakukan proses transformasi.
* Drop kolom yang dimasukkan.

```sh
from sklearn.decomposition import PCA
pca = PCA(n_components=1, random_state=123)
pca.fit(data_jantung[['RestingBP', 'Cholesterol', 'Oldpeak']])
data_jantung['dimension'] = pca.transform(data_jantung.loc[:, ('RestingBP', 'Cholesterol', 'Oldpeak')]).flatten()
data_jantung.drop(['RestingBP', 'Cholesterol', 'Oldpeak'], axis=1, inplace=True)
data_jantung
```

| Age | HeartDisease | Sex_F | Sex_M | ChestPainType_ASY  | ChestPainType_ATA  | ChestPainType_NAP  | ChestPainType_TA  | RestingECG_LVH  | RestingECG_Normal | RestingECG_ST  | ExerciseAngina_N  | ExerciseAngina_Y  | ST_Slope_Down  | ST_Slope_Flat  | ST_Slope_Up | Dimension   |
|-----|--------------|-------|-------|--------------------|--------------------|--------------------|-------------------|-----------------|-------------------|----------------|-------------------|-------------------|----------------|----------------|-------------|-------------|
|  40 |            0 | False |  True |              False |               True |              False |             False |           False |              True |          False |              True |             False |          False |          False |        True |  50.203699  |
|  49 |            1 |  True | False |              False |              False |               True |             False |           False |              True |          False |              True |             False |          False |           True |       False | -58.137443  |
|  37 |            0 | False |  True |              False |               True |              False |             False |           False |             False |           True |              True |             False |          False |          False |        True |  43.902772  |
|  48 |            1 |  True | False |               True |              False |              False |             False |           False |              True |          False |             False |              True |          False |           True |       False | -24.820727  |
|  54 |            0 | False |  True |              False |              False |               True |             False |           False |              True |          False |              True |             False |          False |          False |        True | -43.449184  |

**Split Dataset**

Pada proyek ini digunakan teknik split dataset, split dataset sangat penting dilakukan sebelum tahap modelling. dan untuk melakukan, kita perlu mengimport library split data yaitu  _train_test_split_, kemudian buat 2 variabel yaitu  X yang berfungsi untuk menghapus kolom _age_ dan y untuk menampilkan kolom age lalu bagi dataset menjadi 4 variabel baru  yaitu X_train, X_test, y_train, y_test dengan library _train_test_split_ dengan parameter yang digunakan yaitu :
* X berfungsi untuk menghapus kolom age
* y berfungsi menampilkan kolom age
* test_size adalah ukuran pembagian dataset yaitu sekitar 80 % untuk training dan 20 % untuk testing, data testing ini bertujuan untuk  mengukur kinerja model pada data baru.
* random_state: digunakan untuk mengontrol random number generator yang digunakan, di proyek ini menggunakan __random_state = 123__

```sh
from sklearn.model_selection import train_test_split

X = data_jantung.drop(["Age"],axis =1) 
y = data_jantung["Age"] 
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.1, random_state = 123)

# Menampilkan hasil
print(f'Total # of sample in whole dataset: {len(X)}')
print(f'Total # of sample in train dataset: {len(X_train)}')
print(f'Total # of sample in test dataset: {len(X_test)}')
```

![image](https://github.com/user-attachments/assets/b1ec8dfe-8e78-4fa3-957a-9ca3ac972638)

**Standarisai**

Algoritma machine learning memiliki performa lebih baik dan konvergen lebih cepat ketika dimodelkan pada data dengan skala relatif sama atau mendekati distribusi normal. Proses scaling dan standarisasi membantu untuk membuat fitur data menjadi bentuk yang lebih mudah diolah oleh algoritma. 
Standardisasi adalah teknik transformasi yang paling umum digunakan dalam tahap persiapan pemodelan. Untuk fitur numerik, kita tidak akan melakukan transformasi dengan one-hot-encoding seperti pada fitur kategori. Kita akan menggunakan teknik StandarScaler dari library Scikitlearn, 

```sh
from sklearn.preprocessing import StandardScaler

numerical_features = ['HeartDisease', 'dimension']
scaler = StandardScaler()
scaler.fit(X_train[numerical_features])
X_train[numerical_features] = scaler.transform(X_train.loc[:, numerical_features])
X_train[numerical_features].head()
```

|       | HeartDisease | Dimension  |
|-------|--------------|------------|
|  488  |    -0.873689 |   0.264232 |
|  11   |     1.144571 |  -1.473456 |
|  814  |     1.144571 |   1.280659 |
|  426  |    -0.873689 |  -1.440052 |
|  761  |     1.144571 |  -0.186384 |

Proses standarisasi mengubah nilai rata-rata (mean) menjadi 0 dan nilai standar deviasi menjadi 1. Untuk mengecek nilai mean dan standar deviasi pada setelah proses standarisasi

```sh
X_train[numerical_features].describe().round(4)
```

|               | HeartDisease | dimension |
|---------------|--------------|-----------|
| count         | 529.0000     | 529.0000  |
| mean          | -0.0000      | -0.0000   |
| std           | 1.0009       | 1.0009    |
| min           | -0.8737      | -3.0409   |
| 25%           | -0.8737      | -0.6609   |
| 50%           | -0.8737      | -0.1164   |
| 75%           | 1.1446       | 0.6389    |
| max           | 1.1446       | 3.2617    |

Tabel ini menampilkan statistik deskriptif untuk dua kolom: **HeartDisease** dan **dimension**.

# Modeling
**K-Nearest Neighbors (KNN)**

Deskripsi: Algoritma KNN adalah salah satu algoritma sederhana berbasis jarak. KNN bekerja dengan mengklasifikasikan data baru berdasarkan mayoritas kelas dari tetangga terdekatnya.

Parameter yang Digunakan:
n_neighbors: Jumlah tetangga yang digunakan untuk voting. Hyperparameter ini dapat di-tuning.
weights: Bobot untuk tetangga. Dapat berupa 'uniform' atau 'distance'.

Kelebihan:
Mudah diimplementasikan dan dipahami.
Tidak membutuhkan asumsi khusus tentang distribusi data.

Kekurangan:
Kinerja lambat pada dataset besar karena harus menghitung jarak setiap kali.
Sangat sensitif terhadap fitur dengan skala besar, sehingga normalisasi sangat penting.

**Random Forest (RF)**

Deskripsi: Random Forest adalah algoritma ensemble yang menggunakan banyak pohon keputusan (decision trees). Setiap pohon di-train pada subset data yang berbeda, dan hasil akhirnya adalah voting mayoritas dari semua pohon.

Parameter yang Digunakan:
n_estimators: Jumlah pohon yang digunakan dalam hutan. Semakin banyak pohon, semakin stabil model, tetapi juga meningkatkan waktu komputasi.
max_depth: Kedalaman maksimum pohon, dapat digunakan untuk menghindari overfitting.

Kelebihan:
Kuat terhadap overfitting, terutama dengan dataset berukuran besar.
Mampu menangani data dengan missing values dan bekerja dengan baik pada data dengan kategori maupun numerik.

Kekurangan:
Waktu komputasi yang relatif lama dibandingkan algoritma lain jika jumlah pohon sangat banyak.
Model yang lebih sulit diinterpretasikan karena sifatnya sebagai ensemble.

**AdaBoost (Adaptive Boosting)**

Deskripsi: Algoritma AdaBoost adalah teknik boosting yang fokus pada menggabungkan model-model lemah untuk membentuk model yang kuat. Setiap model berikutnya diberi perhatian khusus pada data yang sebelumnya salah diklasifikasikan oleh model sebelumnya.

Parameter yang Digunakan:
n_estimators: Jumlah model lemah (weak learners) yang digunakan.
learning_rate: Kontrol seberapa besar kontribusi setiap model lemah pada prediksi akhir.

Kelebihan:
Dapat memperbaiki kinerja model yang lemah.
Cocok untuk menangani masalah klasifikasi kompleks.

Kekurangan:
Sangat sensitif terhadap data outlier.
Cenderung overfitting jika jumlah iterasi (n_estimators) terlalu banyak.

**Support Vector Machine (SVM)**

Deskripsi: SVM adalah algoritma yang mencoba menemukan hyperplane optimal yang dapat memisahkan data dari dua kelas dengan margin terbesar.

Parameter yang Digunakan:
C: Parameter regulasi yang mengontrol trade-off antara margin maksimal dan kesalahan klasifikasi.
kernel: Fungsi kernel untuk mengubah ruang fitur (linear, RBF, polynomial).

Kelebihan:
Sangat efektif dalam ruang dimensi tinggi.
Memiliki performa yang baik pada dataset kecil dengan margin yang jelas.

Kekurangan: 
Waktu komputasi yang tinggi pada dataset besar.
Sulit untuk diinterpretasikan, terutama dengan kernel yang kompleks.

# Evaluation

Metrik yang akan kita gunakan pada prediksi ini adalah MSE atau Mean Squared Error yang menghitung jumlah selisih kuadrat rata-rata nilai sebenarnya dengan nilai prediksi.

![image](https://github.com/user-attachments/assets/ff4c8572-5c96-407a-97e9-9304120b5cd1)

Dalam proyek ini, Mean Squared Error (MSE) digunakan untuk mengevaluasi model prediksi. MSE mengukur seberapa jauh nilai prediksi model dari nilai sebenarnya, dengan memberi bobot lebih besar pada kesalahan yang lebih besar karena menggunakan kuadrat dari selisih.

Keterangan:
N = jumlah dataset
yi = nilai sebenarnya
y_pred = nilai prediksi

Hasil Evaluasi

![image](https://github.com/user-attachments/assets/36e9a1b2-f5ff-44db-af70-333a5826fb6f)

Interpretasi:
Semakin kecil nilai MSE, semakin baik model dalam melakukan prediksi.
Karena MSE menghitung selisih kuadrat, ini sangat sensitif terhadap outlier, sehingga model dengan nilai MSE rendah dianggap lebih stabil dan akurat dalam tugas prediksi.

- KNN memiliki train MSE sebesar 0.067668 dan test MSE sebesar 0.082806, menunjukkan bahwa performa model di data uji cukup konsisten dengan performa di data latih.
- Random Forest memiliki train MSE yang sangat kecil (0.014918), namun test MSE yang relatif lebih tinggi (0.10448), yang bisa mengindikasikan overfitting, dimana model terlalu mempelajari data latih dan kurang mampu generalisasi pada data uji.
- Boosting menunjukkan performa yang lebih konsisten antara data latih dan data uji, dengan train MSE sebesar 0.07228 dan test MSE sebesar 0.088933.
- SVM juga memiliki train MSE sebesar 0.073573 dan test MSE sebesar 0.081076, yang menunjukkan performa yang baik dan seimbang antara data latih dan uji.


