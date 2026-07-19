# Modul 3: Membuat Instance ARM Ampere

Instance ARM Ampere A1 adalah resource paling berharga di OCI Always Free. Prosesor ARM-nya efisien untuk container workload dan jauh lebih besar dari yang ditawarkan cloud provider lain di tier gratis.

## Spesifikasi Target

Sesuaikan dengan jenis akunmu:

| Jenis Akun | OCPU | RAM | Catatan |
|---|---|---|---|
| Free Tier (akun baru, post-Juni 2026) | 2 | 12 GB | Limit baru sejak 15 Juni 2026 |
| Pay-As-You-Go / akun lama | 4 | 24 GB | Limit lama, masih berlaku |

Kamu bisa membuat 1 instance besar atau membaginya ke beberapa instance kecil — selama total OCPU dan RAM tidak melebihi kuota bulanan kamu.

Untuk deployment agent tunggal (OpenClaw atau sejenisnya), **1 instance dengan seluruh kuota** adalah konfigurasi paling praktis.

## Langkah Membuat Instance

1. Login ke [cloud.oracle.com](https://cloud.oracle.com).
2. Dari menu utama, pilih **Compute** → **Instances**.
3. Klik **Create Instance**.

**Bagian Name & Placement**
- Beri nama deskriptif, misal `agent-prod-01`.
- Di bawah **Availability Domain**, kamu akan melihat beberapa AD (biasanya AD-1, AD-2, AD-3). Catat ini — kalau satu AD out of capacity, kamu bisa coba AD lain.

**Bagian Image and Shape**
- Klik **Change Image** → Pilih **Ubuntu** → versi **22.04** atau **24.04** → pilih build **aarch64** (ARM).
- Klik **Change Shape** → pilih tab **Ampere** → pilih `VM.Standard.A1.Flex`.
- Geser slider **Number of OCPUs** dan **Amount of Memory** ke nilai maksimal yang tersedia untuk akunmu (2 OCPU / 12 GB atau 4 OCPU / 24 GB).

**Bagian Networking**
- Pilih VCN yang sudah ada atau biarkan OCI membuat yang baru secara default.
- Pastikan **Assign a public IPv4 address** dicentang — kamu butuh ini untuk SSH pertama kali sebelum Tailscale terpasang.

**Bagian Add SSH Keys**

Kamu punya dua opsi:

*Opsi A — Generate (Mudah, untuk pemula):*
- Pilih **Generate a key pair for me**.
- Klik **Save Private Key** dan simpan file `.key` di folder `~/.ssh/` di laptop kamu.
- Beri nama file yang jelas, misal `oci-agent.key`.
- **Kamu tidak bisa mendownload key ini lagi** setelah halaman ini ditutup.

*Opsi B — Upload existing (Jika sudah punya key pair):*
- Pilih **Upload public key**.
- Pilih file `id_rsa.pub` atau `~/.ssh/authorized_keys` dari laptop kamu.
- Gunakan ini jika kamu ingin memakai key yang sudah terdaftar di GitHub atau server lain.

**Bagian Boot Volume**
- Ubah ukurannya ke **100 GB** — kamu punya total 200 GB gratis, jadi pakai setengahnya untuk boot volume yang cukup lega.

Klik **Create** dan tunggu status instance berubah dari *Provisioning* ke *Running* (biasanya 1–3 menit).

## Mengatasi "Out of Host Capacity"

Error ini sangat umum. Bukan artinya kamu ditolak — hanya berarti slot ARM di Availability Domain tersebut sedang penuh.

**Opsi 1: Ganti Availability Domain**
Kembali ke form, pilih AD yang berbeda (misal dari AD-1 ke AD-2 atau AD-3) dan coba lagi.

**Opsi 2: Kurangi Spesifikasi Sementara**
Coba buat dengan 1 OCPU dan 6 GB RAM — instance lebih kecil lebih mudah dapat slot. Setelah berhasil, kamu bisa resize via Console.

**Opsi 3: Pakai Script Otomatis**
Komunitas sudah membuat script yang looping-request ke OCI API sampai berhasil. Yang paling populer: [oci-arm-host-capacity](https://github.com/hitrov/oci-arm-host-capacity). Bisa dijalankan di OCI Cloud Shell (langsung dari browser, gratis) tanpa harus setup environment lokal.

**Opsi 4: Upgrade ke Pay-As-You-Go**
Akun PAYG mendapat prioritas lebih tinggi untuk alokasi resource. Ini cara paling reliable kalau dua opsi di atas sudah dicoba.

## SSH ke Instance Pertama Kali

Setelah instance Running, salin **Public IP Address**-nya dari detail instance.

Di terminal lokal kamu:
```bash
# Set permission yang benar untuk private key
chmod 400 ~/.ssh/oci-agent.key

# Sambungkan via SSH
ssh -i ~/.ssh/oci-agent.key ubuntu@<public-ip>

# Jika menggunakan port non-standar atau ingin lebih praktis,
# buat SSH config (lihat Modul 6)
```

Default username untuk Ubuntu di OCI adalah `ubuntu`.

## Verifikasi — Instance Siap Dipakai

Setelah berhasil SSH, jalankan perintah berikut untuk memastikan semuanya benar:

```bash
# Cek arsitektur processor (harus aarch64 = ARM)
uname -m

# Cek OS dan versi
cat /etc/os-release | head -4

# Cek CPU dan RAM
lscpu | grep "CPU(s)"
free -h

# Cek disk
df -h /

# Cek akses sudo (harusnya tanpa password)
sudo whoami
```

Output yang diharapkan:
- `uname -m` → `aarch64`
- RAM mendekati 12 GB atau 24 GB (sesuai spek)
- `sudo whoami` → `root` (tanpa error)
- Disk `/` sekitar 100 GB

**Jika semua sesuai, instance ARM kamu sudah siap.** Lanjut ke [Modul 4](04_NETWORK_SECURITY.md) untuk konfigurasi firewall sebelum mengekspos port apapun.
