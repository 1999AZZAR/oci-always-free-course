# Modul 11: Terminal Enhancement Toolkit

Modul ini mengubah shell Ubuntu bawaan menjadi lingkungan kerja yang nyaman — prompt informatif, alias cerdas, output terkendali, dan info sistem yang rapi. Semua tool diinstall **menggunakan OpenCode** dari Modul 10, membiasakan kamu dengan agent-driven workflow.

## Tool yang Akan Diinstall

| Tool | Fungsi |
|---|---|
| Oh My Bash | Shell framework — tema, plugin, autocompletion |
| RTK | Rust Token Killer — kompres output shell 60–90% untuk hemat token AI |
| alias-hub | Koleksi shell alias modular untuk produktivitas |
| eza | Pengganti `ls` yang lebih informatif (warna, ikon, tree view) |
| fastfetch | System info display modern (pengganti neofetch) |
| neofetch-ascii | Custom ASCII art + config untuk fastfetch |

## 1. Install eza via OpenCode

`eza` adalah pengganti `ls` modern dengan warna, ikon, dan tree view. Minta OpenCode menginstallnya:

```bash
opencode "Install eza di server Ubuntu ARM64 ini. Cek versi Ubuntu dulu (pakai lsb_release -a), lalu pilih metode yang tepat: apt langsung untuk 24.04+, atau download binary dari GitHub releases untuk 22.04. Verifikasi setelah selesai."
```

OpenCode akan menentukan versi OS dan memilih metode install yang tepat. Alternatif, instal manual:

**Ubuntu 22.04 (ARM64)**:
```bash
EZA_VERSION=$(curl -s "https://api.github.com/repos/eza-community/eza/releases/latest" | grep '"tag_name"' | sed -E 's/.*"v([^"]+)".*/\1/')
wget -q "https://github.com/eza-community/eza/releases/latest/download/eza_aarch64-unknown-linux-gnu.tar.gz" -O /tmp/eza.tar.gz
tar xf /tmp/eza.tar.gz -C /tmp
sudo mv /tmp/eza /usr/local/bin/eza
rm /tmp/eza.tar.gz
eza --version
```

**Ubuntu 24.04+**:
```bash
sudo apt install -y eza
```

## 2. Install Oh My Bash via OpenCode

Oh My Bash menambahkan tema, plugin, dan autocompletion ke bash tanpa mengganti shell.

Gunakan OpenCode:

```bash
opencode "Install Oh My Bash di server ini. Jalankan installer dari curl, lalu minta user memilih tema (default powerline-multiline) dan edit .bashrc untuk setting OSH_THEME. Source file setelah selesai."
```

Atau manual:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```

Jawab `Y` saat diminta konfirmasi.

### Pilih Tema

Edit `~/.bashrc` dan cari baris `OSH_THEME=`:

```bash
nano ~/.bashrc
```

Ganti dengan tema yang kamu suka:

```
OSH_THEME="powerline-multiline"   # Rekomendasi — multi-line, informatif
# OSH_THEME="agnoster"            # Mirip agnoster zsh
# OSH_THEME="font"                # Minimalis, cepat
```

Simpan lalu muat ulang:

```bash
source ~/.bashrc
```

> **Catatan font**: Tema powerline butuh Nerd Font di terminal client laptop kamu, bukan di server. Download dari [Nerd Fonts](https://www.nerdfonts.com/font-downloads) jika karakter prompt terlihat rusak.

## 3. Install RTK via OpenCode

RTK (Rust Token Killer) mengkompresi output perintah shell sebelum dikirim ke context AI — menghemat token 60–90%.

```bash
opencode "Install RTK dari https://github.com/rtk-ai/rtk. Jalankan curl install lalu init global. Verifikasi dengan rtk gain."
```

Atau manual:

```bash
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/master/install.sh | sh
rtk init -g
source ~/.bashrc
```

Verifikasi:

```bash
rtk gain
```

RTK otomatis aktif untuk command yang didukung. Contoh output yang dikompresi:

```
rtk git status          # Output ringkas, bukan full diff
rtk npm install         # Hanya error/warning, skip stream sukses
rtk docker ps -a        # Tabel diformat ulang
```

## 4. Install alias-hub via OpenCode

alias-hub adalah koleksi alias shell modular per kategori (git, docker, file management, networking):

```bash
opencode "Clone repo alias-hub dari https://github.com/1999AZZAR/alias-hub.git ke home directory, lalu tambahkan sourcing ke .bashrc agar semua alias dimuat otomatis setiap session."
```

Atau manual:

```bash
cd ~
git clone https://github.com/1999AZZAR/alias-hub.git
echo "" >> ~/.bashrc
echo "# alias-hub" >> ~/.bashrc
echo 'for f in ~/alias-hub/aliases/*.sh; do source "$f"; done' >> ~/.bashrc
source ~/.bashrc
```

Cek alias yang tersedia:

```bash
alias | grep -i docker
alias | grep -i git
```

Update alias-hub nanti:

```bash
cd ~/alias-hub && git pull
```

## 5. Install fastfetch & Custom ASCII

fastfetch menampilkan info sistem (OS, CPU, RAM, uptime) setiap kali login — pengganti neofetch yang sudah deprecated.

### Install fastfetch

```bash
opencode "Install fastfetch di server Ubuntu ARM ini dan verifikasi versinya."
```

Atau manual:

```bash
sudo apt install -y fastfetch
fastfetch --version
```

### Custom ASCII Art

Clone repo kustomisasi untuk tampilan fastfetch yang lebih personal:

```bash
opencode "Clone repo neofetch_ascii dari https://github.com/1999AZZAR/neofetch_ascii.git ke home directory. Buat folder ~/.config/fastfetch, lalu copy file config yang sesuai ke sana. Test dengan fastfetch."
```

Atau manual:

```bash
cd ~
git clone https://github.com/1999AZZAR/neofetch_ascii.git
mkdir -p ~/.config/fastfetch
ls ~/neofetch_ascii/
cp ~/neofetch_ascii/config.jsonc ~/.config/fastfetch/config.jsonc
fastfetch
```

### Fix Config via OpenCode

Jika ada error path atau format config, biarkan OpenCode memperbaikinya:

```bash
opencode "Fix my fastfetch config at ~/.config/fastfetch/config.jsonc so it works with the neofetch_ascii repo. Test after fixing."
```

## 6. Auto-run fastfetch Saat Login

```bash
echo "" >> ~/.bashrc
echo "# System info on login" >> ~/.bashrc
echo "fastfetch" >> ~/.bashrc
```

Logout dan login lagi untuk melihat hasilnya.

## 7. Verifikasi Semua Tool

Test bahwa semua tool berfungsi:

```bash
eza -la --icons         # Directory listing dengan ikon
rtk gain               # Statistik hemat token
fastfetch              # System info
alias | head -20       # Alias yang aktif
```

---

## Ringkasan Akhir

Setelah modul ini, terminal kamu punya:

- **Oh My Bash** — prompt informatif dengan tema visual
- **RTK** — output command terkendali, hemat token AI
- **alias-hub** — shortcut produktif untuk git, docker, file management
- **eza** — directory listing jauh lebih readable dari `ls`
- **fastfetch** — system info rapi setiap login

Semua tool diinstall dan dikelola via OpenCode — kamu sekarang terbiasa dengan agent-driven workflow untuk administrasi server.
