# Modul 8: Instalasi OpenClaw Agent

Modul ini menginstall OpenClaw dan mengkonfigurasinya agar berjalan sebagai service yang otomatis start setelah reboot.

## Prasyarat

Pastikan hal-hal berikut sudah tersedia di server:
- Node.js terinstall via NVM (`node -v` harus menampilkan versi)
- PM2 terinstall (`pm2 -v` harus merespons)
- Git terinstall (`git --version` harus merespons)
- API key dari provider AI yang ingin kamu pakai (OpenAI, Gemini, atau Anthropic)

## 1. Clone Repository

```bash
cd ~
git clone https://github.com/openclaw/openclaw.git
cd openclaw
```

## 2. Install Dependencies

```bash
npm install
```

Proses ini bisa memakan beberapa menit tergantung jumlah dependency. Jika ada warning tentang `fsevents` atau modul lain yang tidak kompatibel dengan ARM, itu normal — module tersebut biasanya opsional dan hanya relevan untuk macOS.

## 3. Konfigurasi

Salin file environment contoh dan edit sesuai kebutuhan:
```bash
cp .env.example .env
nano .env
```

Isi setidaknya nilai-nilai berikut:

```
# Pilih salah satu atau beberapa provider AI
OPENAI_API_KEY=sk-...
GEMINI_API_KEY=AI...
ANTHROPIC_API_KEY=sk-ant-...

# Port yang dipakai agent (default biasanya 3000)
PORT=3000
```

Simpan dengan `Ctrl+O`, Enter, lalu keluar dengan `Ctrl+X`.

Jangan commit file `.env` ke Git. Pastikan `.env` sudah ada di `.gitignore` — kalau belum, tambahkan:
```bash
echo ".env" >> .gitignore
```

## 4. Test Jalankan

Sebelum didaftarkan ke PM2, coba jalankan manual dulu untuk memastikan tidak ada error:
```bash
node index.js
```

Jika ada error `Missing API key` atau `Module not found`, perbaiki dulu sebelum lanjut. Tekan `Ctrl+C` untuk keluar setelah tes berhasil.

## 5. Jalankan via PM2

```bash
pm2 start index.js --name "openclaw"
pm2 save
```

`pm2 save` menyimpan daftar proses yang berjalan ke disk, sehingga PM2 tahu apa yang harus di-restart setelah reboot.

## 6. Verifikasi

Cek status proses:
```bash
pm2 status
```

Kamu harus melihat baris `openclaw` dengan status `online`.

Lihat log realtime:
```bash
pm2 logs openclaw
```

Tekan `Ctrl+C` untuk keluar dari mode log.

Restart manual jika butuh:
```bash
pm2 restart openclaw
```

## Update di Kemudian Hari

Untuk update ke versi terbaru:
```bash
cd ~/openclaw
git pull
npm install
pm2 restart openclaw
```

## Akses dari Browser (Opsional)

Jika agent kamu memiliki web interface, kamu bisa mengaksesnya melalui Tailscale tanpa membuka port ke publik:

```bash
# Dari laptop kamu yang sudah terhubung Tailscale
# Buka browser dan akses: http://100.x.x.x:3000
```

Ganti `100.x.x.x` dengan IP Tailscale server kamu, dan `3000` dengan port yang dikonfigurasi di `.env`.

Lanjut ke [Modul 9](09_MESSAGING_CONNECT.md) untuk menghubungkan agent ke Telegram atau WhatsApp.
