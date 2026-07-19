# Modul 10: OpenCode — AI Coding Agent di Terminal

Modul ini menginstall OpenCode, AI coding agent open-source yang berjalan langsung dari terminal server OCI — tanpa biaya lisensi, tanpa ketergantungan ke cloud API.

## Apa itu OpenCode?

OpenCode adalah AI coding agent berbasis terminal yang:

- **Open source & gratis** — 160K+ GitHub stars, 7.5M+ developer bulanan.
- **Bisa berjalan 100% offline** — pakai Ollama dengan model lokal di server yang sama, tanpa koneksi internet, tanpa API key.
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

OpenCode butuh model AI untuk bekerja. Kamu punya dua jalur: **lokal/offline** (recommended) atau **cloud API**.

### Jalur A: 100% Offline — Ollama + Model Lokal (Recommended)

Model berjalan langsung di server OCI kamu. Setelah model didownload (sekali), **tidak perlu koneksi internet sama sekali** — tidak ada API key, tidak ada biaya per request, tidak ada data keluar dari server.

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Download model coding (cocok untuk ARM Ampere 4 OCPU / 24 GB RAM)
ollama pull qwen3-coder:3b
```

Model `qwen3-coder:3b` hanya ~1.8 GB dan berjalan lancar di ARM Ampere.

Ollama otomatis mendeteksi OpenCode dan mengkonfigurasi dirinya sendiri. Jalankan OpenCode:

```bash
opencode
```

Ketik `/models` di TUI, pilih model `qwen3-coder:3b` dari daftar.

Jika model tidak muncul secara otomatis, buat konfigurasi manual:

```bash
mkdir -p ~/.config/opencode
nano ~/.config/opencode/opencode.jsonc
```

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama (local)",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "qwen3-coder:3b": {
          "name": "Qwen3 Coder 3B (local)"
        }
      }
    }
  }
}
```

Simpan, jalankan `opencode`, ketik `/models`, dan pilih model lokal kamu.

#### Alternatif Model Lokal

| Model | Ukuran | RAM | Cocok Untuk |
|---|---|---|---|
| `qwen3-coder:3b` | ~1.8 GB | 4 GB+ | Tugas coding ringan-cepat |
| `deepseek-coder-v2` | ~4.2 GB | 8 GB+ | Coding kompleks, refactoring |
| `llama3.2:3b` | ~2.0 GB | 4 GB+ | General purpose |
| `qwen3-coder:7b` | ~4.0 GB | 8 GB+ | Coding advanced |

> Semua model di atas berjalan **lokal di server kamu**. Setelah download awal, tidak ada data yang dikirim ke luar. Cocok untuk project pribadi atau environment yang membutuhkan privasi data.

### Jalur B: Free Tier — Google Gemini / Groq / NVIDIA

Jika kamu ingin model yang lebih besar tanpa membayar, provider berikut menawarkan API gratis:

- **Google Gemini** — daftar di [Google AI Studio](https://makersuite.google.com/app/apikey), buat API key (gratis), lalu `/connect` → Gemini.
- **Groq** — daftar di [Groq Console](https://console.groq.com), buat API key, lalu `/connect` → Groq. Kecepatan inference sangat tinggi.
- **NVIDIA** — daftar di [build.nvidia.com](https://build.nvidia.com), buat API key, lalu `/connect` → NVIDIA.
- **OpenCode Zen (free models)** — Zen punya model gratis: DeepSeek V4 Flash Free, Nemotron 3 Ultra Free, Big Pickle, dan lainnya. `/connect` → OpenCode Zen.

### Jalur C: Berbayar — Langganan yang Sudah Ada

OpenCode bisa menggunakan subscription yang mungkin sudah kamu miliki:

- **ChatGPT Plus/Pro** — login via `/connect` → OpenAI → ChatGPT Plus/Pro
- **GitHub Copilot** — login via `/connect` → GitHub Copilot
- **Claude Pro/Max** — API key Anthropic via `/connect`

> **Rekomendasi**: Mulai dengan **Jalur A (Ollama lokal)**. Setelah model didownload, OpenCode siap pakai kapan saja tanpa internet, tanpa biaya, tanpa registrasi.

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
- **`/share`** — buat link sharing sesi

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
- **100% offline dengan Ollama** — model lokal, tanpa API key, tanpa koneksi internet setelah setup.
- **Fungsionalitas penuh di terminal** — tidak perlu desktop app atau IDE extension.

Lanjut ke [Modul 11](11_TERMINAL_ENHANCEMENT.md) untuk meng upgrade terminal kamu dengan Oh My Bash, RTK, alias-hub, eza, dan fastfetch — semuanya bisa diinstall langsung via OpenCode.
