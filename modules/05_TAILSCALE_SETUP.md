# Modul 5: Setup Tailscale (Mesh VPN Private)

Agar server kamu aman dari serangan bruteforce SSH dan bisa diakses seperti jaringan lokal, kita akan menggunakan **Tailscale**.

## Apa itu Tailscale?
Tailscale membuat jaringan VPN private (Overlay) di atas koneksi internet biasa. Server OCI kamu akan mendapatkan IP internal (misal: `100.x.x.x`) yang hanya bisa diakses oleh perangkat yang kamu izinkan.

## Langkah Instalasi di Server OCI
Setelah berhasil SSH ke server untuk pertama kali, jalankan perintah berikut:

1. **Install Tailscale**:
   ```bash
   curl -fsSL https://tailscale.com/install.sh | sh
   ```

2. **Login & Hubungkan**:
   ```bash
   sudo tailscale up
   ```
   - Klik link yang muncul di terminal untuk autentikasi dengan akun Tailscale kamu (bisa pakai Google/GitHub).

3. **Verifikasi**:
   ```bash
   tailscale ip -4
   ```
   Catat IP tersebut. Sekarang kamu bisa SSH ke server menggunakan IP Tailscale ini dari laptop kamu (yang juga sudah terinstall Tailscale).

## Keuntungan Utama
- Kamu bisa **menutup port 22 (SSH)** di Security List OCI (Ingress Rules).
- Server tetap bisa diakses meskipun IP Publik OCI berubah atau tanpa IP Publik sekalipun.
- Akses dashboard OpenClaw secara private tanpa mengekspos port web ke publik.
