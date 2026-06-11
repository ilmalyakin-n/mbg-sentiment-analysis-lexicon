# Analisis Sentimen Program Makan Bergizi Gratis (MBG) — Versi Lexicon InSet

Selamat datang di repositori proyek penelitian analisis sentimen terkait program **Makan Bergizi Gratis (MBG)** memanfaatkan data media sosial X (Twitter). Repositori ini berisi implementasi *pipeline* pemrosesan data tradisional yang dikombinasikan dengan pelabelan otomatis berbasis kamus leksikon (**Lexicon InSet**) serta komparasi pemodelan menggunakan arsitektur **Long Short-Term Memory (LSTM)** dan **Bidirectional LSTM (Bi-LSTM)**.

---

## 📌 Deskripsi Proyek
Penelitian ini bertujuan untuk menangkap respon publik terhadap program prioritas pemerintah, yaitu Makan Bergizi Gratis (MBG). Melalui repositori ini, dilakukan eksperimen komparasi untuk melihat sejauh mana kualitas pelabelan manual/rule-based tradisional (Lexicon) memengaruhi proses pembelajaran model *Deep Learning* sekuensial satu arah (LSTM) dan dua arah (Bi-LSTM).

## 🛠️ Alur Preprocessing Data
Data mentah hasil *crawling* melalui platform X dibersihkan secara intensif melalui tahapan berikut:
1. **Text Cleaning**: Menghapus URL, mention, hashtag, karakter non-Abjad, dan spasi berlebih.
2. **Case Folding**: Mengubah seluruh teks menjadi huruf kecil (*lowercase*).
3. **Normalisasi Slang**: Mengubah kata-kata tidak baku/singkatan menjadi kata formal sesuai KBBI.
4. **Tokenisasi**: Memecah kalimat menjadi potongan kata tunggal (token).
5. **Stopword Removal**: Menghapus kata umum yang miskin makna semantik (menggunakan kamus Sastrawi + kustomisasi kata jangkar).
6. **Stemming Sastrawi**: Mengembalikan kata berimbuhan menjadi kata dasar murni.

## 🏷️ Metode Pelabelan: Lexicon InSet
Proses pelabelan *ground truth* pada versi ini memanfaatkan **Kamus InSet (Indonesian Sentiment Lexicon)**. Setiap token dievaluasi berdasarkan nilai polaritas positif atau negatif yang terdaftar di dalam kamus, kemudian diakumulasikan menjadi skor total untuk menentukan kelas akhir: **NEGATIVE, NEUTRAL, atau POSITIVE**.

---

## 📊 Hasil Pengujian & Evaluasi Model
Eksperimen dilakukan dalam **4 Skenario** variasi kapasitas jaringan (*hidden units*) dan batasan pelatihan (*epochs*) menggunakan metode *Class Weight* untuk menyeimbangkan bias data.

### Tabel Ringkasan Kinerja Jaringan (Lexicon)
| Skenario | Epoch | Units | Test Acc | Precision | Recall | F1-Score | Train Acc | Val Acc |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Skenario 1** | 36 | 16 | **0.6503** | **0.5840** | 0.6503 | 0.5840 | 0.7245 | 0.6120 |
| **Skenario 2** | 32 | 32 | 0.6004 | 0.4990 | 0.6004 | 0.4990 | 0.6850 | 0.5930 |
| **Skenario 3** | 33 | 48 | 0.6049 | 0.5035 | 0.6049 | 0.5035 | 0.6910 | 0.5985 |
| **Skenario 4** | 26 | 64 | 0.6342 | 0.5324 | 0.6342 | 0.5324 | 0.7020 | 0.6050 |
| **RATA-RATA** | — | — | **0.6008** | **0.4990** | — | — | — | — |

> ⚠️ **Catatan Penting Hasil Eksperimen (Anomali Teoretis):** > Saat dilatih menggunakan label hasil Lexicon, arsitektur **Bi-LSTM justru mencatatkan performa rata-rata yang lebih rendah** (Akurasi Rata-rata: **59.86%**, F1-Score Rata-rata: **46.57%**) dibandingkan LSTM biasa. Uji statistik parametrik (*Paired t-test*) juga mengonfirmasi nilai p-value sebesar **0.9028** ($p > 0.05$), yang berarti perbedaan kinerja kedua model dinyatakan **Tidak Signifikan**.

## 📉 Temuan & Keterbatasan Metode
1. **Context-Blindness**: Pendekatan leksikon tidak mampu memahami urutan kata, klausa pembalik makna (negasi kustom), maupun ekspresi sarkasme publik.
2. **Garbage In, Garbage Out**: Banyaknya salah label (*misclassification*) pada data latih membingungkan arsitektur kompleks seperti Bi-LSTM, sehingga performanya drop di bawah model baseline dasar (LSTM).

---
