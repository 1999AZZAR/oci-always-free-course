# Modul 5: Setup Tailscale Mesh VPN

Tailscale membuat koneksi peer-to-peer terenkripsi (WireGuard di bawahnya) antara semua perangkatmu. Setelah terpasang di server OCI dan laptop kamu, keduanya bisa saling berkomunikasi melalui IP internal (`100.x.x.x`) tanpa melewati internet publik.

Manfaat langsungnya: kamu bisa menutup semua port publik di Security List OCI, termasuk port 22 (SSH), dan tetap bisa mengakses server dengan aman.

## Prasyarat

- Instance OCI sudah berjalan (Modul 3).
- Kamu sudah bisa SSH ke instance via IP publik (Modul 4).
- Akun Tailscale gratis — daftar di [tailscale.com](https://tailscale.com) pakai Google atau GitHub.

## Instalasi di Server OCI

SSH ke server terlebih dahulu:
```bash
ssh -i ~/path/to/oracle_key.key ubuntu@<public-ip>
```

Jalankan installer resmi Tailscale:
```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

Aktifkan Tailscale dengan fitur SSH bawaan (Tailscale SSH):
```bash
sudo tailscale up --ssh
```

Terminal akan menampilkan URL autentikasi. Buka URL tersebut di browser, login dengan akun Tailscale kamu, dan klik **Connect**. Selesai.

Periksa IP Tailscale yang diberikan ke server:
```bash
tailscale ip -4
```

Catat IP ini (formatnya `100.x.x.x`). Ini adalah IP private server kamu di jaringan Tailscale.

## Buka Port UDP 41641 di OCI (Opsional tapi Disarankan)

Tailscale bisa beroperasi tanpa ini, tapi koneksinya akan melalui relay server (DERP) yang menambah latensi. Membuka port UDP 41641 memungkinkan koneksi direct peer-to-peer.

Di OCI Console:
1. Pergi ke **Networking** → **Virtual Cloud Networks** → klik VCN → **Security Lists**.
2. Tambahkan Ingress Rule:
   - **Source CIDR**: `0.0.0.0/0`
   - **IP Protocol**: UDP
   - **Destination Port Range**: `41641`
3. Simpan.

## Instalasi di Laptop/PC Kamu

Download Tailscale untuk sistem operasi kamu dari [tailscale.com/download](https://tailscale.com/download) dan login dengan akun yang sama.

Setelah terhubung, kamu bisa langsung SSH ke server menggunakan IP Tailscale:
```bash
ssh ubuntu@100.x.x.x
```

Jika kamu mengaktifkan Tailscale SSH (`--ssh` flag tadi), kamu bahkan tidak perlu menentukan private key secara manual — autentikasi dihandle oleh Tailscale identity kamu.

## Keuntungan Tailscale SSH

Dengan `--ssh` flag aktif, akses SSH dikontrol lewat **Tailscale Admin Console** di [login.tailscale.com/admin/acls](https://login.tailscale.com/admin/acls). Ini berarti:

- Tidak perlu manage `~/.ssh/authorized_keys` secara manual.
- Kamu bisa memberikan atau mencabut akses per user tanpa menyentuh server.
- Semua akses tercatat di audit log Tailscale.

## Langkah Berikutnya

Setelah Tailscale berjalan dan kamu bisa SSH via IP `100.x.x.x`, lanjut ke [Modul 6](06_SECURE_SSH.md) untuk menutup port publik dan mengamankan akses sepenuhnya.
