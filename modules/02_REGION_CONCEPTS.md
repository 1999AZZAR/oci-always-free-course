# Modul 2: Pemilihan Region & Kuota Always Free

Dua hal paling kritis di modul ini: **pilihan Home Region** dan **pahami batas resource terbaru**. Salah pilih region = tidak bisa diubah selamanya. Salah paham kuota = kaget pas instance kena shutdown.

## Home Region

Home Region adalah lokasi data center utama kamu. Semua resource Always Free **hanya bisa dibuat di Home Region ini**. Sekali dipilih, tidak ada cara untuk mengubahnya kecuali membuat akun baru.

### Pilihan Region untuk Pengguna Indonesia

| Region | Kode | Rekomendasi |
|---|---|---|
| Indonesia North (Batam) | `ap-batam-1` | Latensi terbaik untuk user Indonesia. Tersedia sejak 2024. |
| Singapore | `ap-singapore-1` | Paling stabil, koneksi ke Indonesia ~15–20ms. Pilihan aman. |
| Tokyo | `ap-tokyo-1` | Latensi lebih tinggi (~70ms), tapi kapasitas ARM biasanya lebih longgar. |
| Seoul | `ap-seoul-1` | Mirip Tokyo, kadang lebih mudah dapat ARM instance. |

**Saran**: Jika tujuan utama adalah deploy agent untuk pemakaian pribadi, pilih **Singapore** atau **Batam**. Kalau kamu sering mengalami "Out of Capacity" di Singapore, coba Tokyo atau Seoul — kapasitas ARM di sana biasanya lebih lega.

Kapasitas ARM sangat bergantung pada seberapa banyak orang berburu resource gratis di region tersebut. Tidak ada data resmi soal ini, tapi secara umum region Asia Timur (Tokyo, Osaka, Seoul) punya kapasitas lebih longgar dibanding Singapore.

## Kuota Resource Always Free (Update Juni 2026)

Oracle diam-diam memangkas limit ARM Ampere A1 per tanggal **15 Juni 2026** tanpa pengumuman resmi.

### ARM Ampere A1 Compute (VM.Standard.A1.Flex)

| Jenis Akun | OCPU | RAM |
|---|---|---|
| **Free Tier (akun baru)** | 2 OCPU | 12 GB |
| **Pay-As-You-Go / Akun lama** | 4 OCPU | 24 GB |

Secara teknis, kuota dihitung per bulan dalam satuan jam:
- Free Tier baru: **1.500 OCPU-hours** dan **9.000 GB-hours** per bulan.
- Ini setara dengan 1 instance berjalan non-stop sepanjang bulan dengan 2 OCPU dan 12 GB RAM.

Kamu bisa membagi kuota ini ke beberapa instance kecil — misal 2 instance masing-masing 1 OCPU dan 6 GB RAM.

### Resource Lain yang Tidak Berubah

| Resource | Kuota |
|---|---|
| AMD Micro (VM.Standard.E2.1.Micro) | 2 instance (1/8 OCPU, 1 GB RAM) |
| Block Storage | 200 GB total |
| Object Storage | 20 GB |
| Outbound Transfer | 10 TB/bulan |
| Autonomous Database | 2 database |
| Load Balancer | 1x Flexible (10 Mbps) + 1x NLB |

## Idle Reclamation — Risiko yang Sering Diabaikan

Oracle bisa **otomatis mematikan** instance kamu jika dianggap idle. Kriteria idle selama periode 7 hari:

- CPU utilization < 15% (di persentil ke-95)
- Network utilization < 15%
- Memory utilization < 15% (untuk A1 shapes)

Jika instance dimatikan karena idle dan kamu coba restart, ada kemungkinan tidak bisa — terutama jika kapasitas di region tersebut sedang habis.

**Cara mencegah**: Upgrade ke Pay-As-You-Go. Instance PAYG tidak kena idle reclamation, dan kamu tidak akan ditagih selama pemakaian masih di bawah kuota Always Free.

## Cara Upgrade ke Pay-As-You-Go

1. Login ke OCI Console.
2. Klik ikon profil di kanan atas → **Upgrade and Manage Payment**.
3. Masukkan detail kartu dan konfirmasi upgrade.
4. Instance dan semua resource kamu tidak akan berubah — hanya status akun yang naik.

Upgrade ini tidak membuatmu otomatis kena tagihan. Kamu baru ditagih jika penggunaan melebihi limit Always Free.

## Verifikasi — Pastikan Kamu Siap ke Modul 3

Sebelum lanjut, pastikan hal-hal berikut:

1. **Kamu tahu Home Region-mu** — Login ke OCI Console, lihat di ikon profil → **Tenancy**. Jika bukan Singapore/Batam, catat region kamu.
2. **Kamu paham kuota ARM-mu** — Buka **Governance** → **Limits, Quotas and Usage** → filter service `Compute`, resource `A1-Flex`. Apakah terbaca `2` OCPU atau `4`?
3. **Kamu sudah decide region untuk instance** — Pilih satu region (Home Region) dan catat kode region-nya (misal `ap-singapore-1`).
4. **Kamu siap upgrade ke PAYG jika perlu** — Jika suatu saat ARM instance terus "Out of Capacity", opsi upgrade selalu ada.

Jika semua jelas, lanjut ke [Modul 3](03_CREATE_INSTANCE.md) untuk membuat instance ARM.
