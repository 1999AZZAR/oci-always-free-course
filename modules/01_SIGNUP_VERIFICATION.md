# Modul 1: Pendaftaran & Verifikasi Akun

Oracle Cloud punya sistem fraud detection yang cukup agresif. Banyak orang gagal di sini bukan karena kartu mereka bermasalah, tapi karena ada data yang tidak cocok dengan yang tercatat di bank. Siapkan semuanya sebelum mulai.

## Apa yang Kamu Butuhkan

- **Email aktif** — Gmail atau Outlook. Hindari email kerja yang punya SPF/DKIM ketat, kadang email verifikasi Oracle masuk ke spam.
- **Nomor HP** — Untuk OTP. Pastikan nomor aktif dan bisa terima SMS internasional.
- **Kartu debit/kredit** — Harus berlogo Visa atau Mastercard dan aktif untuk transaksi internasional. Oracle akan melakukan *temporary hold* sekitar $1 USD (atau ekuivalennya) untuk validasi, lalu dikembalikan dalam 1–3 hari kerja.

### Kartu yang Biasanya Berhasil
- Kartu kredit fisik dari bank besar (BCA, Mandiri, BNI, BRI, CIMB Niaga)
- Jenius m-Card (Visa)
- Jago Mastercard

### Kartu yang Sering Ditolak
- Kartu debit biasa tanpa fitur international transaction
- Kartu virtual/prepaid
- Kartu yang belum pernah dipakai untuk transaksi luar negeri

Jika belum yakin, hubungi bank kamu dan minta diaktifkan untuk transaksi e-commerce internasional sebelum mendaftar.

## Langkah Pendaftaran

1. Buka [oracle.com/cloud/free](https://www.oracle.com/cloud/free/).
2. Masukkan nama depan, nama belakang, dan email. Klik **Verify my email** — buka inbox dan klik link konfirmasi.
3. Setelah email terverifikasi, kamu akan diarahkan ke form detail akun:
   - **Cloud Account Name**: Nama tenancy kamu, huruf kecil, tanpa spasi. Ini permanen dan tidak bisa diubah.
   - **Home Region**: Pilih dengan hati-hati. Lihat penjelasan di [Modul 2](02_REGION_CONCEPTS.md).
   - **Account Type**: Pilih *Individual* atau *Company* sesuai kebutuhan.
4. Isi detail alamat. **Kritis**: nama dan alamat harus identik dengan data billing kartu kamu, termasuk ejaan dan singkatan (misal "Jl." atau "Jalan").
5. Masukkan detail kartu. Oracle akan memproses hold sementara.
6. Klik **Start my free trial** dan tunggu email konfirmasi.

Proses bisa instan atau bisa memakan waktu beberapa jam. Jika lebih dari 24 jam belum ada kabar, cek folder spam.

## Setelah Berhasil — Verifikasi

Setelah menerima email konfirmasi, login ke [cloud.oracle.com](https://cloud.oracle.com):

1. **Cek tipe akun**: Klik ikon profil kanan atas → **Tenancy** → lihat di bagian **Cloud Account Type**. Jika terbaca *Free* atau *Trial*, kamu belum upgrade ke PAYG (belum perlu sekarang).
2. **Catat Home Region**: Di halaman yang sama, lihat **Home Region** — ini tidak bisa diubah.
3. **Cek limit Always Free**:
   - Menu → **Governance** → **Limits, Quotas and Usage**
   - Filter dengan **Service**: `Compute` → **Resource**: `Standard1-Micro` dan `A1-Flex`
   - Pastikan kamu melihat kuota `2` atau `4` OCPU untuk ARM
4. **Cek kartu**: Pastikan *temporary hold* sudah hilang dari rekening (1–3 hari).

Jika semua terlihat normal, lanjut ke [Modul 2](02_REGION_CONCEPTS.md).

## Troubleshooting

**Error generik saat validasi kartu:**
- Matikan VPN dan ad-blocker sebelum mendaftar.
- Gunakan Chrome atau Edge dalam mode normal (bukan Incognito).
- Pastikan nama di form sama persis dengan nama di kartu.

**Akun terblokir setelah beberapa percobaan gagal:**
- Tunggu 24–48 jam sebelum mencoba lagi dengan email atau kartu berbeda.
- Percobaan berulang dengan data yang sama akan memperburuk situasi.
- Gunakan opsi **Chat** di halaman sign-up Oracle untuk bicara dengan support.

**Hold $1 muncul tapi akun tidak jadi:**
- Ini berarti kartunya valid tapi ada issue lain (biasanya data tidak cocok). Hubungi Oracle support melalui form di situs mereka.
