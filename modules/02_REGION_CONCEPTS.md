# Modul 2: Pemilihan Region & Konsep Always Free

Memahami bagaimana Oracle membagi wilayah kekuasaannya dan apa arti sebenarnya dari "Gratis Selamanya".

## 1. Memilih Home Region (Kritis!)
Saat mendaftar, kamu diminta memilih **Home Region**. Pikirkan ini baik-baik karena:
- **Tidak bisa diubah**: Sekali pilih, kamu terjebak di sana selamanya.
- **Resource Limitation**: Fitur *Always Free* hanya tersedia di Home Region kamu.
- **Latency**: Pilih yang paling dekat dengan lokasi kamu atau target user kamu.
  - Rekomendasi untuk Indonesia: **Singapore** (Paling stabil/cepat), **Tokyo**, atau **Seoul**.

## 2. Kuota Resource Always Free
Oracle sangat royal, tapi ada batasnya. Berikut detail "Loots" yang bisa kamu ambil:
- **Compute (ARM)**: VM.Standard.A1.Flex. Total 4 OCPU dan 24 GB RAM. Bisa buat 1 instance besar atau 4 instance kecil.
- **Compute (AMD)**: VM.Standard.E2.1.Micro. 2 instance kecil (1 GB RAM masing-masing).
- **Storage**: Total 200 GB Block Volume.
- **Network**: 10 TB Outbound Data Transfer per bulan.

## 3. Strategi "Out of Capacity"
Kadang saat membuat instance ARM, muncul pesan error kapasitas habis. Ini wajar karena banyak orang yang "berburu" resource gratis ini.
**Tips Pro:**
- Coba buat instance di jam-jam sepi (dini hari).
- Gunakan script *OCI Instance Creator* (tersedia di komunitas) untuk mencoba secara otomatis setiap menit.
- **Upgrade ke Pay-As-You-Go**: Ini trik paling ampuh. Kamu tetap tidak akan ditagih selama pemakaian di bawah limit Free Tier, tapi akun kamu diprioritaskan untuk mendapatkan resource.
