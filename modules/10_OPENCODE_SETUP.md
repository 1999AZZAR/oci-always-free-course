# Modul 10: OpenCode — AI Coding Agent di Terminal

Modul ini menginstall OpenCode, AI coding agent open-source yang bisa kamu gunakan langsung dari terminal server OCI — tanpa biaya lisensi, tanpa batasan penggunaan.

## Apa itu OpenCode?

OpenCode adalah AI coding agent berbasis terminal (TUI) yang:

- **Open source & gratis** — kode sumber terbuka di GitHub, 160K+ stars, 7.5M+ developer bulanan.
- **75+ provider AI** — bisa pakai OpenAI, Anthropic Claude, Google Gemini, Groq, DeepSeek, GitHub Copilot, dan lainnya.
- **Bisa gratis total** — pakai model gratis dari provider seperti Gemini API (free tier), Groq (free tier), NVIDIA build.nvidia.com (gratis), atau Ollama (lokal di server).
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

## 2. Konfigurasi Provider AI

OpenCode butuh akses ke model AI. Ada beberapa opsi — pilih salah satu:

### Opsi A: Gratis — Gunakan Google Gemini (Free Tier)

Google memberikan API key gratis dengan rate limit yang cukup untuk penggunaan personal:

1. Buka [Google AI Studio](https://makersuite.google.com/app/apikey), klik **Get API Key**.
2. Buat API key (gratis, tidak perlu kartu kredit).
3. Jalankan OpenCode dan hubungkan:

```bash
opencode
```

Di dalam TUI, ketik:

```
/connect
```

Pilih **Google Gemini**, lalu masukkan API key kamu.

### Opsi B: Gratis — Gunakan Groq (Free Tier)

Groq menawarkan akses gratis ke Llama, Mixtral, Gemma, dan model open-source lain dengan kecepatan sangat tinggi:

1. Daftar di [Groq Console](https://console.groq.com), klik **Create API Key**.
2. Jalankan `opencode`, ketik `/connect`, pilih **Groq**, masukkan API key.

### Opsi C: Gratis — Gunakan Ollama (Model Lokal)

Jalankan model LLM langsung di server OCI tanpa API key sama sekali:

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Download model (pilih salah satu)
ollama pull qwen3-coder:3b    # Ringan, cocok untuk ARM 4 OCPU
ollama pull deepseek-coder-v2 # Lebih besar, butuh RAM cukup
```

Konfigurasi OpenCode untuk pakai Ollama. Buat file `~/.config/opencode/opencode.jsonc`:

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

Jalankan `opencode`, ketik `/models`, dan pilih model lokal kamu.

### Opsi D: Berbayar — Pakai Langganan yang Sudah Ada

OpenCode bisa menggunakan subscription yang mungkin sudah kamu miliki:

- **ChatGPT Plus/Pro** — login via `/connect` → OpenAI → ChatGPT Plus/Pro
- **GitHub Copilot** — login via `/connect` → GitHub Copilot
- **Claude Pro/Max** — masukkan API key Anthropic via `/connect`

### Opsi E: OpenCode Zen (Pay-as-you-go)

OpenCode Zen adalah model yang sudah diuji dan di-benchmark oleh tim OpenCode. Isi $20, bayar per request. Aktifkan via `/connect` → OpenCode Zen.

> **Catatan**: Semua opsi di atas valid. Untuk pemula, rekomendasi termudah: **Opsi A** (Gemini free tier) karena setup-nya paling cepat dan benar-benar gratis.

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
- **Drag & drop gambar** — OpenCode bisa membaca gambar sebagai konteks

### Mode CLI (Non-Interaktif)

Jalankan task langsung dari shell:

```bash
opencode "Jelaskan struktur file di direktori ini"
opencode "Buatkan docker-compose.yml untuk PostgreSQL + Redis"
opencode "Fix error ini: $(cat error.log)"
```

## 5. Contoh Sesi — Fix Error dengan OpenCode

Anggap ada error di project kamu:

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

OpenCode sekarang siap digunakan. Yang perlu diingat:

- **OpenCode gratis dan open source** — tidak ada biaya lisensi.
- **Biaya hanya dari API provider** — bisa $0 jika pakai free tier (Gemini, Groq) atau model lokal (Ollama).
- **Fungsionalitas penuh di terminal** — tidak perlu desktop app atau IDE extension.

Lanjut ke [Modul 11](11_TERMINAL_ENHANCEMENT.md) untuk meng upgrade terminal kamu dengan Oh My Bash, RTK, alias-hub, eza, dan fastfetch — semuanya bisa diinstall langsung via OpenCode.
