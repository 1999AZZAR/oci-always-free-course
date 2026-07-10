# Modul 7: Persiapan Environment (Docker & Node.js)

Sebelum install agent, kita perlu menyiapkan fondasi: Docker untuk mengisolasi aplikasi, dan Node.js untuk menjalankan agent berbasis JavaScript. Semua command dijalankan di dalam instance OCI via SSH.

## 1. Update Sistem

Selalu mulai dengan update — Ubuntu baru diinstall biasanya punya banyak package yang sudah ada versi barunya:

```bash
sudo apt update && sudo apt upgrade -y
```

Jika ada prompt tentang restart services, tekan Enter untuk menerima default.

## 2. Install Docker

Gunakan script resmi Docker, bukan dari Ubuntu repository — versi di Ubuntu repo sering jauh tertinggal:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Setelah selesai, tambahkan user `ubuntu` ke group `docker` agar tidak perlu `sudo` setiap kali menjalankan perintah Docker:

```bash
sudo usermod -aG docker $USER
```

Kemudian reboot atau logout dan login ulang agar perubahan group berlaku:

```bash
sudo reboot
```

Tunggu sekitar 30 detik, lalu SSH ulang ke server. Verifikasi Docker berjalan dengan benar:

```bash
docker version
docker compose version
```

Docker Compose V2 sudah termasuk dalam instalasi Docker modern sebagai plugin (`docker compose`, bukan `docker-compose`). Jika kamu menemukan dokumentasi lama yang masih menggunakan `docker-compose` dengan tanda hubung, ganti saja dengan `docker compose`.

### Catatan Arsitektur ARM

Instance Ampere A1 menggunakan arsitektur `linux/arm64`. Hampir semua image Docker populer (Nginx, Node, PostgreSQL, Redis, Python) sudah mendukung ARM64 secara native. Jika image yang kamu butuhkan tidak ada versi ARM-nya, coba cari di Docker Hub dengan filter platform `linux/arm64`.

## 3. Install Node.js via NVM

NVM (Node Version Manager) memungkinkan kamu menginstall dan mengunci versi Node.js tanpa konflik dengan package system Ubuntu:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

Muat NVM ke shell session saat ini:
```bash
source ~/.bashrc
```

Install versi LTS terbaru dan jadikan default:
```bash
nvm install --lts
nvm use --lts
nvm alias default node
```

Verifikasi:
```bash
node -v
npm -v
```

## 4. Install PM2

PM2 adalah process manager yang menjaga aplikasi Node.js tetap berjalan di background, termasuk auto-restart jika crash:

```bash
npm install -g pm2
```

Konfigurasi PM2 agar auto-start setelah server reboot:
```bash
pm2 startup
```

PM2 akan mencetak sebuah perintah `sudo env PATH=...` — jalankan perintah itu persis seperti yang dicetak, lalu:
```bash
pm2 save
```

## 5. Tool Pendukung

Install tool CLI yang berguna untuk monitoring dan navigasi:

```bash
sudo apt install -y git htop tmux ncdu
```

| Tool | Kegunaan |
|---|---|
| `git` | Clone repository dan pull update |
| `htop` | Monitor CPU dan RAM secara realtime |
| `tmux` | Multiplex terminal — session tidak mati saat SSH disconnect |
| `ncdu` | Analisis penggunaan disk secara interaktif |

### Cara Pakai tmux (Singkat)

```bash
# Buat session baru
tmux new -s main

# Keluar dari session (session tetap jalan di background)
Ctrl+B, kemudian tekan D

# Masuk kembali ke session
tmux attach -t main
```

Biasakan untuk menjalankan semua pekerjaan panjang di dalam tmux session — jika koneksi SSH putus, proses di dalam tmux tetap berjalan.

Lanjut ke [Modul 8](08_INSTALL_OPENCLAW.md) untuk install OpenClaw agent.
