# Modul 7: Persiapan Environment (Docker & Node)

Sebelum kita deploy agent, kita butuh pondasi yang kuat. Kita akan pakai Docker agar aplikasi lebih rapi.

## 1. Update OS
```bash
sudo apt update && sudo apt upgrade -y
```

## 2. Install Docker & Docker Compose
ARM Ampere sangat powerful menjalankan container.
```bash
# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Tambahkan user ubuntu ke group docker biar nggak perlu sudo terus
sudo usermod -aG docker $ubuntu
newgrp docker
```

## 3. Install Node.js (via NVM)
Banyak agent (termasuk OpenClaw) butuh Node.js. Menggunakan NVM adalah cara terbaik untuk manage versi.
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc

# Install Node LTS
nvm install --lts
nvm use --lts
```

## 4. Install Tool Tambahan (Quality of Life)
```bash
sudo apt install -y git htop tmux ncdu
```
- **htop**: Buat mantau penggunaan RAM (12GB/24GB) kamu.
- **tmux**: Biar session terminal nggak mati pas kamu disconnect.
- **ncdu**: Cek file mana yang bikin disk penuh.
