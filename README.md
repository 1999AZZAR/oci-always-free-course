# OCI Always Free Tier & Agent Deployment Course

Panduan lengkap untuk memanfaatkan resource gratis dari Oracle Cloud Infrastructure (OCI), setup jaringan private dengan Tailscale, dan instalasi AI Agent (seperti OpenClaw) untuk asisten pribadi 24/7.

## Mengapa OCI Always Free?
Oracle menawarkan resource gratis yang sangat melimpah dibanding provider lain:
- **ARM Ampere A1 Compute:** Sampai dengan 2 OCPU dan 12 GB RAM untuk akun Free Tier (sebelumnya 4 OCPU / 24 GB RAM, dikurangi per 15 Juni 2026. Akun Pay-As-You-Go lama mungkin masih mendapatkan limit 4 OCPU / 24 GB RAM).
- **AMD Compute:** 2 instance kecil (1/8 OCPU, 1 GB RAM).
- **Block Storage:** Total 200 GB gratis.
- **Data Transfer:** 10 TB per bulan.

## Daftar Modul

### Fase 1: Akun & Administrasi
1. [Pendaftaran & Verifikasi Akun](modules/01_SIGNUP_VERIFICATION.md)
2. [Pemilihan Region & Konsep "Always Free"](modules/02_REGION_CONCEPTS.md)

### Fase 2: Provisioning Resource
3. [Membuat Instance ARM Ampere (12GB/24GB RAM)](modules/03_CREATE_INSTANCE.md)
4. [Konfigurasi VCN & Security Lists (Firewall)](modules/04_NETWORK_SECURITY.md)

### Fase 3: Networking & Akses
5. [Setup Tailscale (Mesh VPN Private)](modules/05_TAILSCALE_SETUP.md)
6. [SSH Tanpa IP Publik (Keamanan Maksimal)](modules/06_SECURE_SSH.md)

### Fase 4: Agent Deployment
7. [Persiapan Environment (Docker & Node.js)](modules/07_PREPARE_ENV.md)
8. [Instalasi OpenClaw Agent](modules/08_INSTALL_OPENCLAW.md)
9. [Menghubungkan ke Messaging (Telegram/WhatsApp)](modules/09_MESSAGING_CONNECT.md)

---
**Disclaimer**: Resource "Always Free" tergantung ketersediaan di masing-masing region. Disarankan menggunakan sistem "Pay As You Go" (tanpa biaya jika hanya pakai free tier) untuk akses resource yang lebih stabil.
