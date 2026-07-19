# Modul 9: Notifikasi Fail2ban via Telegram

Fail2ban sudah melindungi server dari brute force. Sekarang kita sambungkan ke Telegram agar kamu mendapat notifikasi realtime setiap kali ada IP yang di-ban.

## Buat Bot Telegram

1. Buka Telegram, cari `@BotFather`.
2. Kirim perintah `/newbot`.
3. Beri nama bot (misal `Server Alert`) dan username (harus diakhiri `bot`, tanpa spasi).
4. BotFather akan memberikan **API Token** — formatnya seperti `7234567890:AAFxxxxxxxxxxxxxxxxx`. Simpan ini.

## Dapatkan Chat ID Kamu

Cari `@userinfobot` di Telegram dan kirim pesan apapun. Bot itu akan membalas dengan User ID numerik kamu — misal `987654321`. Catat angka ini.

## Install Script Notifikasi

Kita buat script sederhana yang membaca event ban dari log Fail2ban dan mengirim notifikasi ke Telegram.

```bash
sudo nano /usr/local/bin/fail2ban-telegram.sh
```

Isi dengan:

```bash
#!/bin/bash
TOKEN="7234567890:AAFxxxxxxxxxxxxxxxxx"
CHAT_ID="987654321"

IP="$1"
JAIL="$2"

MESSAGE="⚠️ Fail2ban Alert
IP: $IP
Jail: $JAIL
Time: $(date '+%Y-%m-%d %H:%M:%S')"

curl -s -X POST "https://api.telegram.org/bot$TOKEN/sendMessage" \
  -d "chat_id=$CHAT_ID" \
  -d "text=$MESSAGE" \
  -d "parse_mode=Markdown" > /dev/null
```

Sesuaikan `TOKEN` dan `CHAT_ID` dengan milik kamu. Simpan lalu buat executable:

```bash
sudo chmod +x /usr/local/bin/fail2ban-telegram.sh
```

## Hubungkan ke Fail2ban

Fail2ban bisa menjalankan aksi kustom saat terjadi ban. Buat file aksi:

```bash
sudo nano /etc/fail2ban/action.d/telegram.conf
```

Isi dengan:

```ini
[Definition]
actionban = /usr/local/bin/fail2ban-telegram.sh <ip> <name>
```

Simpan, lalu tambahkan aksi ini ke jail SSH. Edit `/etc/fail2ban/jail.local`:

```bash
sudo nano /etc/fail2ban/jail.local
```

Cari bagian `[sshd]` dan tambahkan:

```ini
[sshd]
enabled    = true
port       = ssh
logpath    = %(sshd_log)s
backend    = %(sshd_backend)s
action     = %(action_)s
             telegram
```

Simpan, lalu restart Fail2ban:

```bash
sudo systemctl restart fail2ban
```

## Test Notifikasi

Simulasi serangan dengan 5+ percobaan SSH gagal dari IP lain. Dalam beberapa detik, kamu akan menerima notifikasi Telegram:

```
⚠️ Fail2ban Alert
IP: 192.168.1.100
Jail: sshd
Time: 2026-07-20 14:30:00
```

## Verifikasi

Cek status Fail2ban dan pastikan jail berjalan:

```bash
sudo fail2ban-client status sshd
```

Cek log untuk melihat aksi yang dijalankan:

```bash
sudo tail -f /var/log/fail2ban.log
```

---

## Ringkasan Konfigurasi

Semua komponen yang sudah terpasang:

- Instance ARM di OCI dengan uptime 24/7.
- Firewall berlapis — Security List OCI + iptables OS.
- Akses SSH hanya via Tailscale, tidak ada port publik yang terbuka.
- Fail2ban melindungi SSH dari brute force.
- Notifikasi Telegram realtime jika ada serangan.

Untuk monitoring jangka panjang, jalankan `htop` sesekali untuk memantau konsumsi CPU dan RAM, dan `ncdu ~` untuk cek penggunaan disk.
