# Laporan Proyek Machine Learning - Dimas Surya Wirastama
## _Proyek Pertama_

# Domain Proyek
### Latar Belakang
Stroke merupakan salah satu penyebab utama kematian dan kecacatan di seluruh dunia. Setiap tahunnya, jutaan orang mengalami stroke, yang tidak hanya berdampak signifikan pada kesehatan individu, tetapi juga menimbulkan beban ekonomi bagi keluarga dan sistem kesehatan. Di Indonesia, stroke menjadi penyebab kematian tertinggi, terutama di kalangan dewasa lanjut usia, sehingga penting untuk mendeteksi faktor risiko sejak dini.

Faktor-faktor risiko seperti usia, tekanan darah, kadar kolesterol, kebiasaan merokok, dan riwayat penyakit jantung memainkan peran kunci dalam kemungkinan terjadinya stroke. Dengan meningkatnya jumlah pasien yang berisiko, prediksi stroke menggunakan metode konvensional sering kali tidak cukup akurat dan tidak mampu memberikan hasil yang cepat dan tepat waktu.

Dengan adanya perkembangan teknologi dalam bidang kecerdasan buatan (AI) dan machine learning (ML), muncul peluang untuk meningkatkan kemampuan prediksi stroke berdasarkan analisis data medis. Model machine learning dapat membantu dokter dan tenaga medis dalam memberikan diagnosis yang lebih cepat dan akurat, sehingga upaya pencegahan dan pengobatan dapat dilakukan lebih awal, mengurangi tingkat kematian dan kecacatan akibat stroke.

Melalui proyek ini, saya mengembangkan sebuah sistem prediksi risiko stroke berbasis machine learning menggunakan berbagai model seperti K-Nearest Neighbor (KNN), Random Forest, Gradient Boosting dan Svm. Dengan memanfaatkan data riwayat medis pasien, sistem ini diharapkan dapat membantu meningkatkan kualitas layanan kesehatan dalam mengidentifikasi risiko stroke pada pasien secara lebih efektif.

# Business Understanding
### Problem Statements
- Pernyataan Masalah 1: 
Meningkatnya jumlah kasus stroke membuat prediksi risiko stroke menjadi sangat penting untuk pencegahan dini.
- Pernyataan Masalah 2: 
Ada banyak faktor kesehatan yang mempengaruhi risiko stroke, seperti tekanan darah, kolesterol, dan usia, namun identifikasi faktor paling signifikan sulit dilakukan secara manual.
- Pernyataan Masalah 3: 
Bagaimana cara melakukan pra-pemrosesan data agar dapat digunakan untuk membuat model?
- Pernyataan Masalah 4: 
Bagaimana cara membuat model prediksi berdasarkan dari 4 algoritma yaitu KNN, Random Forest, Adaboost, dan SVM?

### Goals
- Jawaban Pernyataan Masalah 1: 
Mengembangkan model prediksi risiko stroke untuk membantu pencegahan dini dan penanganan yang lebih efektif berdasarkan data kesehatan pasien.
- Jawaban Pernyataan Masalah 2:
Menganalisis berbagai faktor kesehatan seperti tekanan darah, kolesterol, usia, dan faktor lainnya guna mengidentifikasi variabel paling signifikan yang mempengaruhi risiko stroke.
- Jawaban Pernyataan Masalah 3:
Melakukan pra-pemrosesan data yang meliputi penanganan missing values, normalisasi, encoding fitur kategori, dan pembagian data untuk memastikan data siap digunakan dalam model machine learning.
- Jawaban Pernyataan Masalah 4:
Membuat model prediksi menggunakan empat algoritma (KNN, Random Forest, Adaboost, dan SVM) serta melakukan perbandingan performa di antara model-model tersebut untuk menemukan model dengan akurasi terbaik.

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
Jika ada angka di salah satu tabel maka disitu ada data yang missing values, jika ada missing value gunakan kode berikut :
```sh
data_jantung = data_stroke.dropna()
```

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

> The overriding design goal for Markdown's
> formatting syntax is to make it as readable
> as possible. The idea is that a
> Markdown-formatted document should be
> publishable as-is, as plain text, without
> looking like it's been marked up with tags
> or formatting instructions.

This text you see here is *actually- written in Markdown! To get a feel
for Markdown's syntax, type some text into the left window and
watch the results in the right.

## Tech

Dillinger uses a number of open source projects to work properly:

- [AngularJS] - HTML enhanced for web apps!
- [Ace Editor] - awesome web-based text editor
- [markdown-it] - Markdown parser done right. Fast and easy to extend.
- [Twitter Bootstrap] - great UI boilerplate for modern web apps
- [node.js] - evented I/O for the backend
- [Express] - fast node.js network app framework [@tjholowaychuk]
- [Gulp] - the streaming build system
- [Breakdance](https://breakdance.github.io/breakdance/) - HTML
to Markdown converter
- [jQuery] - duh

And of course Dillinger itself is open source with a [public repository][dill]
 on GitHub.

## Installation

Dillinger requires [Node.js](https://nodejs.org/) v10+ to run.

Install the dependencies and devDependencies and start the server.

```sh
cd dillinger
npm i
node app
```

For production environments...

```sh
npm install --production
NODE_ENV=production node app
```

## Plugins

Dillinger is currently extended with the following plugins.
Instructions on how to use them in your own application are linked below.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantaneously see your updates!

Open your favorite Terminal and run these commands.

First Tab:

```sh
node app
```

Second Tab:

```sh
gulp watch
```

(optional) Third:

```sh
karma test
```

#### Building for source

For production release:

```sh
gulp build --prod
```

Generating pre-built zip archives for distribution:

```sh
gulp build dist --prod
```

## Docker

Dillinger is very easy to install and deploy in a Docker container.

By default, the Docker will expose port 8080, so change this within the
Dockerfile if necessary. When ready, simply use the Dockerfile to
build the image.

```sh
cd dillinger
docker build -t <youruser>/dillinger:${package.json.version} .
```

This will create the dillinger image and pull in the necessary dependencies.
Be sure to swap out `${package.json.version}` with the actual
version of Dillinger.

Once done, run the Docker image and map the port to whatever you wish on
your host. In this example, we simply map port 8000 of the host to
port 8080 of the Docker (or whatever port was exposed in the Dockerfile):

```sh
docker run -d -p 8000:8080 --restart=always --cap-add=SYS_ADMIN --name=dillinger <youruser>/dillinger:${package.json.version}
```

> Note: `--capt-add=SYS-ADMIN` is required for PDF rendering.

Verify the deployment by navigating to your server address in
your preferred browser.

```sh
127.0.0.1:8000
```

## License

MIT

**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
