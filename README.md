# stm-if25-40305-123140050

Portofolio tugas mata kuliah **IF25-40305 Sistem Teknologi Multimedia**
Program Studi Informatika — Institut Teknologi Sumatera (ITERA)
Semester Ganjil 2026/2027

| | |
|---|---|
| **Nama** | Muhammad Fajri Firdaus |
| **NIM** | 123140050 |
| **Program Studi** | Informatika |

Repositori ini berisi seluruh tugas perkuliahan Sistem Teknologi Multimedia selama satu semester.
Setiap tugas menempati satu folder bernomor, lengkap dengan notebook yang sudah dieksekusi,
berkas media pendukung, dan catatan tersendiri.

---

## Daftar tugas

| # | Folder | Tugas | Materi | Status |
|---|---|---|---|---|
| 02 | [`02_audio_noise_statis/`](02_audio_noise_statis) | Analisis Sinyal Suara & Noise Statis — representasi audio 4 dimensi, resampling, dan pembuktian aliasing | Minggu 03 | Selesai |

---

## Struktur repositori

```
stm-if25-40305-123140050/
├── README.md                                 # halaman ini
└── 02_audio_noise_statis/
    ├── tugas_audio_noise_statis.ipynb        # notebook lengkap dengan seluruh keluaran
    ├── tugas_audio_noise_statis.pdf          # ekspor PDF notebook
    ├── audio_original.wav                    # rekaman asli (WAV PCM 16-bit, 48 kHz)
    ├── audio_downsampled_naive.wav           # downsampling tanpa filter anti-aliasing
    ├── audio_downsampled_clean.wav           # resampling dengan filter anti-aliasing
    └── README.md                             # catatan perangkat, sumber noise, dan temuan
```

---

## Lingkungan pengembangan

Python 3.12 dengan pustaka saintifik standar:

```bash
uv venv
uv pip install numpy scipy librosa matplotlib soundfile ipykernel
```

**Versi yang dipakai:** numpy 2.5.3 · scipy 1.18.1 · librosa 1.0.0 · matplotlib 3.11.1 ·
soundfile 0.14.0

> Sebagian notebook berukuran besar karena menyimpan seluruh grafik dan pemutar audio hasil
> eksekusi. Bila GitHub gagal me-*render*, gunakan [nbviewer](https://nbviewer.org/) atau baca
> berkas PDF pada folder tugas terkait.

---

## Catatan penggunaan AI

Sesuai ketentuan perkuliahan, AI dipakai sebagai asisten diskusi, pencari referensi sintaks, dan
pembantu *debugging*. Seluruh baris kode dalam repositori ini dipahami sepenuhnya dan diberi
anotasi penjelasan, baik berupa komentar kode maupun sel Markdown analisis.
