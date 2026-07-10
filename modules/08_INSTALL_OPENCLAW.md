# Modul 8: Instalasi OpenClaw Agent

Sekarang kita akan menginstall "Otak" utama di server OCI kamu menggunakan **OpenClaw**.

## Langkah Persiapan
1. Pastikan Node.js dan Git sudah terinstall:
   ```bash
   sudo apt update
   sudo apt install -y git nodejs npm
   ```
2. Install PM2 untuk menjaga agent tetap berjalan di background:
   ```bash
   sudo npm install -g pm2
   ```

## Langkah Instalasi OpenClaw
1. **Clone Repository**:
   ```bash
   git clone https://github.com/openclaw/openclaw.git
   cd openclaw
   ```

2. **Install Dependencies**:
   ```bash
   npm install
   ```

3. **Konfigurasi**:
   Copy file contoh environment:
   ```bash
   cp .env.example .env
   nano .env
   ```
   Isi API Key yang dibutuhkan (OpenAI/Gemini/Anthropic).

4. **Jalankan via PM2**:
   ```bash
   pm2 start index.js --name "openclaw-agent"
   pm2 save
   pm2 startup
   ```

## Verifikasi
Cek status agent:
```bash
pm2 status
pm2 logs openclaw-agent
```
Sekarang agent kamu sudah hidup di server Oracle Cloud yang tangguh (12GB/24GB RAM) dan siap menerima perintah via messaging app.
