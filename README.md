# Analisis Pengalaman Pengguna Layanan Chatbot Perbankan (Excel + Power Query + Power BI)

Analisis terhadap 446 responden untuk memahami pengalaman dan tingkat kepuasan pengguna layanan chatbot perbankan. Proyek ini menggunakan Excel dan Power Query untuk proses data preparation, kemudian Power BI untuk data modeling, analisis deskriptif, dan visualisasi dashboard.

![Dashboard](images/dashboard.png)

## 1. Masalah Bisnis

**Stakeholder:** Tim Pengelola Layanan Digital Perbankan.

Layanan chatbot menjadi salah satu kanal layanan digital yang digunakan untuk membantu nasabah memperoleh informasi dan menyelesaikan kebutuhan perbankan. Namun, diperlukan evaluasi untuk memahami bagaimana pengalaman pengguna dan tingkat kepuasan mereka terhadap layanan tersebut.

Pertanyaan yang dijawab:

- Bagaimana karakteristik responden yang berpartisipasi dalam survei?
- Berapa banyak responden yang pernah menggunakan layanan chatbot?
- Bagaimana pengalaman pengguna terhadap berbagai aspek layanan chatbot?
- Aspek pengalaman apa yang memperoleh penilaian tertinggi dan terendah?
- Bagaimana tingkat kepuasan pengguna terhadap layanan chatbot?
- Bagaimana penilaian pengalaman dan kepuasan berdasarkan gender, kelompok umur, dan frekuensi penggunaan?

## 2. Data

- **Sumber:** Patricia, C., Mastoyo, H., & Widyakusumastuti, M. (2025). *The Impact of E-Chatbot Service Quality on Customer Satisfaction in Indonesian Banking* (Versi 2). Zenodo. [https://doi.org/10.5281/zenodo.17001970](https://doi.org/10.5281/zenodo.17001970)
- **Berkas yang dipakai:** sheet `RAW DATA` dari *OPEN DATA Customers' Satisfaction Survey with Chatbots (Survey data in Bahasa Indonesia).xlsx*.
- **Lisensi:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- **Tabel utama:** `survey_responden`.
- **Tabel analisis:** `distribusi_pengalaman` dan `distribusi_kepuasan`.
- **Tabel referensi:** `informasi_questions` (kamus pertanyaan Q01–Q38).
- **Jumlah responden:** 446 responden.
- **Responden yang pernah menggunakan chatbot:** 432 responden.
- **Responden yang belum pernah menggunakan chatbot:** 14 responden.
- **Periode survei:** 10–29 April 2025.
- **Indikator pengalaman:** 32 pertanyaan (Q01–Q32).
- **Indikator kepuasan:** 6 pertanyaan (Q33–Q38).
- **Skala penilaian:** Likert 1–5.

Data survei digunakan untuk analisis deskriptif. Data mentah tersedia di folder [`data/raw/`](data/raw/) dan dapat diunduh dari halaman Zenodo di atas. Data tidak memuat nama atau kontak responden.

## 3. Proses Data Preparation

Proses persiapan data dilakukan menggunakan Excel dan Power Query.

Tahapan yang dilakukan:

1. Memeriksa struktur data dan tipe data setiap kolom.
2. Mengidentifikasi nilai kosong, error, dan data yang tidak sesuai.
3. Menangani nilai error pada kolom umur.
4. Memeriksa data duplikat dan konsistensi data responden.
5. Menyesuaikan tipe data kolom sesuai kebutuhan analisis.
6. Membersihkan dan menyeragamkan kolom pertanyaan survei.
7. Membuat kolom ID responden sebagai identitas unik untuk kebutuhan data modeling.
8. Membuat kolom kelompok umur untuk kebutuhan analisis per segmen.
9. Membentuk tabel distribusi pengalaman dan kepuasan agar data dapat divisualisasikan berdasarkan pertanyaan dan jawaban.
10. Menyusun tabel `informasi_questions` yang memetakan kode pertanyaan ke pernyataan, indikator, dan kelompoknya.

Catatan hasil pemeriksaan data:

- 14 responden yang belum pernah menggunakan chatbot tidak memiliki jawaban pada kolom gender, umur, frekuensi penggunaan, dan seluruh pertanyaan Q01–Q38 (588 sel kosong). Mereka tetap dihitung sebagai responden, tetapi tidak ikut dalam perhitungan skor pengalaman dan kepuasan.
- Seluruh skor Q01–Q38 berada pada rentang 1–5.
- Sheet `INPUT PLS` pada berkas sumber merupakan data input analisis penulis asli dan tidak digunakan.

Hasil akhir data preparation:

| Tabel | Fungsi |
|---|---|
| `survey_responden` | Menyimpan informasi responden dan jawaban survei dalam bentuk tabel utama |
| `distribusi_pengalaman` | Menyimpan data jawaban untuk indikator pengalaman Q01–Q32 |
| `distribusi_kepuasan` | Menyimpan data jawaban untuk indikator kepuasan Q33–Q38 |
| `informasi_questions` | Kamus pertanyaan Q01–Q38 (tabel referensi, tanpa relasi) |

## 4. Metode dan Tools

**Tools yang digunakan:**

- **Microsoft Excel:** Pemeriksaan awal dan pengelolaan data.
- **Power Query:** Data cleaning, transformation, dan pembentukan tabel analisis.
- **Power BI:** Data modeling, visualisasi, analisis deskriptif, dan pembuatan dashboard (kanvas 1920 × 1080).
- **DAX:** Pembuatan measure untuk menghitung KPI dan indikator analisis.

**Data Modeling:**

Model data menggunakan pendekatan relasi one-to-many:

| Tabel utama | Tabel terkait | Relasi |
|---|---|---|
| `survey_responden` | `distribusi_pengalaman` | One-to-Many |
| `survey_responden` | `distribusi_kepuasan` | One-to-Many |

Relasi menggunakan kolom `ID_Responden` dengan arah filter single direction. Tabel `informasi_questions` berdiri sendiri dan tidak memiliki relasi.

**Definisi KPI:**

| KPI | Definisi |
|---|---|
| Pengguna chatbot | Responden yang pernah menggunakan chatbot |
| Rata-rata kepuasan | Rata-rata skor Q33–Q38 pada pengguna chatbot |
| Pengguna puas | Pengguna chatbot dengan rata-rata skor Q33–Q38 ≥ 4 |
| Rata-rata pengalaman | Rata-rata dari skor delapan aspek tematik (bukan rata-rata seluruh 32 pertanyaan) |

**Pengelompokan indikator pengalaman:**

Sebanyak 32 pertanyaan pengalaman dikelompokkan menjadi delapan aspek tematik untuk mempermudah analisis.

| Aspek | Indikator |
|---|---|
| Pengetahuan dan Ketersediaan | Q01–Q02 |
| Personalisasi Layanan | Q03, Q13–Q16 |
| Kualitas Interaksi | Q04–Q08 |
| Informasi dan Inovasi | Q09–Q12 |
| Penyelesaian Masalah | Q17–Q20 |
| Akurasi dan Kelengkapan Informasi | Q21–Q24 |
| Kepercayaan dan Kredibilitas | Q25–Q29 |
| Efektivitas dan Efisiensi | Q30–Q32 |

Pengelompokan tersebut merupakan pendekatan tematik untuk kebutuhan analisis deskriptif, bukan konstruk yang telah divalidasi secara statistik.

**Indikator kepuasan:**

| Kode | Indikator |
|---|---|
| Q33 | Kepuasan terhadap bantuan |
| Q34 | Kepuasan terhadap dukungan |
| Q35 | Efektivitas penyelesaian masalah |
| Q36 | Pemenuhan harapan |
| Q37 | Kesenangan berinteraksi |
| Q38 | Kepuasan keseluruhan |

## 5. Dashboard

Dashboard terdiri dari tiga halaman:

| Halaman | Isi |
|---|---|
| **Ringkasan Kepuasan** | Enam KPI, rata-rata skor indikator kepuasan, gauge pengguna puas, distribusi jawaban Q33–Q38, kepuasan per frekuensi penggunaan, insight dan rekomendasi |
| **Analisis Pengalaman** | Empat KPI, distribusi jawaban Q01–Q32, rata-rata skor delapan aspek, insight dan rekomendasi |
| **Informasi Questions** | Kamus 38 pertanyaan dengan filter kelompok dan indikator |

Interaktivitas: slicer Gender, Kelompok Umur, dan Frekuensi Penggunaan yang tersinkronisasi antara dua halaman analisis, navigator halaman, tombol **Reset filter**, dan tombol **Lihat keterangan Q** yang membuka halaman Informasi Questions.

## 6. Temuan Utama

![Dashboard Ringkasan Kepuasan](images/dashboard_kepuasan.png)

**Ringkasan KPI:**

| KPI | Nilai |
|---|---:|
| Total responden | 446 |
| Pengguna chatbot | 432 |
| Persentase pengguna chatbot | 96,86% |
| Rata-rata kepuasan pengguna | 3,96 / 5 |
| Pengguna puas | 315 (72,9% dari pengguna chatbot) |
| Rata-rata pengalaman (8 aspek) | 3,70 / 5 |

**Insight utama:**

1. **Mayoritas responden pernah menggunakan chatbot.** Sebanyak 432 dari 446 responden atau 96,86% menyatakan pernah menggunakan layanan chatbot perbankan. Dashboard menampilkannya sebagai 96,9%.

2. **Tingkat kepuasan pengguna tergolong positif.** Rata-rata kepuasan mencapai 3,96 dari skala 5, yang menunjukkan kecenderungan penilaian positif terhadap layanan chatbot. Sebanyak 315 pengguna (72,9%) masuk kategori puas.

3. **Dukungan memperoleh penilaian tertinggi.** Indikator kepuasan terhadap dukungan (Q34) memperoleh nilai rata-rata 3,99.

4. **Efektivitas penyelesaian masalah memiliki nilai terendah.** Indikator Q35 memperoleh rata-rata 3,91. Meskipun nilainya masih relatif positif, aspek ini dapat menjadi salah satu peluang peningkatan layanan.

5. **Perbedaan penilaian antarindikator kepuasan relatif kecil.** Selisih antara indikator dengan nilai tertinggi dan terendah hanya sebesar 0,08 poin.

![Dashboard Analisis Pengalaman](images/dashboard_pengalaman.png)

**Analisis pengalaman pengguna:**

Halaman kedua dashboard menyajikan distribusi jawaban terhadap 32 indikator pengalaman serta perbandingan rata-rata penilaian pada delapan aspek tematik.

6. **Pengalaman dinilai lebih rendah daripada kepuasan.** Rata-rata pengalaman pada delapan aspek sebesar 3,70, lebih rendah dibandingkan rata-rata kepuasan sebesar 3,96.

7. **Akurasi dan kelengkapan informasi memperoleh penilaian tertinggi** (3,95), disusul kepercayaan dan kredibilitas (3,88).

8. **Kualitas interaksi dan personalisasi layanan memperoleh penilaian terendah** (3,29 dan 3,41). Selisih aspek tertinggi dan terendah mencapai 0,66 poin, lebih besar dibandingkan selisih antarindikator kepuasan.

Dashboard juga menyediakan analisis berdasarkan gender, kelompok umur, dan frekuensi penggunaan untuk membantu melihat variasi penilaian antarsegmen responden.

![Dashboard Informasi Questions](images/dashboard_informasi_questions.png)

Halaman ketiga berisi kamus pertanyaan survei sehingga pembaca dapat mengetahui arti kode Q yang tampil pada grafik.

## 7. Rekomendasi

Berdasarkan hasil analisis, beberapa rekomendasi yang dapat dipertimbangkan oleh pengelola layanan chatbot perbankan adalah:

1. **Meningkatkan kemampuan penyelesaian masalah.** Pengelola dapat mengevaluasi kemampuan chatbot dalam memahami variasi pertanyaan dan konteks permasalahan pengguna, terutama pada kebutuhan perbankan yang lebih kompleks.

2. **Memperbaiki kualitas interaksi dan personalisasi.** Kedua aspek ini memperoleh penilaian terendah pada analisis pengalaman, sehingga gaya komunikasi chatbot dan relevansi rekomendasi produk dapat menjadi prioritas evaluasi.

3. **Mengoptimalkan akurasi dan relevansi informasi.** Knowledge base chatbot perlu diperbarui secara berkala agar informasi yang diberikan tetap relevan, konsisten, dan sesuai dengan kebutuhan pengguna.

4. **Mempertahankan kualitas dukungan layanan.** Indikator dukungan memperoleh penilaian tertinggi, sehingga kualitas layanan yang sudah dinilai positif perlu dipertahankan dan dievaluasi secara berkala.

5. **Menyediakan eskalasi ke petugas manusia.** Pengguna perlu memperoleh alternatif bantuan melalui customer service ketika chatbot tidak mampu menyelesaikan permasalahan secara memadai.

6. **Melakukan evaluasi layanan berdasarkan segmen pengguna.** Perbedaan karakteristik dan frekuensi penggunaan dapat menjadi bahan pertimbangan dalam merancang pengalaman layanan yang lebih sesuai dengan kebutuhan pengguna.

Rekomendasi tersebut merupakan implikasi manajerial berdasarkan hasil analisis deskriptif, bukan bukti hubungan sebab-akibat.

## 8. Keterbatasan

- Analisis didasarkan pada data survei dengan 446 responden dan belum tentu mewakili seluruh pengguna layanan chatbot perbankan.
- Data menggambarkan persepsi responden pada periode survei 10–29 April 2025.
- Analisis bersifat deskriptif dan tidak digunakan untuk membuktikan hubungan sebab-akibat.
- Pengelompokan 32 indikator pengalaman menjadi delapan aspek tematik dilakukan untuk mempermudah visualisasi dan interpretasi, bukan berdasarkan pengujian validitas konstruk.
- Penilaian kepuasan dan pengalaman berasal dari jawaban subjektif responden.
- Hasil analisis berdasarkan gender, kelompok umur, dan frekuensi penggunaan perlu diinterpretasikan dengan mempertimbangkan jumlah responden pada setiap kelompok.
- Teks insight pada dashboard bersifat statis dan berlaku untuk seluruh responden tanpa filter.

## 9. Cara Mereproduksi

1. Unduh dataset dari Zenodo (tautan pada bagian 2) atau gunakan berkas pada folder `data/raw/`.
2. Buka file Excel dan lakukan pemeriksaan awal terhadap struktur serta tipe data.
3. Impor data ke Power BI Desktop melalui fitur **Get Data**.
4. Lakukan proses cleaning dan transformation menggunakan Power Query sesuai tahapan data preparation.
5. Bentuk tabel `survey_responden`, `distribusi_pengalaman`, dan `distribusi_kepuasan`, lalu impor `data/informasi_questions.csv` sebagai tabel referensi.
6. Terapkan relasi antar tabel menggunakan `ID_Responden` melalui fitur **Model View**.
7. Buat measure DAX untuk menghitung KPI dan indikator analisis.
8. Bangun visualisasi dan dashboard pada tiga halaman yang telah disediakan.
9. Gunakan slicer untuk mengeksplorasi hasil analisis berdasarkan gender, kelompok umur, dan frekuensi penggunaan.

Alternatif: buka langsung `dashboard_chatbot_perbankan.pbix` dengan Power BI Desktop (gratis). Jika sumber data tidak terbaca, arahkan ke lokasi file data melalui **Home → Transform data → Data source settings**. Tombol dapat diuji pada mode edit dengan **Ctrl + klik**. Tema warna tersedia di `theme/tema_banking.json`.

## 10. Atribusi

Data berasal dari dataset terbuka yang dipublikasikan di Zenodo oleh Patricia, Mastoyo, dan Widyakusumastuti (Binus University) dengan lisensi CC BY 4.0. Hak atas dataset tetap dimiliki penulis aslinya.

Proyek ini adalah analisis deskriptif independen dan tidak berafiliasi dengan penulis dataset. Publikasi asli memakai metode yang berbeda (Structural Equation Modeling dengan PLS), sedangkan proyek ini hanya menyajikan ringkasan deskriptif dan visualisasi.

## Struktur Repository

```text
├── data/
│   ├── raw/
│   │   └── survey_chatbot_zenodo.xlsx
│   └── informasi_questions.csv
├── images/
│   ├── dashboard.png
│   ├── dashboard_kepuasan.png
│   ├── dashboard_pengalaman.png
│   └── dashboard_informasi_questions.png
├── theme/
│   └── tema_banking.json
├── dashboard_chatbot_perbankan.pbix
└── README.md
```

## Penulis

Wahyudi Noviansyah · [LinkedIn](https://linkedin.com/in/wahyudi-noviansyah-63a850374)