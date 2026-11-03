# Tugas Intermediate Data Analytics (Part 1) : Eksplorasi Perilaku Konsumen SuryaMart Menggunakan SQL

![Google BigQuery](https://img.shields.io/badge/Database-Google_BigQuery-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white)
![SQL](https://img.shields.io/badge/Language-Standard_SQL-CC2927?style=for-the-badge&logo=postgresql&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Reporting-Google_Sheets_%7C_Excel-34A853?style=for-the-badge&logo=google-sheets&logoColor=white)
![Bootcamp](https://img.shields.io/badge/Program-Harisenin_Data_Analytics_Bootcamp-blue?style=for-the-badge)

> **Bootcamp Project Assignment (Intermediate 1 — Crack the Code of Meaningful Insights)** — Mengekstrak, memfilter, dan mengagregasi 500 data transaksi pelanggan menggunakan **SQL pada Google BigQuery** untuk mengevaluasi efektivitas program loyalitas, pola kanal belanja, serta strategi pemasaran retail **SuryaMart**.

---

## Latar Belakang & Konteks Bisnis
Proyek ini merupakan tugas studi kasus *Intermediate Part 1* pada **Harisenin Data Analytics Bootcamp**. Dalam skenario bisnis ini, **SuryaMart** merupakan perusahaan retail yang sedang berkembang pesat dan memiliki ratusan data transaksi dari berbagai kategori produk seperti makanan, perlengkapan rumah tangga, elektronik, hingga barang mewah.

Untuk mendukung strategi bisnis, tim *marketing* SuryaMart ingin memanfaatkan data pelanggan guna memahami pola perilaku konsumen, tren pembelian, serta mengevaluasi efektivitas kampanye digital dan program loyalitas pelanggan. Sebagai *Junior Data Analyst*, saya ditugaskan untuk mengekstrak informasi penting dari database perusahaan menggunakan **SQL**, menyaring data, dan merumuskan rekomendasi strategi bisnis berbasis data.

---

## 🛠️ Tech Stack & Tools yang Digunakan
1. **Google Cloud BigQuery**: Digunakan sebagai *cloud data warehouse* untuk menyimpan dataset CSV (`suryamart_d.suryahub`), mendeteksi skema tipe data secara otomatis, dan mengeksekusi *query* SQL.
2. **Standard SQL**: Digunakan untuk melakukan eksplorasi data menggunakan klausa `SELECT`, `WHERE`, `GROUP BY`, `ORDER BY`, `LIMIT`, serta fungsi agregat `COUNT()`, `SUM()`, `AVG()`, dan `ROUND()`.
3. **Google Sheets / Microsoft Excel**: Digunakan untuk mendokumentasikan hasil keluaran *query* SQL dan menyusun matriks *Insight Summary & Strategic Recommendations*.

---

## Alur Kerja Proyek (Project Workflow)
1. **Database & Schema Setup**: Mengunggah dataset mentah (`Data SuryaHub.csv` — 500 baris transaksi, 15 atribut kolom) ke dalam *dataset* `suryamart_d` di Google BigQuery.
2. **Data Exploration & SQL Querying**: Menulis dan menjalankan 7 *query* SQL spesifik untuk menjawab pertanyaan bisnis utama manajemen SuryaMart.
3. **Insight Extraction & Business Strategy**: Menerjemahkan angka hasil agregasi SQL menjadi *key takeaways*, *business insights*, dan rekomendasi strategi yang dapat dieksekusi (*actionable recommendations*).

### 📸 Dokumentasi Setup Tabel & Skema di Google BigQuery
> *Tampilan struktur skema tabel `suryamart_db.suryahub` pada Google BigQuery:*


<img width="1741" height="612" alt="image" src="https://github.com/user-attachments/assets/41866827-2f7d-4510-9376-c1263ca3fe7c" />


---

## Eksekusi Query SQL & Hasil Eksplorasi Data

Berikut adalah dokumentasi pengerjaan 7 pertanyaan bisnis menggunakan SQL beserta cuplikan tabel hasil eksekusinya di Google BigQuery:

### 1. Total Transaksi Pelanggan Perempuan
* **Pertanyaan Bisnis**: *Berapa jumlah total transaksi (bukan nominal uang) yang dilakukan pembelian produk berjenis kelamin perempuan?*
* **Query SQL**:
  ```sql
  SELECT 
      COUNT(Customer_ID) AS Total_Transaksi_Perempuan
  FROM suryamart_db.suryahub
  WHERE Gender = 'Female';
  ```
* **Hasil Eksekusi Query**:

<img width="233" height="70" alt="Screenshot 2026-10-29 102921" src="https://github.com/user-attachments/assets/0d25de05-f57f-44d9-b821-c83fcf863095" />

* **Temuan**: Terdapat **265 transaksi** (53% dari total 500 transaksi) yang dilakukan oleh pelanggan perempuan.

---

### 2. Top 5 Kota dengan Jumlah Pembelian Terbanyak
* **Pertanyaan Bisnis**: *Apa 5 kota dengan jumlah pembelian produk terbanyak?*
* **Query SQL**:
  ```sql
  SELECT 
      Location, 
      COUNT(Customer_ID) AS Jumlah_Transaksi,
      SUM(Frequency_of_Purchase) AS Total_Frekuensi_Pembelian
  FROM suryamart_db.suryahub
  GROUP BY Location
  ORDER BY Jumlah_Transaksi DESC
  LIMIT 5;
  ```
* **Hasil Eksekusi Query**:

<img width="630" height="198" alt="Screenshot 2026-10-29 102931" src="https://github.com/user-attachments/assets/6e8e51ff-d423-4d84-9645-2686a3e922dd" />

* **Temuan**: **Makassar** memimpin dengan **95 transaksi** (total akumulasi frekuensi 658 kali), disusul oleh **Jakarta** (90 transaksi), **Bandung** (82 transaksi), **Surabaya** (78 transaksi), dan **Medan** (78 transaksi).

---

### 3. Analisis Loyalitas Merek (`Brand_Loyalty = 5`) & Penggunaan Diskon
* **Pertanyaan Bisnis**: *Berapa banyak pelanggan yang memiliki Brand_Loyalty = 5? Apakah mereka cenderung menggunakan diskon True atau False?*
* **Query SQL**:
  ```sql
  SELECT 
      Discount_Used, 
      COUNT(Customer_ID) AS Jumlah_Pelanggan
  FROM suryamart_db.suryahub
  WHERE Brand_Loyalty = 5
  GROUP BY Discount_Used;
  ```
* **Hasil Eksekusi Query**:

<img width="635" height="203" alt="Screenshot 2026-11-03 075149" src="https://github.com/user-attachments/assets/ae6338a8-0669-4d92-b281-6ac4ff0e4341" />

* **Temuan**: Dari **106 pelanggan** yang memiliki `Brand_Loyalty = 5`, mayoritas cenderung **tidak menggunakan diskon (`FALSE`)** sebanyak **60 pelanggan (56,6%)**, sedangkan yang menggunakan diskon (`TRUE`) sebanyak **46 pelanggan (43,4%)**.

---

### 4. Segmentasi Berdasarkan Intensi Pembelian (`Purchase_Intent`)
* **Pertanyaan Bisnis**: *Kelompokkan pelanggan berdasarkan Purchase_Intent. Di antara kategori yang ada (Impulsive, Need-based, Planned, Wants-based), siapa yang cenderung belanja lebih mahal dan lebih sering? Jelaskan temuanmu.*
* **Query SQL**:
  ```sql
  SELECT Purchase_Intent,
         AVG(Purchase_Amount_Rp) AS avg_amount,
         AVG(Frequency_of_Purchase) AS avg_frequency
  FROM suryamart_db.suryahub
  GROUP BY Purchase_Intent
  ORDER BY avg_frequency DESC;
  ```
* **Hasil Eksekusi Query**:
  
<img width="640" height="167" alt="Screenshot 2026-11-03 075813" src="https://github.com/user-attachments/assets/a80dd99a-2e48-4171-93bd-219654acf37a" />

* **Temuan**: Pelanggan dengan kategori **Planned** cenderung belanja **paling mahal** (rata-rata **Rp 4.338.039**) dan **paling sering** (rata-rata **6,94 kali**). Sementara itu, kategori **Need-based** menghasilkan total pendapatan tertinggi (Rp 558,82 Juta) karena memiliki jumlah pelanggan terbanyak (130 orang).

---

### 5. Perbandingan Pola Pembelian Antar Kanal (`Purchase_Channel`)
* **Pertanyaan Bisnis**: *Apakah ada perbedaan pola pembelian antara channel pembelian (Online, Mixed, In-Store)? Bandingkan berdasarkan Purchase_Amount_Rp dan Frequency_of_Purchase.*
* **Query SQL**:
  ```sql
  SELECT Purchase_Channel,
  AVG(Purchase_Amount_Rp) AS avg_amount,
  AVG(Frequency_of_Purchase) AS avg_frequency
  FROM suryamart_db.suryahub
  GROUP BY Purchase_Channel;
  ```
* **Hasil Eksekusi Query**:

<img width="637" height="132" alt="Screenshot 2026-11-03 082147" src="https://github.com/user-attachments/assets/4997749f-6bcb-477d-b533-0149abac0487" />

* **Temuan**: Kanal **Mixed** (*Omnichannel*) mencatatkan rata-rata nilai transaksi tertinggi (**Rp 4.459.349**), sedangkan kanal **In-Store** (toko fisik) memiliki rata-rata frekuensi belanja tertinggi (**7,14 kali**).

---

### 6. Evaluasi Efektivitas Program Loyalitas (`Customer_Loyalty_Program_Member`)
* **Pertanyaan Bisnis**: *Apakah anggota program loyalitas (Customer_Loyalty_Program_Member = TRUE) memiliki rata-rata pembelian produk yang lebih tinggi atau lebih sering dibanding non-anggota?*
* **Query SQL**:
  ```sql
  SELECT Customer_Loyalty_Program_Member,
     AVG(Purchase_Amount_Rp) AS avg_amount,
     AVG(Frequency_of_Purchase) AS avg_frequency
  FROM suryamart_db.suryahub
  GROUP BY Customer_Loyalty_Program_Member
  ORDER BY Customer_Loyalty_Program_Member DESC;
  ```
* **Hasil Eksekusi Query**:

<img width="508" height="92" alt="Screenshot 2026-11-03 082527" src="https://github.com/user-attachments/assets/515a5a01-767b-4a87-9edb-9df0e1c12946" />

* **Temuan**: **Tidak.** Anggota program loyalitas (`TRUE`) justru memiliki rata-rata belanja (**Rp 4.104.058**) dan frekuensi (**6,68 kali**) yang lebih rendah dibandingkan non-anggota (`FALSE`) yang mencapai rata-rata **Rp 4.375.335** dan frekuensi **6,99 kali**.

---

### 7. Profil Top 10 Pelanggan dengan Nilai Pembelian Tertinggi
* **Pertanyaan Bisnis**: *Siapa 10 pelanggan dengan total pembelian (Purchase_Amount_Rp) tertinggi? Apa ciri-ciri mereka (lokasi, channel, brand loyalty)?*
* **Query SQL**:
  ```sql
   SELECT Customer_ID,
     SUM(Purchase_Amount_Rp) AS Total_Spent,
     MAX(Location) AS Location,
     MAX(Purchase_Channel) AS Channel,
     MAX(Brand_Loyalty) AS Brand_Loyalty
  FROM suryamart_db.suryahub
  GROUP BY Customer_ID
  ORDER BY Customer_ID DESC
  LIMIT 10
  ```
* **Hasil Eksekusi Query**:

<img width="1160" height="370" alt="Screenshot 2026-11-03 082818" src="https://github.com/user-attachments/assets/41bf89ef-14b5-4400-a522-b23b5283f645" />

* **Temuan**: Sepuluh pelanggan dengan transaksi tertinggi (berkisar antara **Rp 7,60 Juta – Rp 7,72 Juta**) didominasi oleh pelanggan dari **Surabaya** (3 orang) dan **Makassar** (3 orang), mayoritas berbelanja melalui kanal **In-Store** (5 orang) dan **Mixed** (3 orang), serta **40% di antaranya memiliki tingkat loyalitas merek yang rendah (`Brand_Loyalty = 1`)**.

---

## Key Takeaways Utama
1. **Pola Belanja & Penggerak Frekuensi Ritel (*Basket Size & Purchase Frequency Drivers*)**: Pelanggan bertipe *Planned* dan pengguna kanal *Mixed (Omnichannel)* menghasilkan nilai keranjang belanja (*basket size*) tertinggi, sementara toko fisik (*In-Store*) mendorong frekuensi kunjungan tertinggi.
2. **Potensi Pasar Wilayah Luar Ibu Kota (*Regional Market Potential*)**: Makassar menempati peringkat pertama dalam jumlah transaksi (95 transaksi) dan akumulasi frekuensi pembelian (658 kali), membuktikan tingginya potensi pasar di luar Pulau Jawa.
3. **Paradoks Program Loyalitas & Retensi (*Loyalty Program & Retention Paradox*)**: Anggota *Loyalty Program* mencatatkan rata-rata belanja lebih rendah dibanding non-anggota, dan banyak *high-ticket spenders* memiliki `Brand_Loyalty = 1`. Di sisi lain, pelanggan dengan `Brand_Loyalty = 5` tidak bergantung pada diskon (56,6% *False*).

---

## Matriks Rangkuman Insight & Rekomendasi Strategi

| No | Business Question | Temuan / Jawaban | Insight Bisnis | Rekomendasi Strategi |
| :-: | :--- | :--- | :--- | :--- |
| **1** | **Berapa jumlah total transaksi (bukan nominal uang) yang dilakukan pembelian produk berjenis kelamin perempuan?** | **265 dari 500 transaksi (53%)** dilakukan oleh pelanggan perempuan. | Pelanggan perempuan merupakan segmen mayoritas penggerak volume transaksi SuryaMart. | Fokuskan kurasi produk dan kampanye tematik berkala yang relevan dengan kebutuhan perempuan serta optimalkan iklan di media sosial. |
| **2** | **Apa 5 kota dengan jumlah pembelian produk terbanyak?** | **Makassar (95)**, Jakarta (90), Bandung (82), Surabaya (78), dan Medan (78). | Makassar dan kota-kota besar di luar Jakarta memiliki daya beli dan keaktifan belanja yang sangat tinggi. | Alokasikan *geo-targeted ads* dan bangun kemitraan logistik lokal di Makassar, Jakarta, dan Bandung untuk efisiensi ongkir dan pengiriman. |
| **3** | **Berapa banyak pelanggan yang memiliki `Brand_Loyalty = 5`? Apakah mereka cenderung menggunakan diskon True atau False?** | Dari 106 pelanggan sangat loyal, **60 orang (56,6%) tidak memakai diskon (`FALSE`)** dan 46 orang (43,4%) memakai diskon (`TRUE`). | Pelanggan yang sudah loyal terhadap merek tidak sensitif terhadap harga dan tetap berbelanja tanpa insentif diskon. | Kurangi promosi potongan harga langsung pada segmen ini; alihkan ke bentuk apresiasi eksklusif seperti *early access* produk baru atau layanan VIP. |
| **4** | **Kelompokkan pelanggan berdasarkan `Purchase_Intent`. Siapa yang cenderung belanja lebih mahal dan lebih sering?** | Kategori **Planned** memiliki rata-rata belanja tertinggi (**Rp 4,33 Juta**) dan frekuensi tertinggi (**6,94x**). **Need-based** menyumbang total pendapatan terbesar (Rp 558,82 Juta). | Pembeli terencana (*Planned*) menyiapkan anggaran lebih besar per transaksi, sedangkan pembeli *Need-based* mendominasi populasi pelanggan. | Buat fitur *wishlist reminder* dan paket *bundling* bulanan untuk segmen *Planned & Need-based*, serta terapkan *flash sale* di halaman *checkout* untuk memicu segmen *Impulsive*. |
| **5** | **Apakah ada perbedaan pola pembelian antara channel pembelian (Online, Mixed, In-Store)?** | **Mixed** unggul di rata-rata belanja (**Rp 4,45 Juta**), sedangkan **In-Store** unggul di frekuensi belanja (**7,14x**). | Pengalaman belanja lintas kanal (*Omnichannel*) mendorong pembelian bernilai besar, sedangkan toko fisik menjaga rutinitas belanja. | Integrasikan strategi *Omnichannel* seperti fitur *Click & Collect* (beli online, ambil di toko) dan berikan voucher toko fisik setiap selesai bertransaksi di aplikasi. |
| **6** | **Apakah anggota program loyalitas (`Customer_Loyalty_Program_Member = TRUE`) memiliki rata-rata pembelian lebih tinggi atau lebih sering?** | Anggota (`TRUE`) rata-rata belanja **Rp 4,10 Juta (6,68x)**, lebih rendah dari non-anggota (`FALSE`) sebesar **Rp 4,37 Juta (6,99x)**. | Skema program loyalitas saat ini belum efektif mendorong peningkatan nilai keranjang belanja (*upselling*) maupun frekuensi retensi. | Rombak benefit *Loyalty Program* dengan menerapkan *tiered membership* (level keanggotaan berjenjang) dan *cashback* poin yang mensyaratkan minimum pembelanjaan tertentu. |
| **7** | **Siapa 10 pelanggan dengan total pembelian tertinggi? Apa ciri-ciri mereka (lokasi, channel, brand loyalty)?** | Transaksi berkisar **Rp 7,60 Juta – Rp 7,72 Juta**, didominasi kota **Surabaya & Makassar**, kanal **In-Store & Mixed**, namun **40% memiliki `Brand_Loyalty = 1`**. | Pembeli barang bernilai tinggi (*Jewelry, Home Appliances, Furniture*) lebih percaya bertransaksi di toko fisik, namun berisiko tinggi pindah ke kompetitor (*churn*). | Berikan layanan purnajual (*after-sales service*) personal dan daftarkan pelanggan *high-ticket* secara otomatis ke keanggotaan VIP untuk mengunci retensi mereka. |

---

## 👤 Penulis
**Ananda Widi Alrafi — 1306011**  
*Mahasiswa Teknologi Informasi — Institut Teknologi Sepuluh Nopember (ITS)*  
*Participant of Harisenin Data Analytics Bootcamp Batch #6*
