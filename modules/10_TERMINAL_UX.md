# Modul 10: Terminal UX — Developer Toolchain Setup

Modul ini mengubah shell Ubuntu bawaan yang polos menjadi lingkungan kerja yang nyaman untuk sesi coding panjang. Mencakup shell framework, alias cerdas, token optimizer untuk AI agent, CLI coding agent, dan system info display yang rapi.

Semua langkah dijalankan di dalam instance OCI via SSH.

## Tool yang Akan Diinstall

| Tool | Fungsi |
|---|---|
| Oh My Bash | Shell framework — tema, plugin, autocompletion |
| RTK | Rust Token Killer — kompres output shell 60–90% untuk hemat token AI |
| OpenCode | AI coding agent berbasis TUI, gratis, 75+ provider AI |
| alias-hub | Koleksi shell alias modular untuk produktivitas |
| eza | Pengganti `ls` yang lebih informatif (warna, ikon, tree view) |
| fastfetch | System info display modern (pengganti neofetch yang sudah deprecated) |
| neofetch-ascii | Custom ASCII art + config untuk fastfetch |

---

## 1. Install Dependensi Dasar

Pastikan semua tool inti tersedia sebelum memulai:

```bash
sudo apt update
sudo apt install -y curl wget git build-essential
```

### Install Docker CLI (jika belum ada dari Modul 7)

```bash
# Cek dulu apakah docker sudah ada
docker --version

# Jika belum, install
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
```

### Install eza

`eza` tersedia di repository Ubuntu 23.04+ secara langsung. Untuk Ubuntu 22.04, gunakan binary dari GitHub:

```bash
# Ubuntu 22.04 (ARM64)
EZA_VERSION=$(curl -s "https://api.github.com/repos/eza-community/eza/releases/latest" | grep '"tag_name"' | sed -E 's/.*"v([^"]+)".*/\1/')
wget -q "https://github.com/eza-community/eza/releases/latest/download/eza_aarch64-unknown-linux-gnu.tar.gz" -O /tmp/eza.tar.gz
tar xf /tmp/eza.tar.gz -C /tmp
sudo mv /tmp/eza /usr/local/bin/eza
rm /tmp/eza.tar.gz

# Verifikasi
eza --version
```

Untuk Ubuntu 24.04+:
```bash
sudo apt install -y eza
```

---

## 2. Install Oh My Bash

Oh My Bash adalah framework yang menambahkan tema, plugin, dan autocompletion ke bash tanpa mengganti shell:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```

Installer akan meminta konfirmasi. Jawab `Y`. Setelah selesai, shell akan langsung berubah.

### Pilih Tema

Oh My Bash datang dengan puluhan tema. Coba beberapa pilihan yang cocok untuk server headless:

```bash
# Edit konfigurasi
nano ~/.bashrc
```

Cari baris `OSH_THEME=` dan ganti dengan tema yang kamu inginkan:

```
OSH_THEME="powerline-multiline"   # Rekomendasi — multi-line, informatif
# OSH_THEME="agnoster"            # Mirip agnoster zsh
# OSH_THEME="font"                # Minimalis, cepat
# OSH_THEME="cupcake"             # Warna-warni
```

Simpan, lalu muat ulang:
```bash
source ~/.bashrc
```

> **Catatan font**: Tema berbasis powerline membutuhkan Nerd Font di terminal client kamu (laptop), bukan di server. Install [Nerd Fonts](https://www.nerdfonts.com/font-downloads) di laptop kamu jika karakter prompt terlihat rusak.

---

## 3. Install RTK (Rust Token Killer)

RTK mengintersep output perintah shell (`git status`, `npm install`, dll.) dan mengkompresinya sebelum dikirim ke context AI. Cocok dipakai bersama OpenCode atau AI coding agent lainnya.

```bash
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/master/install.sh | sh
```

Setelah instalasi, inisialisasi secara global agar RTK aktif otomatis:

```bash
rtk init -g
source ~/.bashrc
```

Verifikasi instalasi dan lihat statistik penghematan token:
```bash
rtk gain
```

Cara pakai manual (prefix sebelum command):
```bash
rtk git status
rtk npm install
rtk docker ps -a
```

Setelah `rtk init -g`, semua command yang didukung sudah otomatis dikompresi outputnya — tidak perlu prefix manual lagi.

---

## 4. Install OpenCode (AI Coding Agent)

OpenCode adalah terminal UI untuk berinteraksi dengan AI dalam konteks kode. Mendukung 75+ provider (OpenAI, Gemini, Claude, Ollama untuk lokal).

```bash
curl -fsSL https://opencode.ai/install | bash
source ~/.bashrc
```

Verifikasi:
```bash
opencode --version
```

### Konfigurasi API Key

Jalankan OpenCode untuk pertama kali:
```bash
opencode
```

Di dalam TUI, ketik `/connect` untuk mengatur API key. OpenCode akan mengarahkan kamu ke browser untuk autentikasi, atau kamu bisa set langsung via environment variable:

```bash
# Tambahkan ke ~/.bashrc
export OPENAI_API_KEY="sk-..."
export ANTHROPIC_API_KEY="sk-ant-..."
export GEMINI_API_KEY="AI..."
```

Simpan dan muat ulang:
```bash
source ~/.bashrc
```

### Cara Pakai Dasar

```bash
# Buka TUI interaktif
opencode

# Jalankan task langsung dari CLI
opencode "jelaskan error ini: $(cat error.log)"
```

Di dalam TUI:
- `Tab` — toggle antara Plan mode dan Build mode
- `/connect` — kelola API key dan provider
- `Ctrl+C` — keluar

---

## 5. Install alias-hub

alias-hub adalah koleksi alias shell modular yang diorganisir per kategori (git, docker, file management, networking, dll.):

```bash
cd ~
git clone https://github.com/1999AZZAR/alias-hub.git
```

Tambahkan sourcing ke `~/.bashrc` agar alias dimuat setiap session:

```bash
# Tambahkan ke bagian bawah ~/.bashrc
echo "" >> ~/.bashrc
echo "# alias-hub" >> ~/.bashrc
echo 'for f in ~/alias-hub/aliases/*.sh; do source "$f"; done' >> ~/.bashrc
```

Muat ulang:
```bash
source ~/.bashrc
```

Cek alias yang tersedia:
```bash
alias | grep -i docker   # Alias untuk docker
alias | grep -i git      # Alias untuk git
```

Update alias-hub ke versi terbaru:
```bash
cd ~/alias-hub && git pull
```

---

## 6. Install fastfetch & Custom ASCII (via OpenCode)

### Install fastfetch

Gunakan OpenCode untuk menginstall fastfetch — ini cara yang tepat untuk membiasakan diri dengan agent:

```bash
opencode "Install fastfetch on this Ubuntu ARM server and verify it works"
```

Atau install manual:
```bash
sudo apt install -y fastfetch
fastfetch --version
```

### Install Custom ASCII dari neofetch_ascii

Clone repo kustomisasi:
```bash
cd ~
git clone https://github.com/1999AZZAR/neofetch_ascii.git
```

Salin config fastfetch ke lokasi yang tepat:
```bash
mkdir -p ~/.config/fastfetch
```

Lihat isi repo untuk file config yang tersedia:
```bash
ls ~/neofetch_ascii/
```

Salin config yang kamu inginkan:
```bash
# Sesuaikan nama file dengan yang ada di repo
cp ~/neofetch_ascii/config.jsonc ~/.config/fastfetch/config.jsonc
```

Jika ada file ASCII art terpisah, salin juga ke direktori yang direferensikan config tersebut.

Test tampilan:
```bash
fastfetch
```

### Fix Config via OpenCode

Jika ada error pada config (path tidak ditemukan, format salah, dll.), minta OpenCode untuk memperbaikinya:

```bash
opencode "Fix my fastfetch config at ~/.config/fastfetch/config.jsonc. It's based on this repo: https://github.com/1999AZZAR/neofetch_ascii. Show me the current errors and fix them."
```

OpenCode akan membaca file, menganalisis error, dan memperbaiki config secara langsung.

---

## 7. Jalankan fastfetch Saat Login

Tambahkan fastfetch ke `~/.bashrc` agar tampil setiap kali SSH masuk:

```bash
echo "" >> ~/.bashrc
echo "# System info on login" >> ~/.bashrc
echo "fastfetch" >> ~/.bashrc
```

---

## Ringkasan Akhir

Setelah semua langkah selesai, shell kamu sudah punya:

- **Oh My Bash** — prompt yang informatif dengan tema visual
- **RTK** — output command yang dikompresi otomatis, token lebih hemat
- **OpenCode** — AI coding agent langsung dari terminal
- **alias-hub** — shortcut untuk git, docker, file management, dan lainnya
- **eza** — directory listing yang jauh lebih readable dari `ls`
- **fastfetch** — system info yang tampil rapi setiap kali login

Test semuanya dengan:
```bash
eza -la --icons        # Directory listing
rtk gain              # Cek token yang sudah dihemat RTK
fastfetch             # System info
opencode              # Buka AI coding agent
```
