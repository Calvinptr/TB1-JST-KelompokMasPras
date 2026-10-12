# TB1-JSTKelompokXX: Klasifikasi Kelulusan Siswa dengan Perceptron dan ADALINE

Tugas Besar 1 • Jaringan Syaraf Tiruan (IT6053) • FTI UKDW

## 1. Judul dan Kasus

**Judul:** Analisis dan Implementasi Klasifikasi Kelulusan Siswa Menggunakan Perceptron dan ADALINE

**Kasus:** Memprediksi apakah siswa **Lulus (+1)** atau **Tidak Lulus (−1)** berdasarkan faktor belajar dan lingkungan (jam belajar, kehadiran, skor sebelumnya, motivasi, keterlibatan orang tua, dan lainnya). Kasus dirumuskan sebagai *supervised learning* klasifikasi dua kelas. Label dibuat dari `Exam_Score` dengan ambang **[isi: ambang, misalnya ≥ 68]**; kelas positif = Lulus.

**Tujuan:**
- Mengimplementasikan Perceptron dan ADALINE (Delta Rule) secara mandiri dengan NumPy.
- Membandingkan proses belajar kedua model pada data dan pembagian data yang sama.
- Menilai apakah data dapat dipisahkan secara linear, berdasarkan pola data dan perilaku pelatihan.

## 2. Identitas Anggota

**Kelompok:** MasPras

| No | Nama | NIM |
|---|---|---|
| 1 | **Jimmy Christian** | **71230974** |
| 2 | **Prastha Pradipta Purusa** | **71230987** |
| 3 | **Calvin Putra Susanto** | **71230988** |

## 3. Struktur dan Tautan Berkas

| Berkas | Keterangan |
|---|---|
| [`README.md`](README.md) | Dokumen ini |
| [`TB1_KelompokMasPras.ipynb`](TB1_KelompokMasPras.ipynb) | Notebook lengkap: kode, penjelasan, dan keluaran eksperimen |
| [`TB1_KelompokMasPras.pdf`](TB1_KelompokMasPras.pdf) | Laporan (5–8 halaman isi utama) |
| [`Presentasi_TB1.pdf`](Presentasi_TB1.pdf) | Slide presentasi |
| [`dataset.csv`](dataset.csv) | Data yang digunakan dalam eksperimen |
| [`requirements.txt`](requirements.txt) | Dependensi dan versinya |

**Repository:** `https://github.com/Calvinptr/TB1-JST-KelompokMasPras.git`

## 4. Sumber dan Deskripsi Data

**Sumber:** Kaggle, *Student Performance Factors*
https://www.kaggle.com/datasets/lainguyn123/student-performance-factors/data
**Lisensi dan izin penggunaan:** **CC0: Public Domain**
**Berkas CSV di repository:** `dataset.csv` berisi data yang benar-benar dipakai (data asli ditambah kolom label biner).

**Ringkasan data** (verifikasi dengan notebook):

| Item | Nilai |
|---|---|
| Jumlah kolom asli | **40** |
| Missing value | **3 Kolom** |
| Duplikat | **0** |
| Fitur setelah encoding | 40 |
| Distribusi kelas (+1 / −1) | **+1** / **-1** |

**Kolom dan transformasi:**

| Kelompok | Kolom | Perlakuan |
|---|---|---|
| Numerik | `Hours_Studied`, `Attendance`, `Sleep_Hours`, `Previous_Scores`, `Tutoring_Sessions`, `Physical_Activity` | Imputasi median (dari data latih), standardisasi |
| Ordinal | `Parental_Involvement`, `Access_to_Resources`, `Motivation_Level`, `Family_Income`, `Teacher_Quality` | Imputasi modus; Low/Medium/High → 0/1/2 |
| Biner | `Extracurricular_Activities`, `Internet_Access`, `Learning_Disabilities`, `Gender`, `School_Type` | Pemetaan ke 0/1 |
| Nominal | `Peer_Influence`, `Parental_Education_Level`, `Distance_from_Home` | Imputasi modus; one-hot |
| Target | `Exam_Score` | Diubah menjadi label: skor ≥ **[isi]** → **+1** (Lulus), selainnya **−1**. **Tidak dipakai sebagai fitur.** |

**Alasan ambang label:** **[isi]**

**Urutan pengolahan (anti-kebocoran):** split latih/uji (80:20, stratifikasi) → imputasi → encoding → standardisasi. Seluruh parameter (median, modus, rata-rata, simpangan baku) dipelajari **hanya dari data latih**, lalu diterapkan ke data uji.

## 5. Instalasi dan Cara Menjalankan

```bash
# 1. Clone repository
git clone https://github.com/[isi]/TB1-JSTKelompokXX.git
cd TB1-JSTKelompokXX

# 2. (Disarankan) buat virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Pasang dependensi
pip install -r requirements.txt

# 4. Jalankan notebook
jupyter notebook TB1_KelompokXX.ipynb
```

Jalankan seluruh sel dari awal sampai akhir (*Restart & Run All*). Notebook membaca `dataset.csv` dari direktori yang sama, jadi tidak perlu mengunduh data dari Kaggle.

## 6. Versi Python dan Seed

| Item | Nilai |
|---|---|
| Python | **3** |
| Seed (`random_state`) | 42 |
| Pembagian data | 80:20, stratifikasi kelas |
| Inisialisasi parameter | w = 0, b = 0 |
| Urutan sampel | Tetap (tidak diacak) |
| Versi paket | Lihat `requirements.txt` |

## 7. Ringkasan Hasil

**Delapan konfigurasi eksperimen** (data latih dan uji yang sama untuk semua):

| # | Model | η | Epoch | Standardisasi | Akurasi dan latih uji | Error/loss akhir |
|---|---|---|---|---|---|---|
| 1 | Perceptron | 0,001 | 50 | Ya | 0.9357 | 333 |
| 2 | Perceptron | 0,01 | 50 | Ya | 0.9357 | 333 |
| 3 | Perceptron | 0,1 | 50 | Ya | 0.9357 | 333 |
| 4 | ADALINE | 0,001 | 50 | Ya | 0.9364 | 46 |
| 5 | ADALINE | 0,01 | 50 | Ya | 0.9493 | 29 |
| 6 | ADALINE | 0,1 | 50 | Ya | 0.9614 | 18 |
| 7 | Perceptron | 0,01 | 50 | Tidak | 0.6256 | 1452 |
| 8 | ADALINE | 0,01 | 50 | Tidak | 0.4554 | 1861190….. |

*Loss ADALINE dicatat **sebelum** pembaruan bobot pada tiap epoch (J = (1/2n)Σ(y − z)²).*

**Temuan utama:**
- Perceptron (η = 0,01, terstandardisasi): jumlah update turun dari 519 (epoch 1) ke 327 (epoch 3), lalu berfluktuasi di kisaran 291–348 dan tidak pernah mencapai nol. Data **tidak separabel linear sempurna, tetapi mendekati separabel**.
- Dengan inisialisasi nol dan urutan data tetap, perubahan η pada Perceptron hanya menskalakan bobot sehingga prediksi identik. **[isi: konfirmasi dari hasil 3 η]**
- ADALINE: **[isi: perilaku kurva J untuk tiga η, dan epoch divergen jika ada]**
- Pengaruh standardisasi: **[isi: perbandingan eksperimen 7–8 dengan 2 dan 5]**
- Confusion matrix, precision, recall, dan F1 kelas positif: lihat notebook dan laporan.

**Keterbatasan:** label dibuat dari ambang `Exam_Score`; hanya model linear; satu split tanpa validasi silang; dataset diduga sintetis; akurasi dapat menyesatkan jika kelas tidak seimbang.

## 8. Referensi

1. Balde, B. (2023, Januari 28). Perceptron: Building it from scratch in python. Medium.
   https://medium.com/@becaye-balde/perceptron-building-it-from-scratch-in-python-15716806ef6
2. Lain, G. (n.d.). Student performance factors. Kaggle.
   Diambil dari https://www.kaggle.com/datasets/lainguyn123/student-performance-factors/data
3. GeeksforGeeks. (2023, 20 Oktober). Perceptron class in sklearn. GeeksforGeeks.
   https://www.geeksforgeeks.org/machine-learning/sklearn-perceptron/ 

## 9. Bantuan AI

Kelompok menggunakan **Claude** untuk:
- Memahami ketentuan tugas dan menyusun alur pengerjaan
- Memahami konsep kerja Perceptron dan ADALINE

**Cara verifikasi:**
- Seluruh kode dijalankan dan diperiksa sendiri oleh anggota kelompok.
- Kode ADALINE dan Perceptron dicocokkan dengan perhitungan manual pada subset kecil (2 sampel × 2 fitur); hasilnya sama.
- Nama kolom dan nilai kategori dicek langsung terhadap dataset.
- Rumus diperiksa terhadap panduan tugas dan referensi pada bagian 8.
- Setiap angka pada laporan berasal dari keluaran notebook, bukan dari AI.

## 10. Kontribusi Anggota

| Nama | Kontribusi |
|---|---|
| **Jimmy Christian** | **Menyusun laporan, membuat ppt presentasi, menyusun code eksperimen, mencari artikel dan code pendukung** |
| **Prastha Pradipta Purusa** | **Menyusun laporan, membuat ppt presentasi, menyusun code eksperimen, mencari jurnal pendukung laporan** |
| **Calvin Putra Susanto** | **Menyusun laporan, membuat ppt presentasi, menyusun code eksperimen, menyiapkan github pengumpulan tugas** |

Seluruh anggota bertanggung jawab atas hasil dan mampu menjelaskan data, aturan pembaruan parameter, hasil, serta kontribusinya masing-masing.
