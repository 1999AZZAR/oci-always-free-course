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

### Cara Membuka Port di iptables

Sambungkan via SSH, lalu jalankan:

```bash
# Lihat aturan yang aktif saat ini
sudo iptables -L INPUT --line-numbers

# Tambahkan aturan untuk port yang ingin dibuka (ganti 80 dengan port kamu)
sudo iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT

# Simpan agar aturan tetap ada setelah reboot
sudo iptables-save | sudo tee /etc/iptables/rules.v4 > /dev/null
```

Untuk port 443:
```bash
sudo iptables -I INPUT 1 -p tcp --dport 443 -j ACCEPT
sudo iptables-save | sudo tee /etc/iptables/rules.v4 > /dev/null
```

### Verifikasi

Setelah menambahkan aturan, pastikan aturan terbaca:
```bash
sudo iptables -L INPUT --line-numbers | head -20
```

Kamu seharusnya melihat baris `ACCEPT` untuk port yang baru kamu tambahkan.

## Catatan Penting tentang iSCSI

OCI menggunakan iSCSI untuk menghubungkan boot volume ke instance. Ada beberapa aturan iptables bawaan yang menjaga koneksi ini. **Jangan pernah menjalankan `sudo iptables -F`** (flush semua aturan) — ini bisa memutus koneksi iSCSI dan membuat instance tidak bisa diakses, bahkan tidak bisa boot ulang.

Selalu tambahkan aturan secara spesifik per port, bukan flush semua.

## Lanjutan

Setelah firewall dikonfigurasi, lanjut ke [Modul 5](05_TAILSCALE_SETUP.md) untuk setup Tailscale. Setelah Tailscale aktif, kamu bisa menutup port 22 dari Security List dan hanya mengakses instance melalui jaringan private Tailscale.
