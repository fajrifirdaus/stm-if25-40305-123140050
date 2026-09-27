# Tugas 2 — Analisis Sinyal Suara & Noise Statis

**IF25-40305 Sistem Teknologi Multimedia** · Minggu 03 · Institut Teknologi Sumatera
**Nama:** Muhammad Fajri Firdaus · **NIM:** 123140050

Rekaman pembacaan artikel berita di depan sumber derau statis (kipas angin), dibedah ke dalam
empat representasi visual (waveform, spektrum FFT dBFS, spektrogram STFT, Mel-spektrogram),
lalu diuji dampak penurunan laju sampel 48 kHz → 8 kHz dengan dan tanpa filter anti-aliasing.

> Bila GitHub gagal me-*render* notebook (berkas ±8 MB), buka lewat
> [nbviewer](https://nbviewer.org/) dengan menempelkan URL berkas `.ipynb`-nya, atau langsung
> baca `tugas_audio_noise_statis.pdf` di folder ini.

---

## 1. Catatan perekaman

| Aspek | Keterangan |
|---|---|
| **Sumber noise statis** | Kipas angin listrik, berputar pada kecepatan konstan sepanjang perekaman |
| **Posisi perekaman** | Pembaca berada ±0,5–1 meter tepat di depan kipas |
| **Perangkat perekam** | Mikrofon *smartphone* Android |
| **Bahan bacaan** | Beberapa kalimat artikel berita nasional dari portal **detikcom** |
| **Format mentah** | AAC 48 kHz stereo (bawaan perekam) |
| **Format akhir** | WAV PCM 16-bit, 48.000 Hz, stereo, 19,51 detik |

**Mengapa kipas angin?** Derau kipas bersifat *stationary*: statistiknya tidak berubah dari detik
ke detik, sehingga pada spektrogram muncul sebagai pita horizontal yang tidak pernah padam —
mudah dibedakan dari harmonik vokal yang naik-turun mengikuti intonasi. Klaim ini tidak
berhenti di pengamatan mata; di notebook §2.3 sifat tersebut diuji secara kuantitatif.

**Pengolahan sebelum analisis.** Hanya dua, dan keduanya bukan pemrosesan sinyal:

1. Membuang 8.179 sampel bernilai nol murni di awal berkas (artefak *encoder* AAC) — sampel nol
   membuat perhitungan `20*log10(0)` tidak terdefinisi pada grafik dBFS.
2. Memotong ujung rekaman pada detik 19,68 yang jatuh tepat di tengah jeda antar kalimat, agar
   durasi masuk rentang 10–20 detik tanpa ada kata yang terputus.

Tidak ada normalisasi, penguatan, *noise reduction*, maupun filter apa pun yang diterapkan.

---

## 2. Daftar berkas

| Berkas | Isi |
|---|---|
| `tugas_audio_noise_statis.ipynb` | Notebook lengkap beserta seluruh keluaran hasil eksekusi |
| `tugas_audio_noise_statis.pdf` | Ekspor PDF notebook (cadangan visual) |
| `audio_original.wav` | Rekaman asli — WAV PCM 16-bit, 48 kHz, stereo, 19,51 s |
| `audio_downsampled_naive.wav` | Skenario A: desimasi naif `y[::6]` → 8 kHz, **tanpa** filter anti-aliasing |
| `audio_downsampled_clean.wav` | Skenario B: `scipy.signal.resample_poly` → 8 kHz, **dengan** filter anti-aliasing |

Kedua berkas hasil *downsampling* ditulis langsung oleh notebook, sehingga dapat dihasilkan ulang
kapan saja dengan menjalankan ulang seluruh sel.

---

## 3. Ringkasan temuan

**Karakter derau statis.** Energi pita 5–9 kHz hanya bergerak dengan simpangan baku **2,13 dB**
sepanjang 19,5 detik, sementara pita vokal 150–400 Hz bergejolak **8,11 dB** — 3,8 kali lebih
liar. Saat berpindah dari jendela hening ke jendela bicara, pita 12–20 kHz bahkan hanya bergeser
**−0,2 dB**. Derau ini punya dua wajah: keluarga harmonik bernada dasar di kisaran **182–188 Hz**
(khas sumber berputar periodik) dan dataran desis *broadband* di atas 4 kHz (turbulensi udara).
*Noise floor*-nya −39,5 dBFS dengan SNR ±20,9 dB.

**Empat representasi.** Masing-masing menutup kelemahan yang lain: waveform membuktikan "hening"
di rekaman ini bukan nol tetapi buta frekuensi; spektrum FFT menemukan frekuensi dominan derau
tetapi kehilangan sumbu waktu; spektrogram STFT membuktikan derau menyala terus; dan
Mel-spektrogram memberi wilayah formant **54%** tinggi gambar (versus hanya **14,6%** pada skala
linier) sehingga F1≈328 Hz, F2≈1.465 Hz, dan F3≈2.496 Hz terbaca lega dengan data 8× lebih ringkas.

**Aliasing.** Pada rekaman bicara nyata, membandingkan dua spektrum secara langsung hanya
menunjukkan selisih di bawah 1 dB pada hampir seluruh pita — energi asli yang dominan di
frekuensi rendah menutupi energi lipatan. Karena itu aliasing diisolasi lewat sinyal galat
A − B (setelah penyejajaran waktu diverifikasi): hasilnya **−49,94 dBFS RMS, yaitu 25,8 dB di
bawah sinyal**, dengan spektrum rata sepanjang 0–4 kHz dan simpangan baku hanya **1,79 dB** —
**2,7 kali lebih stasioner** daripada sinyal yang menampunginya. Galat yang stasioner hanya bisa
berasal dari sumber yang stasioner, sehingga pelakunya teridentifikasi: **desis kipas yang tadinya
aman di frekuensi tinggi terpantul balik dan mencemari pita vokal.**

**Verifikasi rumus.** Nada penanda 6.500 Hz yang disuntikkan ke salinan rekaman muncul persis di
**1.500 Hz** sesuai ramalan |f − k·fs₂|, dengan galat posisi 0,0 Hz dan tingkat hanya 0,01 dB dari
amplitudo aslinya — komponen yang melipat tidak melemah sama sekali. Skenario B menindasnya
23,3 dB hingga lenyap ke lantai derau. Kontrol *chirp* menutup rangkaian bukti: jejak zig-zag
hasil pengukuran berhimpit dengan ramalan analitis di sepanjang empat pembalikan arah.

---

## 4. Menjalankan ulang notebook

```bash
# dari root repositori
uv venv && uv pip install numpy scipy librosa matplotlib soundfile ipykernel
jupyter lab 02_audio_noise_statis/tugas_audio_noise_statis.ipynb
```

Notebook mencari `audio_original.wav` secara defensif, sehingga tetap berjalan baik dari dalam
folder tugas maupun dari root repositori. Seluruh sel bersifat deterministik: menjalankan ulang
akan menghasilkan angka yang sama persis.

**Pustaka utama:** numpy 2.5.3 · scipy 1.18.1 · librosa 1.0.0 · matplotlib 3.11.1 · soundfile 0.14.0

---

## 5. Catatan penggunaan AI

Sesuai ketentuan tugas, AI digunakan sebagai asisten diskusi, pencari referensi sintaks, dan
pembantu *debugging*. Seluruh baris kode di notebook dipahami sepenuhnya, diberi komentar alur
logika, dan setiap grafik diikuti sel Markdown interpretasi dengan bahasa sendiri. Seluruh angka
yang dikutip dalam narasi berasal dari keluaran sel kode di atasnya — tidak ada nilai yang ditulis
manual tanpa perhitungan.
