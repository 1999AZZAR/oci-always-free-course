# Modul 4: Konfigurasi VCN & Firewall

Traffic ke instance OCI kamu melewati **dua lapis firewall yang independen**: satu di level jaringan OCI (Security List), satu lagi di level OS Ubuntu (iptables). Keduanya harus dibuka — tidak cukup salah satu.

Ini sumber frustrasi paling umum: port sudah dibuka di OCI Console tapi aplikasi tetap tidak bisa diakses dari luar. Jawabannya hampir selalu di iptables level OS.

## Lapisan 1: OCI Security List

Security List adalah firewall di level subnet OCI. Semua instance dalam subnet tersebut tunduk pada aturan ini.

### Membuka Port di Security List

1. Dari OCI Console, pergi ke **Networking** → **Virtual Cloud Networks**.
2. Klik VCN kamu → klik **Subnets** di panel kiri → klik subnet public kamu.
3. Di bagian **Security Lists**, klik **Default Security List for [nama-vcn]**.
4. Klik **Add Ingress Rules**.
5. Isi field berikut:
   - **Source Type**: CIDR
   - **Source CIDR**: `0.0.0.0/0` (akses dari mana saja, untuk port publik seperti 80/443)
   - **IP Protocol**: TCP
   - **Destination Port Range**: port yang ingin dibuka (misal `80` atau `443` atau `8080`)
6. Klik **Add Ingress Rules**.

Ulangi untuk setiap port yang dibutuhkan. Port 22 (SSH) biasanya sudah terbuka secara default.

### Port yang Umum Dibutuhkan

| Port | Protokol | Kegunaan |
|---|---|---|
| 22 | TCP | SSH (sudah terbuka default) |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 41641 | UDP | Tailscale (direct peer connection) |

## Lapisan 2: iptables di Ubuntu

OCI Ubuntu image datang dengan aturan iptables bawaan yang cukup ketat. Bahkan setelah kamu buka port di Security List, iptables di OS bisa masih memblokir traffic tersebut.

> **Jangan gunakan `ufw`** di instance OCI. `ufw` bisa bertabrakan dengan aturan iptables bawaan Oracle dan berpotensi memutus koneksi iSCSI yang digunakan untuk boot volume — bisa bikin instance tidak bisa boot.

### Persiapan — Install iptables-persistent

Agar aturan iptables tetap ada setelah reboot, install package penyimpan aturan:

```bash
sudo apt update
sudo apt install -y iptables-persistent
```

Saat instalasi, akan muncul dialog untuk menyimpan aturan IPv4 dan IPv6 saat ini. Pilih **Yes** untuk keduanya.

### Cara Membuka Port di iptables

Sambungkan via SSH, lalu jalankan:

```bash
# Lihat aturan yang aktif saat ini
sudo iptables -L INPUT --line-numbers
```

Perhatikan nomor baris aturan yang sudah ada. Tambahkan aturan baru di posisi pertama:

```bash
# Tambahkan aturan untuk port 80 (HTTP)
sudo iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT

# Tambahkan aturan untuk port 443 (HTTPS)
sudo iptables -I INPUT 1 -p tcp --dport 443 -j ACCEPT

# Tambahkan aturan untuk port Tailscale (UDP)
sudo iptables -I INPUT 1 -p udp --dport 41641 -j ACCEPT
```

**Simpan aturan** agar tetap ada setelah reboot:

```bash
sudo netfilter-persistent save
```

### Verifikasi

Cek apakah aturan sudah terbaca:

```bash
sudo iptables -L INPUT --line-numbers | head -20
```

Output akan terlihat seperti ini (nomor baris bisa berbeda):

```
Chain INPUT (policy ACCEPT)
num  target     prot opt source       destination
1    ACCEPT     udp  --  anywhere     anywhere     udp dpt:41641
2    ACCEPT     tcp  --  anywhere     anywhere     tcp dpt:443
3    ACCEPT     tcp  --  anywhere     anywhere     tcp dpt:80
4    ACCEPT     all  --  anywhere     anywhere     ...
...
```

Pastikan baris `ACCEPT` untuk port yang kamu buka muncul di daftar.

## Catatan Penting tentang iSCSI

OCI menggunakan iSCSI untuk menghubungkan boot volume ke instance. Ada beberapa aturan iptables bawaan yang menjaga koneksi ini. **Jangan pernah menjalankan `sudo iptables -F`** (flush semua aturan) — ini bisa memutus koneksi iSCSI dan membuat instance tidak bisa diakses, bahkan tidak bisa boot ulang.

Selalu tambahkan aturan secara spesifik per port, bukan flush semua.

## Ringkasan — Dua Lapis Firewall

| Lapisan | Lokasi | Cara Atur |
|---|---|---|
| Security List | OCI Console | Tambah Ingress Rule per port |
| iptables | Dalam OS (SSH) | `sudo iptables -I INPUT ...` lalu `sudo netfilter-persistent save` |

Keduanya harus dibuka agar port bisa diakses dari luar.

## Lanjutan

Setelah firewall dikonfigurasi, lanjut ke [Modul 5](05_TAILSCALE_SETUP.md) untuk setup Tailscale. Setelah Tailscale aktif, kamu bisa menutup port 22 dari Security List dan hanya mengakses instance melalui jaringan private Tailscale.
