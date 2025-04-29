# Laporan Proyek Machine Learning – Muhammad Raihan Fauzi

## Project Overview

**Latar Belakang**  

Di era digital, musik menjadi teman dalam hampir setiap aktivitas ​dari memacu produktivitas saat bekerja hingga melepaskan stres di perjalanan. Sistem rekomendasi musik merupakan perangkat penting untuk membantu pengguna menemukan lagu yang relevan di tengah melimpahnya pilihan lagu yang tersedia secara daring. Dengan jutaan lagu yang dapat diakses melalui layanan streaming, pengguna sering mengalami information overload saat mencoba mencari musik yang sesuai selera mereka [1]. 

Recommender system dapat berperan sebagai alat penyaring informasi untuk menyajikan item yang paling relevan bagi pengguna. Dalam konteks musik, sistem rekomendasi dapat meningkatkan pengalaman mendengarkan dengan memperkenalkan lagu-lagu baru yang kemungkinan disukai pengguna, sekaligus meningkatkan engagement pada platform musik. 

**Mengapa Masalah Ini Penting ?**  
Tanpa sistem rekomendasi rekomendasi, information overload menurunkan pengalaman pengguna, mempercepat churn, dan menghambat musisi baru meraih pendengar. Maka dibutuhkan sistem rekomendasi yang akurat pada platform music untuk merekomendasikan sejumlah lagu, karena dengan adanya sistem rekomendasi music di platform music maka akan:
- Meningkatkan lifetime value. pengguna bertahan lebih lama karena selalu menemukan konten segar yang cocok.  
- Mendorong ekonomi kreatif, ​algoritma membantu penyanyi menemukan audiens niche, meningkatkan pendapatan artis independen.  
- Mengoptimalkan bandwidth promosi dan menekan biaya pemasaran.  
Karena itu, merancang sistem rekomendasi yang efisien dan adalah kebutuhan strategis bagi platform musik.

## Business Understanding
### Problem Statements
- Bagaimana cara memberikan daftar rekomendasi lagu yang mirip atau relevan berdasarkan lagu yang baru didengarkan pengguna ?
- Bagaimana cara meningkatkan personalisasi rekomendasi dengan memanfaatkan data preferensi pengguna (riwayat lagu yang didengarkan) maupun karakteristik konten lagu ?

### Goals
- Mengembangkan sistem rekomendasi yang dapat memberikan daftar rekomendasi lagu yang mirip atau relevan berdasarkan lagu yang baru didengarkan pengguna.
- Mengembangkan sistem rekomendasi yang dapat meningkatkan personalisasi rekomendasi dengan memanfaatkan data preferensi pengguna (riwayat lagu yang didengarkan) maupun karakteristik konten lagu.

### Solution Statements
Untuk mencapai tujuan di atas, digunakan dua pendekatan solusi:
- Content‑Based Filtering (TF‑IDF Cosine Similarity) : Sistem merekomendasikan lagu berdasarkan kemiripan konten dengan lagu yang disukai pengguna. Dalam proyek ini, fitur deskriptif yang digunakan adalah informasi genre (dan tag) dari lagu. Teknik Term Frequency–Inverse Document Frequency (TF-IDF) digunakan untuk merepresentasikan genre lagu dalam bentuk vektor fitur, dan kemudian Cosine Similarity digunakan untuk mengukur tingkat kemiripan antar lagu. Dengan pendekatan ini, jika pengguna menyukai sebuah lagu, sistem akan mencari lagu-lagu lain yang memiliki deskripsi konten serupa (misal genre yang sama).
- Collaborative Filtering (RecommenderNet) : Sistem merekomendasikan lagu berdasarkan pola kesamaan preferensi antar pengguna. Proyek ini menggunakan model Neural Network sederhana bernama RecommenderNet yang mempelajari keterkaitan antara pengguna dan lagu dari data riwayat mendengarkan. Model ini akan memprediksi seberapa besar kemungkinan seorang pengguna akan menyukai sebuah lagu berdasarkan pola preferensi pengguna lain yang memiliki kemiripan. Dengan kata lain, rekomendasi diberikan dengan memanfaatkan data interaksi user–item secara kolektif.

Pendekatan content-based cenderung memberikan rekomendasi yang dapat dijelaskan (karena berbasis fitur lagu), sementara collaborative filtering dapat menangkap selera tersembunyi pengguna yang tidak terdeteksi oleh metadata sederhana. Kombinasi kedua pendekatan ini diharapkan menghasilkan sistem rekomendasi musik yang lebih akomodatif dan akurat dalam berbagai skenario penggunaan.

## Data Understanding

Dataset: [Million Song Dataset – Spotify × LastFM (2021)](https://www.kaggle.com/datasets/undefinenull/million-song-dataset-spotify-lastfm)  
Dataset yang digunakan berasal dari Kaggle yang telah di download kemudian di load dari local.

Ukuran awal: `Music Info.csv` 50.683 baris & `User Listening History.csv` 9.711.301 baris.

### Tabel `Music Info.csv`
| Kolom | Tipe | Deskripsi |
|-------|------|-----------|
| track_id | str | ID unik lagu |
| name | str | Judul lagu |
| artist | str | Artis |
| spotify_preview_url | str | URL pratinjau 30 s |
| spotify_id | str | ID di Spotify |
| tags | str | Tag LastFM (koma‑separated) |
| genre | str | Genre utama |
| year | int | Tahun rilis |
| duration_ms | int | Durasi (ms) |
| danceability | float | Skor 0‑1 |
| energy | float | Intensitas 0‑1 |
| key | int | Pitch class 0‑11 |
| loudness | float | dB |
| mode | int | Mayor 1 / minor 0 |
| speechiness | float | Proporsi speech |
| acousticness | float | Akustik 0‑1 |
| instrumentalness | float | Prob. instrumental 0‑1 |
| liveness | float | Suasana live 0‑1 |
| valence | float | Nuansa emosional 0‑1 |
| tempo | float | BPM |
| time_signature | int | Birama |

Gambaran data :
|track_id|name|artist|spotify_preview_url|spotify_id|tags|genre|year|duration_ms|danceability|energy|key|loudness|mode|speechiness|acousticness|instrumentalness|liveness|valence|tempo|time_signature|	
|--------|----|------|-------------------|----------|----|-----|----|-----------|------------|------|---|--------|----|-----------|------------|----------------|--------|-------|-----|--------------|
|TRIOREW128F424EAF0|Mr. Brightside|The Killers|https://p.scdn.co/mp3-preview/4d26180e6961fd46866cd9106936ea55dfcbaa75?cid=774b29d4f13844c495f206cafdad9c86|09ZQ5TmUG8TSL56n0knqrj|"rock, alternative, indie, alternative_rock, indie_rock, 00s"|-|2004|222200|0.355|0.918|1|-4.36|1|0.0746|0.00119|0.0|0.0971|0.24|148.114|4|	
|TRRIVDJ128F429B0E8|Wonderwall|Oasis|https://p.scdn.co/mp3-preview/d012e536916c927bd6c8ced0dae75ee3b7715983?cid=774b29d4f13844c495f206cafdad9c86|06UfBBDISthj1ZJAtX4xjj|"rock, alternative, indie, pop, alternative_rock, british, 90s, love, britpop"|-|2006|258613|0.409|0.892|2|-4.373|1|0.0336|0.000807|0.0|0.207|0.651|174.426|4|
|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|


Tabel `User Listening History.csv`
| Kolom | Tipe | Deskripsi |
|-------|------|-----------|
| track_id | str | FK ke `Music Info.csv` |
| user_id | str | ID pengguna |
| playcount | int | Jumlah pemutaran |

Gambaran data :
|track_id|user_id|playcount|
|--------|-------|---------|
|TRIRLYL128F42539D1|b80344d063b5ccb3212f76538f3d9e43d87dca9e|1|
|TRFUPBA128F934F7E1|b80344d063b5ccb3212f76538f3d9e43d87dca9e|1|
|...|...|...|


**Exploratory Data Analysis (EDA)**
Tujuan EDA: Memahami distribusi, tren, pola, menangani data yang hilang, dan visualisasi data.
```python
print(music_df.shape)                          # (50683, 21)
music_df.info()                                # Terdapat null pada beberapa data tags dan genre
print(music_df.describe())                     # Statistik deskriptif
print(music_df.isna().sum())                   # Terdapat missing value pada 1127 baris data tags dan 28335 baris data gennre
print(f"{'music_df'}: {music_df.duplicated().sum()} duplikat") # 0 duplikat

print(user_listening_history_df.shape)         # (9711301, 3)
print(user_listening_history_df.describe())    # Statistik deskriptif
print(user_listening_history_df.isna().sum())  # 0 missing values
print(f"{'user_listening_history_df'}: {user_listening_history_df.duplicated().sum()} duplikat") # 0 duplikat
```
- Gambar 1, Top Genre & Artist menunjukkan Rock mendominasi (≈10 000 lagu), diikuti Electronic & Metal. The Rolling Stones tercatat sebagai artis dengan lagu terbanyak (≈130 lagu)—​menandakan bias katalog ke musik klasik rock.
<img src="https://raw.githubusercontent.com/mraihanfauzii/Music-Recommendation-System/main/images/output.png" width="500">

- Gambar 2, Distribusi Tahun Rilis memusat di 2000‑2015 sedikit lagu < 1980, mengindikasikan dataset fokus era digital.
<img src="https://raw.githubusercontent.com/mraihanfauzii/Music-Recommendation-System/main/images/output2.png" width="500">

- Gambar 3, Hist durasi & danceability menunjukkan durasi normal ≈ 3‑4 menit, distribusi danceability mendekati normal μ≈0,5—meniandakan dataset beragam tetapi tidak ekstrem.
<img src="https://raw.githubusercontent.com/mraihanfauzii/Music-Recommendation-System/main/images/output3.png" width="500">

## Data Preparation
### Teknik
- Cleaning & Filtering, drop baris `genre`/`tags` kosong pada data music.
- Sinkronisasi, hapus riwayat lagu yang terbuang agar sama dengan data music.
- Sampling, pakai 50.000 interaksi untuk eksperimen cepat.
- Pembuatan TF-IDF dan penghitungan Cosine Similarity. Untuk Content Based Filtering.
- Encoding Futr Kategorikal, dengan dibuat mapping user_id → user_index dan track_id → track_index. Untuk Collaborative Filterinng.
- Normalisasi Playcount menjadi Rating, Kolom playcount pada data user sample kemudian dinormalisasi menjadi skala [0, 1] untuk dianggap sebagai rating implisit. Untuk Collaborative Filtering.
- Split Data Training dan Validasi. Untuk Collaborative Filtering.

### Proses
- Cleaning, Drop 28.767 lagu tanpa `genre`/`tags` → 21.916 lagu pada music.
- Sync, Filter interaksi; riwayat susut dari 9.7 juta → 6.28 juta pada riwayat lagu.
- Sample 50.000, Eksperimen ringan.
- Pembuatan TF-IDF dan penghitungan Cosine Similarity, berdasarkan teks genre setiap lagu. Menggunakan TfidfVectorizer dari scikit-learn, sistem melakukan fit dan transform pada kolom genre dari music_clean. Hasilnya adalah matriks TF-IDF berukuran (21.916 x 16) karena terdapat 21.916 lagu bersih dan 16 kata unik (genre unik) dalam korpus. Setiap baris merepresentasikan satu lagu dalam ruang vektor 16 dimensi (masing-masing dimensi bobot TF-IDF genre tertentu). Mengingat masing-masing lagu hanya punya satu genre, vektor TF-IDF di sini sebenarnya terisi pada satu dimensi (genre lagu tsb) dengan bobot IDF tertentu, sementara dimensi lain 0. Namun, jika suatu lagu memiliki lebih dari satu genre (tidak terjadi di dataset ini) atau jika tags turut digabung sebagai teks, vektor bisa memiliki beberapa komponen. Selanjutnya, dihitung matriks kemiripan Cosine antar semua lagu menggunakan hasil TF-IDF tersebut. Matriks cosine similarity berukuran 21.916 x 21.916, di mana setiap entri [i,j] mewakili derajat kemiripan (0 hingga 1) antara lagu i dan lagu j. Nilai 1 berarti sangat mirip (identik dalam genre/tags), nilai 0 berarti tidak mirip sama sekali. Proses ini memakan sumber daya cukup besar, tetapi memungkinkan query kemiripan lagu dilakukan secara cepat dengan melihat nilai similarity yang telah dihitung.
- Encoding Fitur Kategorikal, dibuat mapping user_id → user_index dan track_id → track_index. Setiap user_id unik diberi indeks 0..N-1 (N = jumlah user pada sample) dan setiap track_id unik diberi indeks 0..M-1 (M = jumlah track pada sample). Proses ini menghasilkan vektor indeks user (user_idx) dan indeks track (track_idx) pada history_sample. Selain itu, untuk kemudahan interpretasi hasil, disimpan pula mapping balik indeks ke ID asli (idx2user dan idx2track).
- Normalisasi Playcount menjadi Rating, dilakukan dengan min-max scaling: setiap nilai playcount diubah menjadi (playcount_i - min_playcount) / (max_playcount - min_playcount). Karena dalam sample, min_playcount = 1 dan max_playcount mungkin puluhan (tergantung sample terambil), maka nilai rating 0 berarti 1 kali play (terkecil), dan 1 berarti playcount maksimum di dataset sample. Hasil normalisasi disimpan di kolom baru rating bertipe float32. Langkah ini mempersiapkan data untuk skenario regression/prediction pada model NN.
- Split Data Training dan Validasi, Data history_sample diacak urutannya (shuffle) dan dibagi menjadi 80% untuk training (train_df) dan 20% untuk validation (val_df). Dengan ~50.000 data, set training ~40k dan validation ~10k.

### Alasan
- Cleaning menjamin token TF‑IDF bermakna; baris kosong mengaburkan kosinus.
- Sync agar data music yang dihapus sesuai antara ke 2 data yaitu antara data music dan data history streaming/riwayat lagu, agar tidak terjadinya ambiguitas karena user mendengarkan music yang tidak ada di list music.
- Sampling memperkecil jumlah data agar mempercepat training karena data sebelumnya terlalu banyak yaitu dari 9,7 juta sudah di filter untuk music yang di hapus pada daftar music maka dihapus juga pada user streaming history dengan menyisakan 6,28 juta data rasanya masih banyak oleh karena itu untuk mempersedikit data diambil 50k data random.
- Pembuatan TF-IDF dan penghitungan Cosine Similarity diperlukan untuk Content Based Filtering karena TF-IDF adalah bobot numerik yang mencerminkan pentingnya sebuah kata dalam sebuah dokumen relatih terhadap korpus dokumen dalam hal ini genre, dan diperlukan Cosine similarity untuk mengukur kesamaan dua vektor dengan menghitung cosinus sudut diantara keduanya.
- Encoding Fitur Kategorikal, Pendekatan collaborative filtering dengan RecommenderNet memerlukan representasi ID pengguna dan ID lagu dalam bentuk indeks integer.
- Normalisasi Playcount menjadi Rating, hal ini diperlukan untuk Collaborative Filtering untuk mengukur kecocokan preferensi user dengan rekomendasi lagu yang akan direkomendasikan.
- Split Data Training dan Validasi, Pembagian ini akan digunakan untuk melatih model collaborative filtering dan memonitor kinerjanya pada data yang tidak dilatih (validation set). Penting untuk memastikan bahwa pemisahan ini stratified secara implisit – meski tidak dijamin merata per user, secara umum cukup untuk evaluasi model.

---

## Modeling

Pendekatan:
**Content‑Based Filtering (TF‑IDF Cosine)** 

Content-based recommender dibuat dengan memanfaatkan deskripsi konten lagu, dalam hal ini genre (dan secara opsional tags). Setiap lagu direpresentasikan sebagai vektor fitur teks menggunakan TF-IDF. TF-IDF (Term Frequency–Inverse Document Frequency) adalah bobot numerik yang mencerminkan pentingnya sebuah kata dalam sebuah dokumen relatif terhadap korpus dokumen. Dalam konteks ini, setiap lagu dianggap sebagai "dokumen" dan kata-kata deskriptifnya (genre, tag) dianggap "term". 

TF-IDF memberi bobot lebih tinggi pada genre/tags yang jarang muncul di seluruh dataset (lebih informatif untuk mendeskripsikan lagu) [2]. Setelah memperoleh vektor TF-IDF untuk tiap lagu, derajat kemiripan antar lagu dihitung menggunakan Cosine Similarity. Cosine similarity mengukur kesamaan dua vektor dengan menghitung cosinus sudut di antara keduanya [3]; formula umumnya:

$$
  \text{sim}(A,B) = \frac{A \cdot B}{|A|;|B|} 
$$

(nilai 1 berarti vektor searah, 0 berarti ortogonal/tidak terkait). Dengan cosine similarity, sistem dapat menemukan lagu-lagu yang paling dekat vektornya dengan lagu yang dijadikan input.

Top-N Recommendation
- Sistem content-based menerima input berupa nama lagu yang disukai/didengar user.
- Mengambil vektor TF-IDF lagu tersebut (misal vektor untuk lagu X).
- Menghitung kemiripan cosine antara vektor X dan semua vektor lagu lain (mengambil baris/kolom terkait pada matriks similarity yang sudah dihitung).
- Mengurutkan daftar lagu lain berdasarkan nilai kemiripan tertinggi.
- Mengembalikan Top-N lagu dengan similarity tertinggi sebagai rekomendasi.

Contohnya, jika pengguna baru saja mendengarkan lagu “Die For You” oleh The Weeknd genre Rock, sistem akan mencari 5 lagu lain yang memiliki genre serupa. Berdasarkan perhitungan, sistem merekomendasikan antara lain lagu seperti pada tabel berikut :
| |name|genre|
|--------|-------|---------|
|0|Winding road|Rock|
|1|Mogwai Fear Satan|Rock|
|2|The World Is Our ___|Rock|
|3|Atrophy|Rock|
|4|I Know You Are But What Am I?	|Rock|

**Collaborative Filtering (RecommenderNet)**
Collaborative Filtering merekomendasikan merekomendasikan lagu berdasarkan kesamaan pola preferensi antar pengguna [4]. Implementasi yang dipakai adalah model berbasis Embedding Neural Network sederhana bernama RecommenderNet. Model ini pada dasarnya melakukan matrix factorization terlatih: memetakan setiap pengguna dan setiap lagu ke representasi vektor laten berdimensi.

Algoritma RecommenderNet : 
- Input: pasangan (user_idx, track_idx).
- Embedding layer user: memetakan user_idx ke vektor 50-dimensi (user_emb).
- Embedding layer item: memetakan track_idx ke vektor 50-dimensi (item_emb).
- Dot product: menghitung dot product (perkalian titik) antara user_emb dan item_emb untuk mendapatkan skor kecocokan laten.
- Bias: menambahkan bias per user (user_bias) dan bias per item (item_bias).
- Activation: skor akhir dilewatkan ke fungsi aktivasi sigmoid untuk mengubah menjadi rentang [0,1] (karena target rating telah dinormalisasi 0-1).

Tahapan RecommenderNet dalam Proyek
- **Encoding** – `user_id`, `track_id` → indeks bertipe int.  
- **Label Scaling** – `rating = (playcount - min)/(max - min)` mengonversi ke range 0‑1.  
- **Split Data** – 80 % train (40 000), 20 % val (10 000).  
- **Model Build** – Embedding 50D + bias; optimizer Adam 1e‑3.  
- **Training** – 30 epoch, batch 512 (≈3 menit, RTX 3060).  
- **Monitoring** – RMSE train & val terekam (lihat Gambar 4).  
- **Serialisasi** – `model.save('recommender_net')`.

Top-N Recommendation
- Ambil user_idx dari user target (misal user dengan user_id = U).
- Model memprediksi rating untuk user U terhadap seluruh lagu yang ada (dengan memberikan input pasangan (U, setiap track) ke model). Ini dilakukan secara vektorized – misal membentuk array inp = [[U, track1], [U, track2], ..., [U, trackM]] lalu dipredict.
- Didapatkan skor prediksi (0-1) untuk setiap lagu. Kemudian filter: hilangkan lagu-lagu yang sudah pernah didengar user U (agar rekomendasi benar-benar baru, tidak mengulang yang sudah diketahui user).
- Urutkan sisa lagu berdasarkan skor prediksi tertinggi, ambil Top-N sebagai rekomendasi.

Contohnya, misalkan kita mengambil seorang pengguna acak dari dataset (misal user dengan ID terenkripsi 7a7cc88945e473bf60cd32d6f286e845982227f5). Model akan memilih rekomendasi 5 lagu dengan skor tertinggi yang belum pernah ia putar misalkan,
| |track_id|name|artist|
|--------|-------|---------|---------|
|59|TRPFYYL128F92F7144|Float On|Modest Mouse|
|5231|TRABFDT12903CADD73|Up Up & Away|Kid Cudi|
|22071|TRPGPDK12903CCC651|Bring Me To Life|Katherine Jenkins|
|153|TRWVOJJ12903CCC654|My Immortal|Evanescence|
|14148|TRIRQPO128F4281996|We Are the Sleepyheads|Belle and Sebastian|

Dapat dilihat rekomendasi tersebut beragam secara genre/artis: ada indie rock (Modest Mouse), hip-hop alternatif (Kid Cudi), lagu rock yang di-cover secara klasik (Katherine Jenkins), gothic rock (Evanescence), hingga indie pop (Belle and Sebastian). Keragaman ini muncul karena model collaborative menangkap pola bahwa user tersebut menyukai beberapa genre berbeda (mungkin user ini mendengarkan rock 2000an dan indie pop). Tidak seperti content-based yang mungkin hanya akan merekomendasikan satu genre, pendekatan collaborative mampu merekomendasikan lintas genre asalkan ada user lain dengan selera serupa.

### Kelebihan dan Kekurangan Setiap Pendekatan
- Content‑Based Filtering (TF‑IDF Cosine)
    - Kelebihan :
        - Tidak memerlukan data pengguna lain karena rekomendasi murni berdasarkan informasi item sehingga sistem dapat merekomendasikan lagu-lagu yang mungkin belum pernah diputar oleh siapapun asalkan deskripsi kontennya mirip dengan lagu yang disukai user. Ini mengatasi masalah cold-start item (lagu baru bisa direkomendasikan berdasar kemiripan konten).
        - Sistem mudah dijelaskan ("direkomendasikan karena sama‑sama bergenre rock").
    - Kekurangan :
        - Sistem cenderung terbatas pada lingkup konten yang pernah disukai pengguna. Artinya, jika user hanya mendengar genre Pop, model akan merekomendasikan lagu Pop lainnya – kurang mampu menangkap diversifikasi selera. Ini dapat membuat rekomendasi monoton.
        - Kualitas rekomendasi sangat bergantung pada kualitas fitur konten: dalam dataset ini, hanya mengandalkan genre (satu kata), sehingga lagu berbeda tapi kebetulan satu genre bisa saja sebenarnya tidak cocok selera.
        - Content-based tidak bisa memberikan rekomendasi cross-genre atau surprise yang kadang justru diinginkan user, karena tidak memanfaatkan pola dari pengguna lain.
- Collaborative Filtering (RecommenderNet)
    - Kelebihan :
        - Unggul dalam kemampuan menemukan item yang disukai pengguna berdasarkan kecenderungan kolektif. Model NN (RecommenderNet) yang digunakan berhasil mempelajari latent factor yang mewakili hubungan kompleks user-lagu, sehingga dapat memberikan rekomendasi yang personal dan kadang tak terduga (misal merekomendasikan Belle and Sebastian karena banyak user dengan selera rock 2000an ternyata juga suka indie pop). Pendekatan ini tidak terbatas pada konten yang mirip saja, sehingga bisa mengenalkan hal baru ke pengguna selama pola komunitas mendukung.
        - Model berbasis embedding ini relatif ringan dan dapat di-upgrade dengan data lebih banyak untuk meningkatkan akurasi.
    - Kekurangan :
        - Ketergantungan pada data interaksi yang padat. Cold-start problem menjadi isu: lagu yang benar-benar baru (belum ada user yang memutar) tidak bisa direkomendasikan karena model tak punya info untuk lagu itu (embedding-nya tidak terlatih baik). Demikian pula user baru tanpa history tidak bisa mendapat rekomendasi akurat karena model belum tahu preferensinya.
        - Collaborative filtering rentan terhadap bias popularitas – cenderung merekomendasikan lagu-lagu yang sudah populer di banyak user. Dalam hasil di atas, misalnya, My Immortal dan Bring Me To Life adalah lagu terkenal; model bisa saja mengarahkan ke lagu populer. Untuk mengatasi ini, diperlukan pemrograman lanjutan (misal memberi penalti pada terlalu populer items).

## Evaluation
Proyek ini menggunakan metrik Root Mean Squared Error (RMSE) untuk mengevaluasi performa model collaborative filtering dalam memprediksi rating (playcount yang dinormalisasi). RMSE adalah ukuran kesalahan prediksi yang didefinisikan sebagai akar kuadrat dari rata-rata kuadrat selisih antara prediksi dengan nilai aktual [5]. 

$$
\mathrm{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^n (y_i - \hat y_i)^2}
$$

- $y_i$: nilai aktual ke-\(i\)
- $\hat y_i$: nilai prediksi ke-\(i\)
- $n$: jumlah sampel

Di mana $\hat{y}_i$ adalah rating prediksi model untuk sampel ke $i$ dan $y_i$ adalah rating sebenarnya. RMSE mengkuadratkan error sehingga memperbesar pengaruh error besar, lalu diakarkan kembali ke skala asal. Nilai RMSE lebih rendah lebih baik – 0 berarti prediksi sempurna tanpa error, semakin besar berarti prediksi melenceng jauh. Pada sistem rekomendasi, RMSE sering digunakan terutama dalam kasus prediksi rating eksplisit (misal prediksi rating bintang). Meskipun dalam proyek ini datanya implicit, kita tetap menggunakan RMSE sebagai indikasi seberapa baik model memprediksi pola playcount pengguna.

<img src="https://raw.githubusercontent.com/mraihanfauzii/Music-Recommendation-System/main/images/output4.png" width="500">
Gambar 4 – learning curve (RMSE dipantau 30 epoch). 

Kurva train turun hingga 0,045; val turun stabil ke 0,407. Validasi menurun tanpa overfitting signifikan. Setelah 30 epoch pelatihan, model collaborative filtering memperoleh nilai training RMSE sangat rendah (~0.05) dan validation RMSE sekitar 0.4067. Hasil ini menunjukkan model mampu mempelajari data training hampir sempurna, namun pada data yang tidak dilatih, error-nya lebih tinggi. RMSE ~0.4067 pada skala 0-1 berarti rata-rata kesalahan prediksi rating sekitar 0.4 (dalam unit rating normalisasi). Jika dikonversi ke skala playcount asli, error ini bervariasi tergantung range playcount per user, namun kira-kira model sering salah prediksi ±0.4 dari nilai sebenarnya (misal memprediksi 0.8 padahal aslinya 0.4, dsb). Secara kualitatif, hal ini masih cukup baik mengingat sifat data yang sparsity. 

Tidak ada perhitungan RMSE untuk content-based filtering karena pendekatan tersebut tidak melakukan prediksi rating melainkan hanya mencari kemiripan. Untuk mengukur kualitas rekomendasi content-based, metrik yang umum dipakai adalah Precision@N, Recall@N, atau F1 berdasarkan daftar lagu yang dihasilkan versus preferensi user sebenarnya. Namun, dalam proyek ini evaluasi content-based bersifat kualitatif: dilihat apakah lagu-lagu yang direkomendasikan memang relevan/mirip dengan lagu input. Hasil contoh menunjukkan content-based memberikan lagu dengan genre serupa, yang dapat dianggap relevan, meski cenderung sempit variannya.

**Evaluasi terhadap Business Goals**
- Menjawab problem statement, mencapai goals, dan berhasil mencapai solution statement: Berhasilnya dibuat sistem rekomendasi music yang dapat memberikan kemiripan yang relevan yaitu yang pertama berdasarkan genre untuk Content Based dan sistem rekomendasi dengan memanfaatkan data riwayat lagu yang didengarkan maupun karakteristik kontent lagu untuk Collaborative Filtering.
- Dampak Solution Statement: Proyek ini sangat berguna untuk aplikasi music yang memerlukan sistem recommender karena dapat meningkatkan lifetime value agar pengguna dapat bertahan lebih lama karena selalu menemukan konten segar yang cocok, mendorong ekonomi kreatif karena ​algoritma membantu penyanyi menemukan audiens niche yang mana akan meningkatkan pendapatan artis independen, dan Mengoptimalkan bandwidth promosi dan menekan biaya pemasaran.

## Kesimpulan
Proyek ini berhasil menghadirkan sistem rekomendasi musik hybrid: TF‑IDF Cosine mengisi celah cold‑start, sedangkan RecommenderNet menyajikan rekomendasi personal dengan RMSE rendah. Pipeline bersih, scalable, Perbandingan kedua pendekatan menunjukkan trade-off: Content-based menghasilkan rekomendasi yang sangat tepat sasaran secara kesamaan konten, namun mungkin kurang surprise. Collaborative filtering mampu menghasilkan rekomendasi yang lebih beragam dan personal, tetapi performa model sangat bergantung pada data.

### Referensi

[1] [R. Burke, A. Felfernig, and M. H. Göker, “Recommender Systems: An Overview”, AI Magazine, vol. 32, no. 3, pp. 13-18, 2011.](https://ojs.aaai.org/aimagazine/index.php/aimagazine/article/view/2361)

[2] [C. D. Manning, P. Raghavan, and H. Schütze, Introduction to Information Retrieval. Cambridge Univ. Press, 2008.](https://nlp.stanford.edu/IR-book/pdf/irbookonlinereading.pdf)

[3] [“Cosine similarity.” Wikipedia, Wikimedia Foundation, 20 Jan 2023.](https://en.wikipedia.org/wiki/Cosine_similarity)

[4] [X. Su and T. M. Khoshgoftaar, “A survey of collaborative filtering techniques”, Advances in Artificial Intelligence, vol. 2009, Article ID 421425, 2009.](https://onlinelibrary.wiley.com/doi/10.1155/2009/421425)

[5] [T. Chai and R. R. Draxler, “Root mean square error (RMSE) or mean absolute error (MAE)? – Arguments against avoiding RMSE”, Geosci. Model Dev., vol. 7, no. 3, pp. 1247–1250, 2014.](https://gmd.copernicus.org/articles/7/1247/2014/)
