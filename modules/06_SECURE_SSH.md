# Modul 6: SSH Aman & Akses Tanpa IP Publik

Setelah Tailscale terinstall (Modul 5), saatnya kita mengamankan akses pintu belakang server.

## 1. Merapikan Private Key
Simpan file `.key` yang kamu download tadi. Di Linux/Mac, kamu harus mengatur permission-nya:
```bash
chmod 400 my_oracle_key.key
```

## 2. SSH via Tailscale
Sekarang kamu bisa login menggunakan IP Tailscale. Kenapa? Karena IP ini private dan tidak terekspos ke scanner bot di internet.
```bash
ssh -i my_oracle_key.key ubuntu@100.x.x.x
```

## 3. Mematikan SSH Port di Publik (Maksimal Keamanan)
Jika kamu sudah yakin Tailscale berjalan lancar:
1. Masuk ke **OCI Console** -> **Networking** -> **Security Lists**.
2. **Hapus** aturan Ingress Rule untuk Port 22.
3. Sekarang, server kamu tidak bisa di-scan dari internet, tapi kamu tetap bisa masuk via Tailscale. **Ini adalah Gold Standard keamanan.**

## 4. Konfigurasi ~/.ssh/config (Tips Pro)
Biar nggak capek ngetik command panjang, buat file config di laptop kamu:
```bash
nano ~/.ssh/config
```
Isi dengan:
```text
Host oci-agent
    HostName 100.x.x.x (Ganti dengan IP Tailscale)
    User ubuntu
    IdentityFile ~/path/to/my_oracle_key.key
```
Sekarang kamu cukup ngetik: `ssh oci-agent` untuk login. Gampang kan?
