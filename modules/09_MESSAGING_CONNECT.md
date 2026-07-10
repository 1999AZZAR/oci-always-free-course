# Modul 9: Integrasi Telegram & WhatsApp

Agent kamu sudah berjalan di server. Sekarang kita sambungkan ke messaging app agar bisa diakses dari mana saja — cukup kirim pesan di Telegram atau WhatsApp.

## Opsi A: Telegram Bot

Telegram adalah pilihan paling mudah karena punya API bot yang stabil dan tidak perlu scanning QR.

### Langkah 1 — Buat Bot Telegram

1. Buka Telegram, cari `@BotFather`.
2. Kirim perintah `/newbot`.
3. Beri nama bot (nama tampil, boleh ada spasi) dan username (harus diakhiri `bot`, tanpa spasi).
4. BotFather akan memberikan **API Token** — formatnya seperti `7234567890:AAFxxxxxxxxxxxxxxxxx`. Simpan ini.

### Langkah 2 — Dapatkan User ID Kamu

Cari `@userinfobot` di Telegram dan kirim pesan apapun. Bot itu akan membalas dengan User ID numerik kamu — misal `987654321`.

User ID ini digunakan untuk membatasi akses agent hanya ke akunmu sendiri.

### Langkah 3 — Konfigurasi di OpenClaw

Edit file `.env` di server:
```bash
cd ~/openclaw
nano .env
```

Tambahkan atau ubah baris berikut:
```
TELEGRAM_BOT_TOKEN=7234567890:AAFxxxxxxxxxxxxxxxxx
TELEGRAM_ALLOWED_USERS=987654321
```

Untuk mengizinkan lebih dari satu user:
```
TELEGRAM_ALLOWED_USERS=987654321,111222333
```

Simpan, lalu restart agent:
```bash
pm2 restart openclaw
```

### Langkah 4 — Test

Buka Telegram, cari bot kamu berdasarkan username-nya, dan kirim `/start`. Jika berhasil, bot akan merespons.

---

## Opsi B: WhatsApp

WhatsApp lebih rumit karena menggunakan QR scan dan tidak punya API resmi. Sebagian besar library WhatsApp untuk Node.js menggunakan pendekatan reverse-engineering (Baileys, whatsapp-web.js). Ini berarti:

- Risiko akun WhatsApp diblokir jika terdeteksi sebagai bot oleh WhatsApp.
- Perlu scanning ulang QR setiap beberapa minggu sekali.
- Library kadang putus saat WhatsApp update protokolnya.

Untuk pemakaian pribadi dengan traffic rendah, risikonya terbilang kecil. Tapi jika keandalan penting, gunakan Telegram.

### Proses Setup WhatsApp

1. Pastikan fitur WhatsApp sudah diaktifkan di `.env` OpenClaw kamu.
2. Buka tmux session agar bisa melihat output:
   ```bash
   tmux attach -t main
   ```
3. Jalankan agent dan lihat log:
   ```bash
   pm2 logs openclaw
   ```
4. Di log, akan muncul QR code berupa karakter ASCII. Scan menggunakan menu **Linked Devices** di aplikasi WhatsApp di HP kamu (Settings → Linked Devices → Link a Device).
5. Setelah terscan, log akan menampilkan pesan "authenticated" atau sejenisnya.

---

## Mengamankan Akses Agent

Pastikan agent hanya merespons pesan dari user yang kamu izinkan. Jangan pernah deploy agent tanpa pembatasan user, terutama jika menggunakan API key berbayar — bot publik yang terbuka bisa menguras kredit kamu dalam hitungan menit jika ditemukan orang lain.

Untuk Telegram, parameter `ALLOWED_USERS` sudah menangani ini. Verifikasi perilakunya dengan mengirim pesan dari akun Telegram lain dan pastikan bot tidak merespons.

---

## Setup Selesai

Ini adalah checkpoint terakhir. Konfigurasi yang sudah berjalan:

- Instance ARM di OCI dengan uptime 24/7.
- Firewall berlapis — Security List OCI + iptables OS.
- Akses SSH hanya via Tailscale, tidak ada port publik yang terbuka.
- Agent berjalan di PM2, auto-restart setelah crash atau reboot.
- Terhubung ke Telegram (atau WhatsApp) untuk kontrol via chat.

Untuk monitoring jangka panjang, jalankan `htop` sesekali untuk memantau konsumsi CPU dan RAM, dan `ncdu ~` untuk cek penggunaan disk.
