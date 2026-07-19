# Modul 8: Instalasi & Konfigurasi Fail2ban

Modul ini menginstall Fail2ban dan mengkonfigurasinya untuk melindungi SSH dan service lain dari serangan brute force.

## Prasyarat

Pastikan hal-hal berikut sudah tersedia di server:
- Server OCI sudah berjalan dan bisa diakses via SSH
- sudo access (user kamu harus punya hak sudo)
- Service SSH berjalan di port default (22) atau port kustom

## 1. Install Fail2ban

```bash
sudo apt update
sudo apt install fail2ban -y
```

Setelah instalasi selesai, Fail2ban akan otomatis berjalan sebagai service systemd. Cek statusnya:

```bash
sudo systemctl status fail2ban
```

Kamu akan melihat status `active (running)`.

## 2. Konfigurasi Dasar

Fail2ban menggunakan file konfigurasi di `/etc/fail2ban/`. Jangan edit file `jail.conf` langsung — buat file lokal override:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

Cari dan sesuaikan parameter berikut:

```ini
[DEFAULT]
# Lama waktu bann (dalam detik). 1 jam = 3600
bantime = 1h

# Jumlah percobaan gagal sebelum kena ban
maxretry = 5

# Jendela waktu (dalam detik) untuk menghitung maxretry
findtime = 10m

# Ignore alamat IP tertentu (contoh: IP Tailscale kamu agar tidak ikut keban)
ignoreip = 127.0.0.1/8 ::1 100.x.x.x
```

Ganti `100.x.x.x` dengan IP Tailscale server kamu atau IP pribadi yang tidak ingin di-ban.

Pastikan jail SSH aktif. Cari bagian `[sshd]`:

```ini
[sshd]
enabled = true
port    = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s
```

Simpan dengan `Ctrl+O`, Enter, lalu keluar dengan `Ctrl+X`.

## 3. Restart & Verifikasi

Setelah mengubah konfigurasi, restart Fail2ban:

```bash
sudo systemctl restart fail2ban
```

Cek status jail yang aktif:

```bash
sudo fail2ban-client status
```

Kamu akan melihat daftar jail termasuk `sshd`. Cek detail jail SSH:

```bash
sudo fail2ban-client status sshd
```

Output akan menampilkan jumlah banned IP, filter yang digunakan, dan daftar IP yang sedang di-ban.

## 4. Test Konfigurasi

Kamu bisa simulasi serangan dari server lain (atau dari localhost sendiri) untuk memastikan Fail2ban bekerja:

```bash
# Dari terminal lain, coba SSH dengan password salah beberapa kali
ssh nonexistent@<ip-server>
```

Setelah 5 kali gagal (sesuai `maxretry`), cek status jail:

```bash
sudo fail2ban-client status sshd
```

IP pengirim harus muncul di daftar banned IP.

## 5. Unban Manual

Jika kamu sendiri ke-ban (misalnya karena salah konfigurasi `ignoreip`), jangan panik. Buka session dari IP lain atau console OCI, lalu:

```bash
sudo fail2ban-client set sshd unbanip <ip-yang-keban>
```

## 6. Konfigurasi untuk Port SSH Kustom

Jika kamu mengubah port SSH dari 22 ke port lain (misal 2222), edit `/etc/fail2ban/jail.local` dan ubah parameter `port`:

```ini
[sshd]
enabled = true
port    = 2222
```

Kemudian restart:

```bash
sudo systemctl restart fail2ban
```

## 7. Auto-start saat Reboot

Fail2ban sudah otomatis terdaftar sebagai systemd service dan akan aktif setiap kali server restart. Verifikasi:

```bash
sudo systemctl is-enabled fail2ban
```

Output harus `enabled`.

## Pemantauan Log

Log Fail2ban ada di `/var/log/fail2ban.log`. Kamu bisa memantaunya dengan:

```bash
sudo tail -f /var/log/fail2ban.log
```

Untuk melihat ringkasan semua ban yang pernah terjadi:

```bash
sudo zgrep 'Ban' /var/log/fail2ban.log*
```

---

Lanjut ke [Modul 9](09_MESSAGING_CONNECT.md) untuk mengirim notifikasi ke Telegram jika ada IP yang di-ban.
