# Rekomendasi Aplikasi

## Project Overview : Rekomendasi Aplikasi Google Play Store
 
Aplikasi adalah perangkat lunak yang berisi program untuk menjalankan fungsi tertentu untuk mencapai tujuan dari pengguna. Aplikasi terdapat di berbagai sistem operasi seperti iOS, Android dan lain-lain. Google Play Store adalah toko aplikasi resmi untuk sistem operasi Android, terdapat banyak aplikasi yang dapat menunjang aktivitas pengguna sehari-hari dengan berbagai kategori seperti Game untuk hiburan, Education untuk belajar dan banyak kategori lainnya. Banyaknya aplikasi yang tersedia terkadang membuat kita tidak puas hanya dengan menggunakan satu aplikasi saja, kita ingin mencoba aplikasi lainnya namun ingin aplikasi tersebut memiliki kemiripan dengan aplikasi yang sudah kita pakai. Misal, kita telah memakai sebuah aplikasi Game tetapi karena bosan memainkan hal yang sama kita ingin mencoba aplikasi Game lainnya. Oleh karena itu, akan dibuat sistem Rekomendasi Aplikasi pada Google Play Store yang dapat merekomendasikan aplikasi yang serupa dengan aplikasi yang pernah digunakan.
 
## Business Understanding
Membuat sistem yang dapat merekomendasikan aplikasi yang mirip dengan aplikasi yang pernah digunakan. Menggunakan pendekatan Content Based Filtering pada sistem, karena rekomendasi yang akan dibuat berdasarkan aplikasi yang pernah digunakan.
 
### Problem Statement
- Bagaimana memberikan rekomendasi aplikasi yang mirip dengan aplikasi yang pernah digunakan oleh pengguna ?
 
### Goal
- Memberikan rekomendasi aplikasi yang memiliki kemiripan dengan aplikasi yang pernah digunakan.
 
### Solution approach
 
Untuk membuat sistem rekomendasi Aplikasi, teknik yang akan digunakan adalah **Content Based Filtering**. Content Based Filtering adalah pendekatan sistem rekomendasi dengan merekomendasikan item yang mirip dengan item yang disukai atau pernah diguankan oleh pengguna. Dalam kasus ini, sistem akan merekomendasikan aplikasi yang mirip dengan aplikasi yang pernah digunakan sebelumnya berdasarkan kategori pada aplikasi. Kelebihan dari sistem rekomendasi ini adalah sistem dapat merekomendasikan item terbaru atau yang bahkan belum pernah di-*rate* oleh siapaun. Dan tentunya juga memiliki kekurangan yaitu sistem memerlukan data item yang pernah digunakan oleh pengguna atau riwayat interaksi penguna dengan sistem rekomendasi.
 
## Data Understanding
Data yang akan digunakan adalah Dataset [Google Play Store Apps](https://www.kaggle.com/lava18/google-play-store-apps) yang berisi mengenai data aplikasi Google Play Store. File yang akan digunakan adalah **googleplaystore.csv**.
 
Fitur-fitur pada file Google Play Store adalah sebagai berikut :
- App : nama aplikasi
- Category : kategori aplikasi
- Rating : rating aplikasi
- Reviews : banyak review pada aplikasi
- Size : ukuran aplikasi
- Installs : jumlah *install* aplikasi
- Type : tipe aplikasi (gratis, berbayar)
- Price : harga aplikasi
- Content Rating : siapa saja yang dapat memakai aplikasi (teen, mature, dll)
- Genres : genre dari aplikasi
- Last Updated : kapan terakhir kali aplikasi di-update
- Current Ver : versi terkini aplikasi
- Android Ver : versi android yang dapat menjalankan aplikasi
 
Dan fitur yang akan digunakan hanya **App** dan **Category** karena kedua fitur ini yang penting sistem rekomendasi yang akan dibangun.
 
## Data Preparation
- Melakukan pengecekan *missing value*, hal ini dilakukan untuk memastikan tidak ada nilai yang kosong yang dapat mengakibatkan bias pada data. Walaupun tidak terdapat *missing value*, namun pada fitur Category terdapat nilai yang tidak sesuai yaitu nilai **'1.9'**. Kemungkinan hal ini terjadi karena salah input pada Category atau disebabkan oleh hal lain. Karena hanya satu App saja yang merupakan kategori **1.9**, maka nilai ini di-drop dari data. Sekarang fiur Category sudah sesuai dengan 33 kategori.
 
## Modeling
*Modeling* menggunakan Content Based Filtering. Tahap yang dilakukan pada proses *modeling* adalah :
1. TF-IDF Vectorizer :
-- Menggunakan fungsi tfidfvectorizer(). Dimulai dengan meng-inisialisasi fungsi, kemudian melakukan perhitungan idf dan melakukan mapping.
-- Fit dan transformasi ke dalam matriks, hasilnya terdapat matriks dengan ukuran (10840, 33) yang berarti 10840 ukuran data dan 33 kategori aplikasi.
-- Menggunakan fungsi todense pada matriks tadi untuk menghasilkan vektor dalam bentuk matriks.
-- Menampilkan matriks yang berisi kolom Category dan baris App dengan membuat dataframe. Jika nilai matriks 1.0, artinya baris yang berisi nama App merupakan kategori dari nama kolom yang berisi Category.
2. Menghitung dan menampilkan Cosine Similarity antara App yang menghasilkan angka 0 dan 1. Jika pertemuan baris dan kolom bernilai 1, maka baris dan kolom tersebut berkolerasi dan sebaliknya. Contohnya, App *AB Screen Recorder* berkorelasi dengan App *BRL AG*.
3. Membuat fungsi rekomendasi dengan parameter nama_app, similarity_data, items (App dan Category), dan k=10 (10 rekomendasi yang akan diberikan). Fungsi ini yang nantinya akan menghasilkan 10 rekomendasi berdasarkan kemiripan aplikasi dengan aplikasi yang pernah digunakan.
4. Pada model dicoba mendapatkan rekomendasi dengan App *Sketch - Draw & Paint* sebagai aplikasi yang pernah digunakan dan aplikasi ini termasuk ke dalam kategori *ART_AND_DESIGN*. Hasilnya sistem rekomendasi mampu menghasilkan 10 rekomendasi aplikasi yang semuanya merupakan kategori *ART_AND_DESIGN*.
Hasil rekomendasinya :
![result](https://raw.githubusercontent.com/KurniaTanggang/recommendation/main/image.png)
Dari 10 item yang direkomendasikan, semuanya memiliki kategori **ART_AND_DESIGN** yang similiar dengan aplikasi sebelumnya. 
 
## Evaluation
Metric yang digunakan pada model adalah Precision. Precision mengevaluasi seberapa tepat sistem dapat merekomendasikan item dengan membagi item yang relevan dengan item yang direkomendasikan. Jika nilainya semakin mendekati 1 maka semakin baik juga sistem rekomendasi yang telah dibangun.
 
Formula pada *precission*  :
 
```sh
precission = #of recommendation that are relevant/#of item we recommend
```
Dengan nilai terbaik adalah 1 dan nilai terburuk adalah 0.
 
Precision pada sistem rekomendasi ini tidak bisa dihitung dengan memanggil library karena tidak ada data target/label seperti supervised learning. Sehingga untuk menghitungnya, kita langsung menerapkan formula ke dalam code.
 
Pada sistem rekomendasi aplikasi yang dibangun dan hasil yang telah didapatkan, **precision**-nya bernilai **10/10** dikarenakan dari 10 item yang direkomendasikan semuanya similiar dengan aplikasi sebelumnya.
Maka penerapan Precision pada kode adalah sebagai berikut :
```
relevant = 10
item_recommended = 10
precision = relevant/item_recommend
print("Hasil Precision = ", precision)
```
```
Hasil Precision =  1.0
```
Dari model yang dibangun dan evaluasi yang dilakukan disimpulkan bahwa model yang dibangun sudah baik dan model dapat merekomendasikan 10 Aplikasi yang similiar dengan Aplikasi yang pernah digunakan sebelumnya.
