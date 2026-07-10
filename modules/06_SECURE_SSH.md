# Modul 6: SSH Aman & Akses Tanpa IP Publik

Setelah Tailscale aktif dan kamu sudah berhasil SSH via IP `100.x.x.x`, kamu bisa mengamankan server dengan menutup semua akses publik. Server tetap bisa diakses penuh melalui jaringan Tailscale, tapi bot scanner di internet tidak akan menemukan port yang terbuka.

## 1. Verifikasi Koneksi Tailscale Dulu

Sebelum menutup port publik, pastikan kamu bisa SSH via IP Tailscale dari laptop kamu:

```bash
ssh -i ~/path/to/oracle_key.key ubuntu@100.x.x.x
```

Jika berhasil masuk, lanjut. Jangan tutup port publik sebelum koneksi Tailscale terkonfirmasi — kamu bisa mengunci dirimu sendiri di luar server.

## 2. Tutup Port 22 di OCI Security List

1. Pergi ke **Networking** → **Virtual Cloud Networks** → klik VCN kamu → **Subnets** → klik subnet public.
2. Klik **Default Security List**.
3. Temukan Ingress Rule untuk TCP port 22.
4. Klik ikon titik tiga di sebelah kanan aturan tersebut → **Remove**.
5. Konfirmasi penghapusan.

Mulai sekarang, port 22 tidak bisa diakses dari internet. SSH hanya bisa dilakukan melalui jaringan Tailscale (`100.x.x.x`).

## 3. Konfigurasi ~/.ssh/config (Opsional tapi Sangat Disarankan)

Mengetik `ssh -i ~/path/to/key ubuntu@100.x.x.x` setiap kali cukup membosankan. Buat alias di SSH config:

```bash
nano ~/.ssh/config
```

Tambahkan blok ini (ganti nilai sesuai dengan konfigurasi kamu):

```
Host oci-agent
    HostName 100.x.x.x
    User ubuntu
    IdentityFile ~/path/to/oracle_key.key
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

Simpan, lalu cukup jalankan:
```bash
ssh oci-agent
```

Opsi `ServerAliveInterval` dan `ServerAliveCountMax` berguna untuk mencegah koneksi idle terdrop saat kamu tidak mengetik apa-apa dalam waktu lama.

## 4. Proteksi Private Key

File `.key` yang kamu download waktu buat instance harus dilindungi. SSH menolak key dengan permission yang terlalu longgar:

```bash
chmod 400 ~/path/to/oracle_key.key
```

Simpan key ini di direktori `~/.ssh/` dan pastikan direktori tersebut juga punya permission yang benar:
```bash
chmod 700 ~/.ssh
```

Kalau kamu pakai Tailscale SSH (dengan `--ssh` flag dari Modul 5), kamu bisa skip penggunaan private key sama sekali — Tailscale yang handle autentikasi.

## 5. Backup Private Key

Simpan satu copy di password manager kamu (Bitwarden, 1Password, atau sejenisnya). Jika file key hilang dan tidak ada backup, satu-satunya cara untuk masuk ke server adalah melalui **OCI Cloud Shell** atau console **Serial Console** di OCI Dashboard — dan itu cukup merepotkan.

## Ringkasan Posisi Keamanan

Setelah modul ini selesai:

- Port 22 tidak terekspos ke internet publik.
- Akses SSH hanya bisa dari perangkat yang terdaftar di jaringan Tailscale kamu.
- Bot scanner tidak bisa menemukan layanan yang berjalan di server kamu.
- IP publik instance masih ada (dibutuhkan Tailscale untuk handshake awal), tapi tidak ada port yang terbuka untuk umum.

Lanjut ke [Modul 7](07_PREPARE_ENV.md) untuk instalasi Docker dan menyiapkan environment sebelum deploy agent.
