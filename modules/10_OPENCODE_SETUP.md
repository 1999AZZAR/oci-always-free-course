# Modul 10: OpenCode — AI Coding Agent di Terminal

Modul ini menginstall OpenCode, AI coding agent open-source yang bisa langsung dipakai setelah instalasi — tanpa login, tanpa API key, tanpa biaya.

## Apa itu OpenCode?

OpenCode adalah AI coding agent berbasis terminal (TUI) yang:

- **Open source & gratis** — 160K+ GitHub stars, 7.5M+ developer bulanan.
- **Free models included** — sudah termasuk model gratis bawaan, siap pakai setelah install tanpa perlu registrasi.
- **75+ provider AI** — OpenAI, Anthropic Claude, Google Gemini, Groq, DeepSeek, GitHub Copilot, dan lainnya.
- **LSP-aware** — otomatis memahami struktur kode di project.
- **Multi-session** — bisa jalankan beberapa agent paralel.
- **Plan & Build mode** — review dulu sebelum eksekusi.

## 1. Install OpenCode

```bash
curl -fsSL https://opencode.ai/install | bash
source ~/.bashrc
```

Verifikasi:

```bash
opencode --version
```

## 2. Langsung Pakai (Tanpa Setup)

Setelah install, OpenCode sudah siap pakai dengan free model bawaan. Tidak perlu `/connect`, tidak perlu API key, tidak perlu login ke provider manapun.

```bash
# Masuk ke direktori project, lalu jalankan
cd ~
opencode
```

Mulai sesi coding langsung:

```
Buatkan file hello.py yang mencetak "Hello from OCI!" dan jalankan.
```

OpenCode akan menulis kode, menjalankannya, dan menampilkan output — semua tanpa konfigurasi apapun.

## 3. Ganti Model (Opsional)

Jika kamu ingin mencoba model lain, ketik `/models` di TUI untuk melihat daftar model yang tersedia. Pilih salah satu yang kamu suka.

Untuk menambahkan provider tambahan (misalnya pakai API key sendiri), ketik `/connect` dan ikuti petunjuknya.

## 4. Inisialisasi Project

Agar OpenCode memahami struktur project kamu, inisialisasi:

```
/init
```

OpenCode akan membuat file `AGENTS.md` yang berisi konteks untuk sesi coding selanjutnya.

## 5. Cara Pakai Dasar

### Mode TUI (Interaktif)

```bash
cd ~/project-anda
opencode
```

Navigasi:
- **Tab** — toggle **Plan mode** (rencana tanpa eksekusi) dan **Build mode** (eksekusi langsung)
- **`@`** — cari file di project dengan fuzzy search
- **`/undo`** — batalkan perubahan terakhir
- **`/redo`** — terapkan kembali perubahan
- **`/connect`** — tambah provider (opsional)
- **`/models`** — ganti model

### Mode CLI (Non-Interaktif)

```bash
opencode "Jelaskan struktur direktori ini"
opencode "Buatkan docker-compose.yml untuk PostgreSQL + Redis"
opencode "Fix error ini: $(cat error.log)"
```

## 6. Contoh Sesi

```bash
opencode "Cari penyebab error 'EADDRINUSE' di file server.js dan perbaiki"
```

OpenCode akan:
1. Membaca `server.js`
2. Menganalisis penyebab port conflict
3. Menawarkan solusi (Plan mode)
4. Setelah disetujui, menulis perubahan (Build mode)
5. Cek perubahan dengan `git diff` sebelum commit

## 7. Verifikasi — Audit Seluruh Setup dengan OpenCode

Inilah tes paling praktis: minta OpenCode untuk membaca repo kursus ini dan memverifikasi bahwa semua konfigurasi dari Modul 1 sampai 9 sudah benar di server kamu.

```bash
# Clone repo kursus ini (atau sudah ada)
cd ~
git clone https://github.com/1999AZZAR/oci-always-free-course.git
cd oci-always-free-course

# Minta OpenCode untuk audit
opencode
```

Kirim prompt berikut di TUI:

```
Baca semua file modul dari modules/01 sampai modules/09.
Lalu audit server ini:
1. Cek OCI instance — arsitektur dan OS (uname -a, lscpu)
2. Cek Tailscale status (tailscale status)
3. Cek SSH config (~/.ssh/config)
4. Cek Docker dan Node.js versi
5. Cek Fail2ban status dan jail yang aktif
6. Cek iptables rules
7. Cek apakah ada service berjalan di port publik (ss -tlnp)

Buat ringkasan: apa yang sudah sesuai, apa yang masih kurang, dan saran perbaikannya.
```

OpenCode akan mengeksekusi perintah-perintah audit, membaca output, dan membandingkannya dengan panduan di modul. Hasilnya adalah laporan lengkap status server kamu.

Ini membuktikan dua hal sekaligus:
- OpenCode berfungsi penuh di server OCI
- Seluruh setup dari awal course sudah terverifikasi

---

## Ringkasan

- **Install lalu pakai** — free model bawaan, tanpa registrasi, tanpa API key.
- **Biaya $0** — OpenCode open source, model gratis sudah termasuk.
- **Bisa upgrade kapan saja** — tambah provider via `/connect` jika butuh model lebih besar.

Lanjut ke [Modul 11](11_TERMINAL_ENHANCEMENT.md) untuk upgrade terminal dengan Oh My Bash, RTK, alias-hub, eza, fastfetch — semuanya bisa diinstall via OpenCode.
