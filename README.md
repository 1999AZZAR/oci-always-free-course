# OCI Always Free Tier — Panduan Deployment AI Agent

Panduan lengkap untuk memanfaatkan resource gratis dari Oracle Cloud Infrastructure (OCI), mengamankan akses dengan Tailscale mesh VPN, dan melindungi server dengan Fail2ban.

## Mengapa OCI?

Dibanding AWS Free Tier atau GCP Free Tier yang limitnya cukup ketat, OCI memberikan lebih banyak RAM dan CPU tanpa biaya — cocok untuk self-hosting agent, API, atau container stack kecil.

## Resource Always Free (Per Juli 2026)

> **Update Juni 2026:** Oracle memangkas limit ARM Ampere A1 untuk akun Free Tier baru dari 4 OCPU / 24 GB menjadi **2 OCPU / 12 GB RAM**. Akun Pay-As-You-Go lama umumnya tidak terpengaruh. Detail di [Modul 2](modules/02_REGION_CONCEPTS.md).

| Resource | Spesifikasi |
|---|---|
| ARM Ampere A1 Compute | 2 OCPU + 12 GB RAM (Free Tier baru) atau 4 OCPU + 24 GB RAM (PAYG / akun lama) |
| AMD Micro Instances | 2x VM.Standard.E2.1.Micro (1/8 OCPU, 1 GB RAM masing-masing) |
| Block Storage | 200 GB total (boot + block volume) |
| Object Storage | 20 GB |
| Outbound Data Transfer | 10 TB/bulan |
| Autonomous Database | 2 instance (ATP atau ADW) |
| Load Balancer | 1x Flexible (10 Mbps) + 1x Network Load Balancer |
| Monitoring & Logging | 500 juta ingestion points + 10 GB log storage/bulan |

Resource di atas berlaku **selamanya** (bukan 12 bulan seperti AWS Free Tier), selama akun aktif dan tidak idle lebih dari 30 hari berturut-turut.

## Daftar Modul

### Fase 1: Akun & Administrasi
1. [Pendaftaran & Verifikasi Akun](modules/01_SIGNUP_VERIFICATION.md)
2. [Pemilihan Region & Kuota Always Free](modules/02_REGION_CONCEPTS.md)

### Fase 2: Provisioning Resource
3. [Membuat Instance ARM Ampere](modules/03_CREATE_INSTANCE.md)
4. [Konfigurasi VCN & Firewall](modules/04_NETWORK_SECURITY.md)

### Fase 3: Akses Aman
5. [Setup Tailscale Mesh VPN](modules/05_TAILSCALE_SETUP.md)
6. [SSH Tanpa IP Publik](modules/06_SECURE_SSH.md)

### Fase 4: Server Hardening
7. [Persiapan Environment (Docker & Node.js)](modules/07_PREPARE_ENV.md)
8. [Instalasi & Konfigurasi Fail2ban](modules/08_FAIL2BAN_SETUP.md)
9. [Notifikasi Fail2ban via Telegram](modules/09_MESSAGING_CONNECT.md)

### Fase 5: Terminal UX
10. [Developer Toolchain (Oh My Bash, RTK, OpenCode, alias-hub, eza, fastfetch)](modules/10_TERMINAL_UX.md)

---

**Catatan**: Selama penggunaan kamu masih di bawah limit Always Free, Oracle tidak akan menagih sepeser pun — bahkan setelah upgrade ke Pay-As-You-Go. Upgrade tersebut justru disarankan untuk stabilitas resource dan menghindari idle reclamation.

Tidak familiar dengan istilah teknis tertentu? Lihat [Glosarium](GLOSARIUM.md).
