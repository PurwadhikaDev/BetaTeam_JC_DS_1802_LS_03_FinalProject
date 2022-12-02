# BetaTeam_JC_DS_1802_LS_03_FinalProject

# **E-commerce Customer Churn Analysis and Prediction**

By:
1. Nadia Puspitasari
2. Matthew Nicholas Lengkey
3. Rayhan Romy Syahputra

### Context

Sebuah perusahaan X memiliki bisnis di bidang E-Commerce dimana pembeli dan penjual bisa bertransaksi (melakukan penjualan/pembelian) melalui website tersebut. Transaksi pembelian dari website E-Commerce bisa datang dari berbagai kategori barang/jasa. Perusahaan X mendapatkan keuntungan dari tiap transaksi yang dilakukan oleh customer, sehingga adanya pertumbuhan customer dibutuhkan agar perusahaan bisa mendapatkan lebih banyak keuntungan. Di beberapa tahun terakhir perusahaan mengalami pertumbuhan customer yang cukup baik, namun dari data terbaru mulai menunjukkan bahwa adanya peningkatan customer yang churn dari keseluruhan customer di website E-Commerce tersebut. 

Sebagai konteks, ada dua cara agar perusahaan dapat mempertahankan pertumbuhan keuntungan. Pertama yaitu mempertahankan customer lama agar menetap sebagai customer. Cara kedua yaitu mencari customer baru. Berdasarkan statistik dari berbagai industri bisnis, hasil riset menemukan bahwa customer acquisition memiliki biaya 4-5x lipat lebih dari customer retention

Pada kesimpulannya, mendapatkan customer baru memakan biaya yang lebih banyak dibandingkan dengan mempertahankan customer lama untuk tidak churn sehingga kita harus lebih fokus ke customer retention. Sehingga, perusahaan harus memikirkan cara untuk memprediksi customer yang berpotensi untuk churn dan memberikan treatment yang diperlukan agar customer menetap di platform E-Commerce perusahaan.

Customer Retention Cost List:
- Staff costs for customer service, customer success, and account management
- Marketing costs focused on customer retention
- Customer loyalty programs

Kita asumsikan perusahaan per tahunnya menggelontorkan dana $100,000 untuk memaintain 1000 customer lama

Sehingga:

- **Retention Cost: $100/customer**

Sedangkan, customer acuqisition cost yang dihabiskan oleh perusahaan sebanyak $450,000 untuk mendapatkan 1000 customer baru

Sehingga:

- **Acquisition Cost: $450/customer**

### Problem Statement

Salah satu tantangan yang dihadapi oleh bisnis E-Commerce adalah untuk membuat customer agar menetap dan tetap melakukan transaksi. **Perusahaan mengalami penurunan pertumbuhan customer akibat customer churn yang mengakibatkan keuntungan perusahaan stagnan/berkurang**.

### Goals

Berdasarkan masalah yang dihadapi, perusahaan harus bisa **memprediksi customer mana saja yang berpotensi untuk churn, lalu memberikan treatment yang tepat untuk customer tersebut agar tidak churn**. Sehingga perusahaan bisa **mempertahankan keuntungan** yang telah didapatkan. Juga, **meminimalisir retention cost yang diperlukan** untuk customer yang mau melakukan churn.

### Analytic Approach

Kita akan menganalisis data untuk menemukan pola yang membedakan customer yang akan churn atau yang tidak churn. Kemudian kita akan membangun model klasifikasi yang akan membantu perusahaan untuk dapat memprediksi probabilitas customer akan churn atau tidak.

### Metric Evaluation

0: customer tidak churn
1: customer churn

- TP: customernya aktualnya churn dan diprediksi churn
- TN: customernya aktualnya tidak churn dan diprediksi tidak churn
- FN: customernya aktualnya churn dan diprediksi tidak churn
- FP: customernya aktualnya tidak churn dan diprediksi churn

Cost FN (False Negative):

(Kekurangan)
- Kehilangan customer (alias churn)
- Adanya cost customer acquistion untuk menggantikan customer yang telah churn   

Cost FP (False Positive):

(Kelebihan)
- Akibat dari salah treatment terhadap customer yang sebenarnya tidak churn tapi diprediksi churn, maka reputasi E-Commerce semakin baik (customer yang tidak churn akan mengira bahwa plaform E-Commerce murah hati untuk memberikan promo secara cuma-cuma)

(Kekurangan) 
- Salah target treatment untuk customer yang tidak churn (tapi diprediksi churn)
- Sia-sianya biaya customer retention, waktu dan sumber daya

Berdasarkan konsekuensinya, maka sebisa mungkin yang akan kita lakukan adalah membuat model yang dapat mengurangi cost customer retention dari perusahaan tersebut tetapi tanpa harus ada customer yang churn dari website E-Commerce perusahaan.

Oleh karena itu, kita memutuskan untuk menitikberatkan ke False Negative, tetapi juga tidak lupa dengan False Positive, dengan lebih menitikberatkan pada recall Maka dari itu focus metric yang kita gunakan adalah **F2-Score**

Reference:

- https://www.clv-calculator.com/customer-costs/retention-costs-clv/retention-cost-formula/
- https://www.paddle.com/resources/customer-acquisition-vs-retention
- https://www.huify.com/blog/acquisition-vs-retention-customer-lifetime-value

## Data Understanding

Dataset source: [Link](https://www.kaggle.com/datasets/ankitverma2010/ecommerce-customer-churn-analysis-and-prediction)

Note:
- Target dalam dataset tidak seimbang
- Feature dalam dataset terbagi menjadi data numerical (integer/float) dan categorical.
- Setiap baris data merepresentasikan data customer dan transaksinya dalam sebuah platform e-commerce.
<br>

**Attribute Information**

| Attribute | Data Type, Length | Description |
| --- | --- | --- |
| CustomerID | Integer | Unique customer ID |
| Tenure | Float | Tenure of customer in organization |
| PreferredLoginDevice | Text | Preferred login device of customer |
| CityTier | Integer | City tier |
| WarehouseToHome | Float | Distance in between warehouse to home of customer |
| PreferredPaymentMode | Text | Preferred payment method of customer |
| Gender | Text | Gender of customer |
| HourSpendOnApp | Float | Number of hours spend on mobile application or website |
| NumberOfDeviceRegistered | Integer | Total number of devices registered on a particular customer |
| PreferedOrderCat | Object | Preferred order category of customer in last month |
| SatisfactionScore | Integer | Satisfactory score of customer on service |
| MaritalStatus | Object | Marital status of customer |
| NumberOfAddress | Integer | Total number of address added on particular customer |
| Complain | Integer | Any complaint has been raised in last month |
| OrderAmountHikeFromlastYear | Float | Percentage increases in order from last year |
| CouponUsed | Float | Total number of coupon has been used in last month |
| OrderCount | Float | Total number of orders has been places in last month |
| DaySinceLastOrder | Float | Day since last order by customer |
| CashbackAmount | Float | Average cashback in last month |
| Churn | Integer | 0 - customer who have not churned; 1 customer who churned |

## Exploratory Data Analysis

Setelah dibuatkannya visualisasi atas distribusi data dan korelasi data antar variabel, selanjutnya akan dilakukan data analisis. Sebelum dilakukan analisis, kami memiliki asumsi-asumsi terkait analisis customer churn ini. Adapun asumsi kami akan dituangkan dalam beberapa pertanyaan sebagai berikut:
- Apakah customer yang berhenti menggunakan langganan ecommerce berhenti di awal bulan penggunaan layanan?
- Apakah customer yang mengajukan complain cenderung berhenti menggunakan layanan ecommerce?
- Apakah angka kepuasan yang rendah akan menunjukkan tingkat churn yang tinggi?
- Apakah customer yang keluar dari layanan ecommerce tidak lagi melakukan order pembelanjaan seminggu terakhir?
- Bagaimana pembelian produk dari customer yang churn? Apakah berpengaruh dari cashback yang didapatkan?
- Apakah ada metode pembayaran tertentu yang berhubungan dengan customer churn?
- Apakah customer yang login menggunakan handphone lebih banyak yang berhenti dari layanan ecommerce?
- Apakah customer laki-laki dan customer yang sudah menikah yang lebih banyak berhenti dari layanan ecommerce?

## Preprocessing

Tahapan preprocessing yang dilakukan sebelum modeling
- Remove Data Duplication
- Fill Missing Values
- Encoding
- Scaling

## Modeling 

Analisis lengkap terlampir pada notebook.

## Conclusion and Recommendation

### Conclusion
Berdasarkan hasil classification report dari model kita, kita dapat menyimpulkan/mengambil konklusi bahwa bila seandainya nanti kita menggunakan model kita untuk memprediksi customer yang churn atau tidak, berdasarkan hasil f2score oleh model LGBM, kita mendapatkan akurasi f2score 0.849 sebelum hyperparameter tuning. Setelah melakukan hyperparameter tuning hasil meningkat menjadi 0.898. Untuk hasil yang tidak akurat, kita menitikberatkan ke False Negative karena lebih merugikan dibandingkan False Positive

Statistik Churn: 
- 0: 2152
- 1: 418
- **Total data**: 2570

Tanpa model:
Tanpa model, kita sulit untuk mengetahui customer mana yang churn atau tidak, Maka perhitungannya:
- Total customer yang pasti churn setelah diberi promo => 418 orang sehingga:
- Acquisition cost untuk menggantikan customer yang churn:
 418 * 450 USD = 188100 USD
- Total Biaya --> 188100 USD

Perbandingan dengan sampel (20%/100%)
- 188100 USD * 20% = **37620 USD**
- Jumlah penghematan => 0 USD

sehingga potensi biaya promosi yang dikeluarkan menjadi lebih banyak.

Dengan model (Test set yakni 20%):

**Total data**: 514

Berdasarkan confusion matrix:
- Biaya untuk promosi => (76+11) * 100 USD = 8700 USD
- Acquisition cost untuk menggantikan customer yang churn:
    8 * 450 USD = 3600 USD
- Total Biaya --> 8700 USD + 3600 USD = **12300 USD**
- Jumlah penghematan => **37620 USD - 12300 USD = 25320 USD**

Berdasarkan perhitungan di atas, dengan menggunakan model yang telah dibuat, perusahaan bisa jauh lebih menghemat biaya yang dikeluarkan. Lebih baik mengeluarkan biaya untuk retention cost daripada lebih berpotensi untuk kehilangan customer (churn).

### Recommendation
- Untuk Modeling:
  - Menambah fitur-fitur/kolom-kolom baru yang memiliki keterkaitan dengan potensi customer untuk churn.
  - Melakukan cohort analysis untuk melihat customer yang churn berdasarkan periode akses aplikasi/periode transaksi customer.
  - Menambah sampel pada dataset agar model dapat memiliki banyak referensi sehingga prediksi bisa menjadi lebih tepat.
  - Mencoba algoritma machine learning lainnya dan melakukan hyperparameter tuning.
  - Menganalisa data-data yang model yang salah prediksi untuk mengetahui alasannya dan karakteristiknya bagaimana.

- Untuk Bisnis:
  - Membuat marketing campaign yang lebih segmented (terutama untuk customer dengan karakteristik churn lebih tinggi, misal customer laki-laki, customer yang berstatus single) agar penggunaan budget marketing lebih sesuai juga.
  - Mereview UI/UX dari aplikasi ecommerce baik via computer/handphone agar dapat memberikan pengalaman yang lebih baik bagi customer saat berbelanja.
  - Memberikan lebih banyak promo dan support customer yang baik di awal bagi new customer agar dapat meningkatkan lamanya periode customer menggunakan layanan ecommerce.
  - Mempertimbangkan pemberian cashback (khususnya pada pembeli mobile phone) yang lebih banyak agar mengurangi jumlah customer yang churn.
  - Mereview kerjasama dengan pihak ketiga terkait support pembayaran COD guna meningkatkan experience yang lebih baik bagi customer.












