# Modul 10: OpenCode — AI Coding Agent di Terminal

Modul ini menginstall OpenCode, AI coding agent open-source yang bisa kamu gunakan langsung dari terminal server OCI — gratis, tanpa perlu resource lokal untuk LLM.

## Apa itu OpenCode?

OpenCode adalah AI coding agent berbasis terminal (TUI) yang:

- **Open source & gratis** — 160K+ GitHub stars, 7.5M+ developer bulanan.
- **Bisa gratis total** — OpenCode Zen menyediakan model gratis (DeepSeek V4 Flash Free, Big Pickle, Nemotron 3 Ultra Free, dan lainnya) tanpa biaya per token.
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

## 2. Konfigurasi Model AI

### Opsi A: OpenCode Zen — Model Gratis (Recommended)

OpenCode Zen menyediakan model gratis yang bisa langsung dipakai tanpa biaya, tanpa perlu API key provider lain, dan tanpa membebani RAM/CPU server OCI.

Model gratis yang tersedia:

| Model | Tipe | Biaya |
|---|---|---|
| DeepSeek V4 Flash Free | Coding | Gratis |
| Big Pickle | Stealth model | Gratis |
| Nemotron 3 Ultra Free | General | Gratis |
| MiMo-V2.5 Free | General | Gratis |
| North Mini Code Free | Coding | Gratis |

Setup:

1. Jalankan OpenCode:
```bash
opencode
```

2. Di dalam TUI, ketik:
```
/connect
```

3. Pilih **OpenCode Zen**. Browser akan terbuka — login dengan GitHub atau Google.

4. Setelah login, salin API key yang diberikan dan tempel di terminal.

5. Ketik `/models` dan pilih model gratis seperti `DeepSeek V4 Flash Free` atau `Big Pickle`.

> **Kenapa ini yang direkomendasikan?** Server OCI Always Free punya resource terbatas (2–4 OCPU, 12–24 GB RAM) yang lebih baik dialokasikan untuk aplikasi dan service, bukan untuk menjalankan LLM lokal. Dengan Zen, semua komputasi AI terjadi di cloud — server OCI tetap ringan.

### Opsi B: Free API Tier — Google Gemini / Groq / NVIDIA

Provider berikut menawarkan API gratis tanpa kartu kredit:

- **Google Gemini** — daftar di [Google AI Studio](https://makersuite.google.com/app/apikey), buat API key, lalu `/connect` → Gemini.
- **Groq** — daftar di [Groq Console](https://console.groq.com), buat API key, lalu `/connect` → Groq. Kecepatan inference sangat tinggi.
- **NVIDIA** — daftar di [build.nvidia.com](https://build.nvidia.com), buat API key, lalu `/connect` → NVIDIA.

### Opsi C: Langganan yang Sudah Ada

OpenCode bisa menggunakan subscription yang mungkin sudah kamu miliki:

- **ChatGPT Plus/Pro** — `/connect` → OpenAI → ChatGPT Plus/Pro (login via browser)
- **GitHub Copilot** — `/connect` → GitHub Copilot (login via GitHub)
- **Claude Pro/Max** — API key Anthropic via `/connect`

## 3. Inisialisasi Project

Masuk ke direktori project dan init OpenCode:

```bash
cd ~/project-anda
opencode
```

Di dalam TUI, ketik:

```
/init
```

OpenCode akan menganalisis struktur project dan membuat file `AGENTS.md` yang berisi konteks untuk sesi coding selanjutnya.

## 4. Cara Pakai Dasar

### Mode TUI (Interaktif)

```bash
opencode
```

Navigasi di dalam TUI:
- **Tab** — toggle antara **Plan mode** (hanya rencana, tanpa eksekusi) dan **Build mode** (eksekusi langsung)
- **`@`** — cari file di project dengan fuzzy search
- **`/undo`** — batalkan perubahan terakhir
- **`/redo`** — terapkan kembali perubahan
- **`/connect`** — kelola API key dan provider
- **`/models`** — ganti model AI

### Mode CLI (Non-Interaktif)

Jalankan task langsung dari shell:

```bash
opencode "Jelaskan struktur file di direktori ini"
opencode "Buatkan docker-compose.yml untuk PostgreSQL + Redis"
opencode "Fix error ini: $(cat error.log)"
```

## 5. Contoh Sesi — Fix Error dengan OpenCode

```bash
opencode "Cari penyebab error 'EADDRINUSE' di file server.js dan perbaiki"
```

OpenCode akan:
1. Membaca file `server.js`
2. Menganalisis kode yang menyebabkan port conflict
3. Menawarkan solusi (Plan mode)
4. Setelah kamu setuju, menulis perubahan (Build mode)
5. Kamu bisa cek perubahan dengan `git diff` sebelum commit

---

## Ringkasan

- **OpenCode gratis dan open source** — tidak ada biaya lisensi.
- **Zen free models** — langsung pakai, tanpa API key, tanpa beban di server OCI.
- **Fungsionalitas penuh di terminal** — tidak perlu desktop app atau IDE extension.

Lanjut ke [Modul 11](11_TERMINAL_ENHANCEMENT.md) untuk meng upgrade terminal kamu dengan Oh My Bash, RTK, alias-hub, eza, dan fastfetch — semuanya bisa diinstall langsung via OpenCode.
