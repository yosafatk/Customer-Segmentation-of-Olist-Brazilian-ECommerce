# <h1><b>Customer Segmentation of Olist Brazilian ECommerce</b></h1>

**By: Beta Group**

1. Yosafat Kurniawan
2. Hari Prasetyo
***
## **Problem Statement**
![Olist Data Schema](https://static-s.aa-cdn.net/img/gp/20600011714266/eqLTXWdyygKUf85JsCXmcLSr1GnoYNLJfFVCmY-N8xGFr2T3PWwNcFdJ2Sx7MwcO6ac?v=1)

**Context**

Dataset ini disediakan oleh Olist, department store terbesar di pasar Brazil. Olist menghubungkan bisnis kecil dari seluruh Brazil ke seluruh jaringan tanpa kerumitan dan dengan satu kontrak. Penjual dapat menjual produknya melalui Olist Store dan mengirimkannya langsung ke pelanggan menggunakan mitra logistik dari Olist. Setelah konsumen membeli produk dari Olist Store, penjual akan diberi notifikasi untuk memenuhi pesanan tersebut. Setelah pelanggan menerima produk atau perkiraan tanggal pengiriman, pelanggan mendapatkan survei kepuasan melalui email dimana pelanggan dapat memberikan catatan terhadap pengalaman pembelian dan juga menuliskan komentar.

**Problem Statement**

Bisnis ecommerce tentunya membutuhkan transaksi dari konsumen-konsumennya secara berkelanjutan agar dapat bertahan. Olist Ecommerce perlu mengetahui seperti apa karakteristik dan perilaku dari konsumennya. Dengan demikian, Olist Ecommerce akan dapat menentukan strategi apa yang perlu diambil ke depannya dalam kegiatan pemasarannya.

Segmentasi dan target pasar merupakan salah satu fondasi dasar dalam bisnis Ecommerce. Berdasarkan artikel yang diterbitkan oleh [Agenciaeplus](https://agenciaeplus.com.br/en/e-commerces-brasileiros-marketing/), bahwa E-Commerce di Brazil menghabiskan 13% dari revenue-nya untuk kebutuhan pemasaran. Dengan melakukan prediksi segmentasi pasar, kita dapat mengarahkan strategi kegiatan pemasaran kepada segmen/kelompok konsumen yang disesuaikan dengan karakteristiknya, sehingga kegiatan pemasaran bisa lebih efektif dan efisien. Segmentasi pasar dapat ditinjau dari berbagai sudut pandang, seperti kemampuan bayar, demografi, waktu, dll.

**Goals**

Berdasarkan permasalahan tersebut, perusahaan Olist ingin memiliki kemampuan 
untuk memprediksi segmentasi dari konsumen-konsumennya, sehingga perusahaan dapat melakukan kegiatan pemasaran dengan efektif dan meningkatkan penjualannya.

**Analytic Approach**

Beberapa metode analitik yang akan digunakan yaitu pertama melakukan analisis RFM dan kedua melakukan Clustering dengan metode KMeans. Dengan menggunakan Clustering Kmeans, dapat diperoleh segmentasi konsumen sesuai dengan kemiripan karakteristiknya. Dengan analisis RFM, kita dapat melihat segmen konsumen juga yang ditinjau dari karakteristik Recency, Frequency, dan Monetary nya. Pada akhirnya di tiap metode yang dilakukan akan dapat ditentukan kegiatan pemasaran yang sesuai untuk masing-masing segmen konsumen.

**Metric Evaluation**

Evaluasi metrik yang akan digunakan adalah elbow method dan silhouette score. Kedua metode tersebut digunakan untuk melihat berapa jumlah cluster yang optimal untuk clustering. Elbow method dapat melihat perubahan tingkat kemiripan setiap penambahan satu cluster. Jumlah cluster yang dipilih adalah jumlah cluster dimana tingkat kemiripan clusternya berubah dari perubahan yang signifikan menjadi tidak signifikan. Pada grafik Elbow Method dapat dilihat pada titik yang membentuk siku. Sedangkan Silhouette score pada dasarnya suatu ukuran yang mengkombinasikan seberapa dekat setiap data poin dalam suatu cluster yang sama dengan seberapa dekat setiap data poin pada cluster tersebut dengan data poin pada cluster lainnya. Jumlah cluster yang dipilih adalah yang memiliki nilai shilouete paling tinggi.
***

## **Data Understanding**
Dataset dapat diperoleh di situs [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).<br>
Data terdiri dari 9 dataset dengan skema seperti berikut.
![Olist Data Schema](https://imgur.com/HRhd2Y0.png)
**Attribute Information**

**olist_customers_dataset.csv**

| Columns Name | Data Type, Length | Description | 
| -- | -- | -- | 
| ***customer_id***| object |  ID Customer. (*Key to the orders dataset*)|
| customer_unique_id | object | ID unik pengidentifikasi customer.| 
| customer_zip_code_prefix | int64 | Lima digit pertama kode pos (*Format kode pos brazil: XXXXX-XXXX*)| 
| customer_city  |  object |Nama kota customer| 
| customer_state |  object |Nama negara bagian customer| 

<br>

**olist_geolocation_dataset.csv**

| Columns Name | Data Type, Length | Description| 
| -- | -- | -- | 
| geolocation_zip_code_prefix | int64 |Lima digit pertama kode pos| 
| geolocation_lat | float64 |Latitude| 
| geolocation_lng | float64 |Longitude| 
| geolocation_city |  object |Nama kota| 
| geolocation_state |  object |Nama negara bagian| 

<br>

**olist_order_items_dataset.csv**

| Columns Name | Data Type, Length | Description | 
| -- | -- | -- | 
| order_id  |  object |ID unik setiap order| 
| order_item_id | int64 | Nomor urut mengidentifikasi jumlah item yang termasuk dalam order yang sama| 
| product_id  |  object |ID unik setiap produk| 
| seller_id | object  |ID unik setiap penjual|
| shipping_limit_date| object  | Tanggal batas pengiriman penjual untuk menangani pesanan ke mitra logistik | 
| price | float64 |Harga item| 
| freight_value | float64 |Ongkos kirim (jika satu order memiliki lebih dari satu item, nilai ongkos kirim dibagi antara item)| 

<br>

**olist_order_payments_dataset.csv**

| Columns Name | Data Type, Length | Description | 
| -- | -- | -- | 
| order_id  |  object |ID unik setiap order|
| payment_sequential | int64 |Customer dapat membayar order dengan lebih dari satu metode pembayaran. Ini adalah urutan pembayarannya| 
| payment_type |  object |Metode pembayaran yang dipilih customer.| 
| payment_installments | int64 |Jumlah installments yang dipilih customer.| 
| payment_value  | float64 | transaction value.| 

<br>

**olist_order_reviews_dataset.csv**

| Columns Name | Data Type, Length | Description | 
| -- | -- | -- | 
| review_id | object | ID ulasan |
| order_id | object | ID unik setiap order |
| review_score |float64 |  Penilaian angka 1 sampai 5 yang diberikan oleh pelanggan pada satisfaction survey| 
| review_comment_title | object | Judul komentar oleh customer dalam bahasa Portugis |
| review_comment_message | object |  Isi komentar oleh customer dalam bahasa Portugis |
| review_creation_date | object| Tanggal pengiriman satisfaction survey kepada customer |
| review_answer_timestamp | object | Tanggal dan waktu jawaban survei kepuasan oleh customer |

<br>

**olist_orders_dataset.csv**

| Columns Name | Data Type, Length | Description | 
| -- | -- | -- | 
| order_id | object |  ID unik setiap order |
| customer_id | object |  ID Customer|
| order_status | object |  Status pesanan ('delivered', 'shipped', etc.) |
| order_purchase_timestamp  | object | Waktu pembelian |
| order_approved_at | object | Waktu persetujuan pembayaran |
| order_delivered_carrier_date | object | Tanggal pengiriman pesanan oleh mitra logistik |
| order_delivered_customer_date | object | Tanggal aktual penerimaan pesanan oleh customer |
| order_estimated_delivery_date | object | Tanggal perkiraan delivery yang diinformasikan kepada pelanggan pada saat pembelian |

<br>

**olist_products_dataset.csv**

| Columns Name | Data Type, Length | Description | 
| -- | -- | -- | 
| product_id | object | ID produk |
| product_category_name | float64| Nama kategori produk dalam bahasa Portugis |
| product_name_lenght | float64| Panjang karakter nama produk |
| product_description_length | float64| Panjang karakter deskripsi produk |
| product_photos_qty | float64| Jumlah foto produk |
| product_weight_g |float64 | Berat produk (g)|
| product_length_cm | float64| Panjang produk (cm) |
| product_height_cm |float64 | Tinggi produk (cm) |
| product_width_cm | float64|  Lebar produk (cm) |

<br>

**olist_sellers_dataset.csv**

| Columns Name | Data Type, Length | Description | 
| -- | -- | -- | 
| seller_id | object | ID penjual |
| seller_zip_code_prefix | int64 | Lima digit pertama kode pos penjual |
| seller_city | object | Kota penjual |
| seller_state | object | Negara bagian penjual |

<br>

**product_category_name_translation.csv**

| Columns Name | Data Type, Length | Description | 
| -- | -- | -- | 
| product_category_name | object | Nama kategori produk dalam bahasa Portugis |
| product_category_name_english | object | Nama kategori produk dalam bahasa Inggris |
***
## **Analytics**
Visualisasi Tableau dapat dilihat pada link berikut : [Tableau Dashboard](https://public.tableau.com/app/profile/hari.prasetyo/viz/FinalProject_16797569810040/Dashboard1)
<br>
<br>

![Tableau Dashboard 1](https://github.com/PurwadhikaDev/BetaGroup_JC_DS_OL_08_FinalProject/blob/main/Picture/Tableau%201.png)
<br>
<br>

#### Sales Overview
Jumlah order mengalami peningkatan dari tahun 2016 sampai 2018. Hal ini menunjukkan perusahaan Olist berkembang dengan baik. Jumlah order terbanyak terdapat pada November 2017 yaitu sebanyak 7188. Olist Ecommerce sendiri baru didirikan pada tahun 2015 . Artinya sebagai Ecommerce yang baru berdiri dan mengalami perkembangan yang cukup cepat dari tahun 2016-2018 menunjukkan bahwa Olist Ecommerce sudah cukup baik dalam menjangkau konsumen. Total Sales tahun 2018 mencapai R$ 11,113,007, **meningkat 20,4%** dari tahun 2017 walaupun tahun 2018 belum selesai.

Ref: https://www.crunchbase.com/organization/olist

<br>
<br>

![Tableau Dashboard 2](https://github.com/PurwadhikaDev/BetaGroup_JC_DS_OL_08_FinalProject/blob/main/Picture/Tableau%202.png)

#### Kategori Produk
Top 5 kategori produk berdasarkan jumlah penjualan berbeda dengan apabila berdasarkan quantity penjualannya. Artinya, Olist Ecommerce dapat menyesuaikan kategori produk yang ingin ditingkatkan sesuai dengan tujuannya (apakah meningkatkan quantity ataupun jumlah penjualan). Hal yang dapat dilakukan misalnya dengan memperbanyak variasi barang yang ditawarkan pada kategori-kategori tersebut dan juga bisa dengan memperbanyak merchant yang berkualitas di kategori produk tersebut
 Hal ini juga dapat meningkatkan Brand Image dari Olist Ecommerce sendiri, sehingga Olist Ecommerce dapat lebih dikenal dan meningkatkan penjualannya.

#### Waktu Transaksi
Jumlah transaksi tertinggi dimulai dari jam 10 pagi hingga jam 4 sore dan kemudian menurun. Jumlah order kembali naik pada jam 8 hingga jam 10 malam. Hal tersebut disebabkan pada jam tersebut orang-orang mulai aktif melakukan kegiatannya, termasuk berbelanja online. sedangkan pada jam 1 hingga jam 6 pagi adalah jumlah transaksi terendah disebabkan orang-orang sedang beristirahat.

#### Metode Pembayaran
Jumlah transaksi yang menggunakan metode pembayaran credit card mencapai 73,76%. Hal tersebut sejalan dengan artikel yang diterbitkan oleh [JPMorgan](https://www.jpmorgan.com/merchant-services/insights/reports/brazil), bahwa penggunaan kartu kredit cukup tinggi di Brazil, menunjukkan permintaan yang kuat terhadap kredit konsumen.
<br>
<br>
![Cohort](https://github.com/PurwadhikaDev/BetaGroup_JC_DS_OL_08_FinalProject/blob/main/Picture/Cohort.png)
<br>
<br>
#### Cohort Analysis
Berdasarkan hasil cohort analysis di atas yang menggunakan periode bulanan, diperoleh bahwa hanya kurang dari 1% customer yang kembali melakukan transaksi di bulan-bulan berikutnya. Hal ini sejalan dengan analisa pada poin 5.7 dimana mayoritas customer baru pernah bertransaksi sebanyak 1x saja di Olist Ecommerce. Tentunya hal ini perlu diperhatikan oleh Olist Ecommerce. 
***
## **Customer Segmentation**
### Recency, Frequency & Monetary

![RFM](https://github.com/PurwadhikaDev/BetaGroup_JC_DS_OL_08_FinalProject/blob/main/Picture/RFM.png)

Berdasarkan segmentasi RFM yang sudah dilakukan, diperoleh 6 segmen dengan 3 segmen yang dominan secara jumlah yaitu Recent Users, Lost Customers, dan Can't Lose Them, sedangkan 3 segmen lainnya yaitu Loyal Customers, About to Sleep, dan Champions jumlah nya tidak dominan. Berikut beberapa rekomendasi yang dapat dilakukan melalui kegiatan pemasaran :
1. Recent Users <br>
Jika dilihat dari data historical, Olist Ecommerce kesulitan untuk mempertahankan customernya (terbukti dari frekuensi transaksi dari unique customer mayoritas hanya 1x saja). Segmen Recent Users akan menjadi potensi yang baik bagi Olist Ecommerce dimana customer baru saja melakukan transaksi di Olist Ecommerce. Olist Ecommerce perlu tetap menjaga relationship nya dengan segmen Recent Users ini. 

2. Lost Customers <br>
Segmen ini merupakan customer yang churn. Untuk mendapatkan kembali customer ini perlu usaha yang lebih. 

3. Can't Lose Them <br>
Segmen ini perlu dimonitor dan diberi perlakuan khusus untuk dapat mempertahankannya.

4. Loyal Customers <br>
Segmen ini sudah puas dengan produk Olist namun perlu dijaga agar customer merasa dihargai.

5. About To Sleep <br>
Segmen ini sudah lama tidak melakukan transaksi dan sebentar lagi akan menjadi Lost Customers jika tidak diberi perlakuan apapun. 

6. Champions <br>
Segmen ini merupakan segmen terbaik yang dimiliki Olist Ecommerce sehingga sangat penting sekali untuk menjaga kepercayaan dari segmen ini.

### KMeans Clustering Using Combined Database and RFM
![Cluster](https://github.com/PurwadhikaDev/BetaGroup_JC_DS_OL_08_FinalProject/blob/main/Picture/Cluster.png)

- Cluster 0 (Low Spender)<br>
Seperti sudah dijelaskan sebelumnya cluster ini merupakan cluster dengan jumlah konsumen terbanyak. Cluster ini memiliki rata-rata spending paling kecil dilihat dari Monetary, Price, dan Freight Value. Customer State yang dominan yaitu SP dan Product Category yang dominan yaitu health_beauty. Untuk Payment Installments cluster ini memiliki nilai yang paling kecil juga. Hal ini wajar jika melihat rata-rata spending yang dikeluarkan juga relatif kecil dibanding cluster lainnya, sehingga konsumen tidak membutuhkan jumlah installments yang banyak. Payment type yang dominan yaitu Credit Card. Nilai Recency dari cluster ini adalah yang terbesar dibanding cluster lainnya. 

- Cluster 1 (Big Spender) <br>
Cluster ini memiliki rata-rata spending paling besar jika dilihat dari Monetary, Price, dan Freight Value. Customer State yang dominan yaitu SP dan Product Category yang dominan yaitu furniture_decor. Untuk Payment Installments cluster ini memiliki nilai yang paling besar. Hal ini wajar jika melihat rata-rata spending yang dikeluarkan juga relatif tinggi dibanding cluster lainnya, sehingga konsumen akan menyukai jumlah installments yang banyak. Payment type yang dominan yaitu Credit Card.  Nilai Recency dari cluster ini adalah yang terkecil dibanding cluster lainnya. 

- Cluster 2 (Medium Spender) <br>
Cluster ini memiliki rata-rata spending di tengah-tengah jika dilihat dari Monetary, Price, dan Freight Value. Customer State yang dominan yaitu SP dan Product Category yang dominan yaitu furniture_decor. Untuk Payment Installments cluster ini memiliki nilai di tengah-tengah dari ketiga cluster. Payment type yang dominan yaitu Credit Card.  Nilai Recency dari cluster ini adalah di tengah-tengah dari ketiga cluster yang ada. 
***

## Conclusion
Berdasarkan hasil analisa yang sudah dilakukan, diperoleh bahwa Olist Ecommerce memiliki masalah dimana hanya 3% konsumen yang melakukan transaksi lebih dari 1x. Dari Cohort Analysis juga diperoleh bahwa hanya kurang dari 1% konsumen yang kembali melakukan transaksi di bulan-bulan berikutnya. Hal ini menunjukkan bahwa Retention Customer yang masih minim dan masih terdapat potensi yang besar untuk Olist Ecommerce dapat mengembangkan bisnisnya dengan cara mempertahankan pelanggan yang lama. Berdasarkan informasi dari infografis "Customer Acquisition vs Retention Costs - Statistics And Trends" oleh Khalid [invesp](https://www.invespcro.com/blog/customer-acquisition-retention/), biaya untuk mendapatkan pelanggan baru adalah 5x lebih besar dibanding mempertahankan pelanggan lama serta probabilitas menjual ke pelanggan lama 60-70% sedangkan probabilitas menjual ke pelanggan baru hanya 5-20%. Dengan demikian, Retention Customer perlu menjadi fokus utama dari Olist Ecommerce untuk segera diperbaiki ke depannya.

Selanjutnya berdasarkan modeling yang sudah dilakukan menggunakan 2 metode diperoleh hasil sebagai berikut:
1. Metode RFM <br>
Dari Analisis Model RFM, diperoleh 6 segmen dengan 3 segmen yang dominan secara jumlah yaitu Recent Users, Lost Customers, dan Can't Lose Them, sedangkan 3 segmen lainnya yaitu Loyal Customers, About to Sleep, dan Champions jumlah nya tidak dominan. Hal ini berarti bahwa masih terdapat potensi bagi Olist Ecommerce dari segmen Recent Users dan Can't Lose Them.
2. Clustering KMeans using Combined Databased and RFM <br>
Dari hasil clustering diperoleh 3 cluster yaitu Big Spender, Medium Spender, dan Low Spender. Mayoritas konsumen didominasi oleh cluster Low Spender. Hal ini dapat menjadi fokus segmen bagi Olist Ecommerce untuk dapat mengembangkan bisnisnya.

Berdasarkan 2 model yang sudah dibuat, kami memilih menggunakan model Kmeans dimana KMeans memiliki kelebihan dibanding RFM yaitu KMeans menggunakan algoritma dalam penentuan cluster nya serta sudah melibatkan berbagai fitur dalam proses clusteringnya, sedangkan model RFM dibuat berdasarkan aturan tertentu yang ditetapkan berdasarkan domain knowledge.

## Recommendation

Olist Ecommerce perlu memberi perhatian lebih terhadap Retention dari pelanggan lamanya. Berdasarkan segmentasi hasil Clustering KMeans, kami pun memberikan rekomendasi berdasarkan tiap Cluster sebagai berikut:
- Cluster Low Spender <br>
  1. Memperbanyak opsi cicilan kartu kredit dengan berbagai macam bank.
  2. Memberikan voucher promo pada periode tertentu. Misal voucher yang berlaku hanya pada saat hari Nasional / hari Raya tertentu.
  3. Memberikan voucher gratis/diskon ongkos kirim.
  4. Berinteraksi dengan Users seperti memberi ucapan selamat hari Raya ataupun yang lainnya.
  5. Memberi Notification berisi pembelian terakhir yang dilakukan sehingga mengingatkan kembali tentang customer journey di Olist Ecommerce.

- Cluster Big Spender <br>
  1. Memperbanyak opsi cicilan kartu kredit dengan berbagai macam bank
  2. Memperbanyak opsi jumlah cicilan
  3. Memberikan promo cicilan tanpa bunga
  4. Memberi promo/voucher khusus yang bersifat personal kepada segmen ini.
  5. Menawarkan program loyalitas kepada konsumen

- Cluster Medium Spender <br>
  1. Memperbanyak opsi cicilan kartu kredit dengan berbagai macam bank
  2. Membuat Notification untuk menarik dan mengingatkan kembali customer terhadap Olist Ecommerce.
  3. Menawarkan program loyalitas kepada konsumen
  4. Berinteraksi dengan Users seperti memberi ucapan selamat hari Raya ataupun yang lainnya.
