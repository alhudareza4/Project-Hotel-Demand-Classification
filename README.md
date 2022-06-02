# Project-Hotel-Demand-Classification
## Created By : Alhuda Reza Mahara

## Business Problem Understanding
### Context

Sebuah perusahaan yang bergerak di bidang perhotelan ingin mengetahui dan mengatasi permasalahan yang menyebabkan para konsumen melakukan pembatalan pesanan hotel.

Target :

0 : Konsumen tidak melakukan pembatalan pesanan hotel

1 : Konsumen melakukan pembatalan pesanan hotel

### Problem Statement

Pembatalan pesanan hotel yang dilakukan oleh konsumen dapat menyebabkan opportunity lost yang diakibatkan karena kamar yang harusnya ditempati tapi tidak jadi ditempati oleh konsumen.

### Goal 

Maka berdasarkan permasalahan tersebut, ingin memiliki kemampuan untuk memprediksi kemungkinan konsumen melakukan pembatalan pesanan hotel, sehingga perusahaan dapat memfokuskan sumber daya yang ada pada pesanan yang memiliki potensi yang lebih besar untuk tidak melakukan pembatalan pesanan dan tetap menjaga kepercayaan konsumen.

dan juga, perusahaan ingin mengetahui apa yang membuat konsumen akan melakukan pembatalan pesanan hotel atau tidak, sehingga perusahaan dapat membuat rencana yang lebih baik dalam mengurangi pembatalan pesanan yang dilakukan oleh konsumen.

### Analytic Approach

Yang akan kita lakukan adalah melakukan analisis data untuk menemukan pola yang membedakan pesanan yang berpotensi dibatalkan oleh konsumen.

kemudian kita akan membangun model klasifikasi yang akan membantu perusahaan perhotelan untuk dapat memprediksi probabilitas konsumen yang akan membatalkan pesanannya atau tidak.

### Metric Evaluation

![matrix](https://user-images.githubusercontent.com/47660678/171606181-0774e74c-b141-4ead-9e3a-b31aa4af5787.jpg)

Type 1 error : False Positive
Konsekuensi: Kehilangan kepercayaan konsumen

Type 2 error : False Negative
Konsekuensi: potensi kehilangan konsumen potensial (Banyak Kamar Kosong)

Berdasarkan konsekuensinya, maka sebisa mungkin yang akan kita lakukan adalah membuat model yang dapat mengurangi potensi kehilangan konsumen potensial, tetapi tanpa menimbulkan efek kehilangan kepercayaan konsumen. Jadi pada kasus kali ini kita akan mencoba menyeimbangkan hasil dari precision dan recallnya menggunakan matriks F1-Score dari kelas positive (is_canceled). Jadi nanti metric utama yang akan kita gunakan adalah F1-Score.

### Data Understanding

Dataset Source : https://drive.google.com/file/d/1eeXOEY5B_zfSNhWeDDqPWCo9hAyDRAbc/view?usp=sharing

Note :

- Dataset Tidak Seimbang
- Sebagian data bersifat kategorikal
- Setiap baris data merepresentasikan informasi pesanan Hotel

![image](https://user-images.githubusercontent.com/47660678/171606486-64fb0000-911d-46ca-837d-26c4c8e02601.png)

### Data Cleaning

Dari data yang akan digunakan tidak terdapat data yang mengalami kesalahan format dan juga kita tidak memeriksa data yang memiliki duplikat karena data yang digunakan tidak memiliki kode unik yang membedakan satu pesanan dengan pesanan lainnya, Terdapat data yang hilang/kosong di kolom country, maka kita akan menghandle data yang hilang/kosong dan membuatnya menjadi siap pakai untuk melakukan analisa terhadap masalahnya dan juga pembuatan model machine learningnya.

Missing Value pada data tidak memiliki pola sehingga kita hanya mengganti missing value dengan modus:

![image](https://user-images.githubusercontent.com/47660678/171607116-bf180987-892b-47b3-8353-ba23b2a95557.png)


Berikut Data sebelum dibersihkan:

![image](https://user-images.githubusercontent.com/47660678/171606665-c1c51e67-ce2c-4b03-bb45-d454f32f8331.png)

Berikut Data yang sudah dibersihkan:

![image](https://user-images.githubusercontent.com/47660678/171606926-7518c1a6-70f3-4291-9161-1d4a054ee6ff.png)

### Data Analysis
1. Data Cancel berdasarkan tipe deposit

![image](https://user-images.githubusercontent.com/47660678/171607355-90c75546-8653-4774-b566-44f9f67d57a4.png)

Dari Grafik diatas dapat dilihat bahwa konsumen yang memesan kamar dengan deposit_type No deposit memiliki jumlah pesanan yang tidak dibatalkan paling tinggi begitu juga dengan pesanan yang dibatalkan. hal ini mengindikasikan bahwa deposit_type ini tidak mempengaruhi konsumen untuk melakukan pembatalan pesanan. sedangkan kamar dengan deposit_type Refundable memiliki jumlah pembatalan yang sangat kecil dan jumlah yang tidak melakukan pembatalan relatif lebih besar. sedangkan kamar dengan deposit_type Non Refund memiliki jumlah pembatalan yang relatif sangat besar apabila dibandingkan dengan jumlah yang tidak melakukan pembatalan. hal ini mengindikasikan hubungan yang cukup kuat antara konsumen yang ingin melakukan pembatalan dengan pemilihan hotel dengan deposit_type Non Refundable. untuk pembuktian lebih lanjut akan kita periksa difeature importances dari model.

2. Sepuluh Negara terbanyak Pemesan layanan hotel

![image](https://user-images.githubusercontent.com/47660678/171607573-6cbff457-16a5-4348-8ed8-bdbf55663835.png)

Dari grafik diatas menunjukan bahwa Portugal dengan code PRT merupakan negara terbanyak yang melakukan pesanan hotel disusul dengan Inggris dengan Code GBR. mayoritas pemesan dari negara Portugal ini bisa mengindikasikan bahwa data pesanan hotel yang kita gunakan adalah data pesanan hotel dalam negeri negara Portugal, hal ini dapat disimpulkan pesanan dari Negara Portugal Mencakup 3 kali lipat dari seluruh pesanan hal ini merupakan pesanan dari penduduk Portugal itu sendiri.

3. Hubungan Jumlah pesanan space parkir yang dipesan dengan jumlah pembatalan

![image](https://user-images.githubusercontent.com/47660678/171608060-6bff72eb-db69-4bf2-9880-4efa0911721c.png)

konsumen yang tidak melakukan request space parkir mobil menunjukan tingkat pembatalan dan tidak melakukan pembatalan kamar hotel. hal ini mengindikasikan bahwa konsumen yang tidak melakukan request space parkir mobil tidak memiliki pengaruh untuk melakukan pembatalan pesanan. berbeda dengan konsumen yang melakukan request space parkir mobil cenderung tidak melakukan pembatalan pesanan hotel. hal ini menunjukan konsumen yang melakukan request space parkir memiliki hubungan yang cukup kuat untuk tidak melakukan pembatalan pesanan hotel, yang pembuktian lebih lanjutnya akan kita lakukan pada Feature Importances.

### Preprocessing Scheme


- Target : is_canceled
- OneHotEncoding : deposit_type, customer_type
- BinaryEncoder : country, market_segment, reserved_room_type
- SimpleImputer Mode : country
- passthrough : previous_cancellations, booking_changes, days_in_waiting_list, required_car_parking_spaces , total_of_special_requests

### Modelling & Evaluation

Kita akan mencoba memilih model yang akan digunakan berdasarkan F1 Score yang paling tinggi dan Standard Deviasi yang paling stabil.

![image](https://user-images.githubusercontent.com/47660678/171608445-a1fabac9-b193-4240-89b3-4b7533779abd.png)

Berdasarkan Modelling ini, model XGBoost merupakan model dengan score Mean F1 Paling Tinggi dan juga nilai standard deviasi yang relatif lebih stabil dari model lainnya.

Lalu kita coba Benchmarking dengan data test dengan hasil sebagai berikut:

![image](https://user-images.githubusercontent.com/47660678/171608790-460e0cd4-e2b1-4624-b787-4998276024fa.png)

Terlihat kembali bahwa model XGBoost adalah yang terbaik performanya pada test data.

untuk kedepannya proses klasifikasi yang digunakan adalah model XGBoost.

### Handling Imbalance

![image](https://user-images.githubusercontent.com/47660678/171609097-595095bb-a4bf-4853-84f4-b5bc2bbb9640.png)

Proporsi data target menunjukan bahwa data tidak seimbang (Imbalance) sehingga selanjutnya kita akan membandingkan metode resampling oversampling, undersampling dan gabungan dari keduanya untuk mendapat kan hasil metric score yang terbaik.
kita akan mencoba kembali memilih metode resample berdasarkan F1 Score paling tinggi dan Standard Deviasi paling stabil.

![image](https://user-images.githubusercontent.com/47660678/171609309-cde84175-4369-4624-9036-90ac2abd6f80.png)

Berdasarkan Perbandingan dari metode Resampling yang digunakan, metode SMOTETomek menunjukan peningkatan Score mean F1 yang cukup signifikan dari nilai awal dan juga menunjukan nilai standard deviasi yang cukup rendah apabila dibandingkan dengan metode lainnya.

### Hyperparameter Tuning

Selanjutnya kita akan melakukan Hyperparameter Tuning pada parameter n_estimator, learning_rate, max_leaves dan max_depth.
Dengan Default Hyperparameter XGBoost => 'learning_rate' = 0.3, 'n_estimators' = 100, 'max_depth' = 6, 'max_leaves' = 0
Dari proses ini didapat Hyperparameter terbaik XGBoost dari hasil Gridsearch => learning_rate = 0.4, max_depth = 6, max_leaves = 0, n_estimators = 100.

Untuk hyperparameter max_leaves default valuenya tetap yang terbaik, untuk learning_rate yang optimal dari gridsearch kali ini adalah 0.4 dari nilai default 0.3, untuk max_depth yang optimal adalah 6 dari nilai default 8 dan untuk n_estimators yang optimal dari gridsearch kali ini adalah 150 dari nilai default 100.

### Perbandingan Score Sebelum dan Sesudah Tuning

Dari proses perbandingan ini didapat :

![image](https://user-images.githubusercontent.com/47660678/171609833-e4a9841b-03a2-4c62-8037-effa030415f4.png)

Terlihat bahwa model XGBoost setelah kita tuning hyperparameternya memiliki nilai F1 Score yang lebih baik walaupun hanya naik sekitar 2 % saja.

Mari kita lihat juga perbandingan classification reportnya.

![image](https://user-images.githubusercontent.com/47660678/171609904-da253935-5e9a-4628-b35f-b8c8422cb27e.png)

Kembali lagi terlihat bahwa model XGBoost setelah kita tuning hyperparameternya memiliki classification report yang lebih baik walaupun hanya lebih baik sedikit saja. Oleh karena itu kita akan menggunakan model XGBoost yang sudah di tuned sebagai model akhir kita.

### Feature Importances

![image](https://user-images.githubusercontent.com/47660678/171610001-aed1e6ed-800b-4f5f-9d16-641b60c91be9.png)

Terlihat bahwa ternyata untuk model XGBoost kita, fitur/kolom non Refund adalah yang paling penting, kemudian diikuti dengan required_car_parking_spaces, total_of_special_request dan seterusnya.

Dari hasil Data Analysis sebelumnya telah terbukti dari Feature Importances ini bahwa Data Non Refund dan Required car parking spaces memiliki hubungan yang kuat untuk melakukan klasifikasi konsumen mana saja yang akan melakukan pembatalan pesanan.

### Conclusion & Recomendation

Berdasarkan hasil classification report dari model kita, kita dapat menyimpulkan/mengambil konklusi bahwa bila seandainya nanti kita menggunakan model kita untuk mengklasifikasikan konsumen yang akan melakukan pembatalan pesanan, maka model kita dapat mengurangi 74,2 % konsumen yang kemungkinan besar membatalkan pesanan hotel, dan model kita dapat mendapatkan 84% konsumen yang memang berniat tidak melakukan pembatalan pesanan. (semua ini berdasarkan F1 Scorenya)

Model kita ini memiliki ketepatan prediksi Konsumen yang melakukan pembatalan sebesar 70 % (precisionnya), jadi setiap model kita memprediksi bahwa konsumen akan melakukan pembatalan pesanan hotel, maka kemungkinan tebakannya benar itu sebesar 70% kurang lebih. Maka masih akan ada konsumen yang sebenarnya tidak ingin melakukan pembatalan pesanan tetapi diprediksi sebagai konsumen yang melakukan pembatalan pesanan sekitar 20 % dari keseluruan konsumen yang melakukan pemesanan hotel (berdasarkan recall).

Bila seandainya rata-rata biaya hotel permalam diportugal itu 75$ (berdasarkan sumber dari https://championtraveler.com/price/cost-of-a-trip-to-portugal/),
dan andaikan jumlah pesanan hotel kita miliki untuk suatu kurun waktu sebanyak 200 orang (dimana andaikan 20 orang melakukan pembatalan, dan 180 orang lagi tidak melakukan pembatalan), maka hitungannya kurang lebih akan seperti ini :

Tanpa Model (semua kandidat kita check dan tawarkan) :
- Total Biaya => 200 x 75 USD = 15000 USD
- Total Konsumen yang melakukan pembatalan => 20 orang 
- Total Konsumen yang tidak melakukan pembatalan => 180 orang 
- Biaya yang terbuang => 20 x 75 USD = 1500 USD
- Jumlah opportunity lost => 1500 USD

Dengan Model () :
- Potens Penghematan => (16 x 75 USD) + (4 x 75 USD) = 1200 USD + 300 USD = 1500 USD
- Total konsumen yang melakukan pembatalan => 16 orang (karena recall 1/yg tertarik itu 79%)
- Total konsumen yang tidak melakukan pembatalan => 24 orang (karena recall 1/yg tertarik itu 79%)
- Biaya yang terbuang => 4 x 75 USD = 300 USD (berdasarkan recall 0/))
- Jumlah penghematan => 1500 x 300 USD = 1200 USD 

Berdasarkan contoh hitungan tersebut, terlihat bahwa dengan menggunakan model kita, maka perusahaan tersebut akan menghemat biaya yang cukup besar tanpa mengorbankan tingkat kepercayaan konsumen.

Recomendation
Hal-hal yang bisa dilakukan untuk meningkatkan kemungkinan konsumen untuk tidak melakukan pembatasan pesanan dan meningkatkan kemampuan model, yaitu :

- Menyediakan dan mempromosikan ekstra parking space bagi para konsumen hotel  sehingga menarik para konsumen yang berpeluang tidak melakukan pembatalan pesanan hotel.
- Menghapus pesanan hotel dengan deposit_type Non Refundable
- Menggencarkan Promosi untuk Market Segment Online TA
- Menambahkan promosi dan fitur2 baru yang kemungkinan bisa berhubungan dengan special request untuk para konsumen.





