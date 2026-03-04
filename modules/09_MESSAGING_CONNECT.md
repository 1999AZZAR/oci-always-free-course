# Modul 9: Menghubungkan ke Messaging (Telegram/WhatsApp)

Agent kamu sudah jalan, tapi gimana cara ngobrol sama dia? Kita akan hubungkan ke Telegram atau WhatsApp.

## 1. Menyiapkan Bot Telegram
1. Cari **@BotFather** di Telegram.
2. Ketik `/newbot`, beri nama, dan dapatkan **API Token**.
3. Cari **@userinfobot** untuk mendapatkan **User ID** kamu (agar cuma kamu yang bisa akses agent).

## 2. Menghubungkan ke OpenClaw
Edit file `.env` di folder OpenClaw kamu:
```bash
nano .env
```
Isi bagian Telegram:
```text
TELEGRAM_BOT_TOKEN=your_token_here
ALLOWED_USERS=your_user_id_here
```
Restart agent kamu:
```bash
pm2 restart openclaw-agent
```

## 3. Menyiapkan WhatsApp (Opsi Lain)
WhatsApp lebih *tricky* karena butuh scanning QR code.
1. Pastikan fitur WhatsApp di `.env` sudah ON.
2. Jalankan log pm2: `pm2 logs openclaw-agent`.
3. Akan muncul QR Code di terminal. Scan menggunakan menu "Linked Devices" di aplikasi WhatsApp kamu.

## Kesimpulan
Sekarang kamu punya AI Agent pribadi yang berjalan di server tangguh (OCI), terkoneksi aman (Tailscale), dan bisa kamu perintah lewat chat kapan saja. 

**Selamat! Kamu resmi jadi pemilik server cloud tanpa keluar biaya sepeser pun.**
