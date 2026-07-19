# Glosarium

Kumpulan istilah teknis yang digunakan di seluruh modul kursus ini. Disusun alfabetis.

---

## A

**Always Free**
Tier layanan Oracle Cloud yang menyediakan resource komputasi tanpa biaya selama akun aktif. Berbeda dengan free trial (yang punya batas waktu), Always Free tidak kadaluarsa — tapi ada batas resource dan syarat penggunaan aktif.

**Ampere A1**
Prosesor ARM buatan Ampere Computing yang digunakan OCI untuk instance Always Free. Arsitekturnya `aarch64` (ARM64), berbeda dari kebanyakan server Intel/AMD (x86_64). Sangat efisien untuk workload container dan web service.

**API Key**
String autentikasi yang digunakan untuk mengidentifikasi siapa yang memanggil sebuah API. Diperlukan untuk OpenAI, Gemini, Anthropic, dan provider AI lainnya. Jangan pernah commit ke repository Git.

**apt**
Package manager bawaan Ubuntu. Digunakan untuk install, update, dan hapus software dari repository resmi Ubuntu. Perintah umum: `apt install`, `apt update`, `apt upgrade`.

**AArch64**
Nama teknis untuk arsitektur ARM 64-bit. Digunakan secara bergantian dengan `arm64`. Saat download binary atau image Docker, pilih versi `aarch64` atau `arm64` untuk instance Ampere A1.

**Availability Domain (AD)**
Subdivisi dari sebuah region OCI. Satu region bisa punya 1–3 AD. Masing-masing AD punya hardware fisik yang terpisah. Jika satu AD out of capacity untuk ARM, coba AD lain di region yang sama.

---

## B

**Bash**
Shell default di Ubuntu. Program yang menginterpretasikan perintah yang kamu ketik di terminal. File konfigurasi utamanya adalah `~/.bashrc`.

**Block Volume**
Penyimpanan berbasis blok di OCI, setara dengan hard disk virtual. Bisa di-attach ke instance dan digunakan sebagai disk tambahan. Always Free menyediakan 200 GB total untuk boot volume dan block volume.

**Boot Volume**
Block volume khusus yang menyimpan sistem operasi instance. Saat instance dihapus, boot volume bisa dipertahankan untuk backup atau migrasi ke instance baru.

---

## C

**CIDR**
*Classless Inter-Domain Routing*. Notasi untuk mendefinisikan rentang alamat IP. `0.0.0.0/0` berarti "semua IP" (akses publik), `10.0.0.0/24` berarti 256 IP dalam subnet tertentu.

**Container**
Unit software yang mengemas aplikasi beserta semua dependensinya dalam satu paket yang bisa dijalankan di mana saja. Docker adalah runtime container paling umum. Berbeda dengan virtual machine — container berbagi kernel OS host.

**curl**
Alat CLI untuk melakukan HTTP request. Banyak digunakan untuk download file atau memanggil API dari terminal. Contoh: `curl -fsSL https://example.com/script.sh | bash`.

---

## D

**DERP Server**
*Designated Encrypted Relay for Packets*. Server relay yang digunakan Tailscale ketika koneksi direct peer-to-peer tidak bisa dibuat (karena NAT atau firewall yang ketat). Koneksi via DERP lebih lambat dari direct. Membuka UDP port 41641 membantu menghindari relay.

**Docker**
Platform untuk membangun dan menjalankan container. Docker Compose memungkinkan definisi multi-container stack dalam satu file YAML.

**Docker Compose**
Tool untuk mendefinisikan dan menjalankan aplikasi multi-container. Konfigurasi ditulis dalam `compose.yaml` (atau `docker-compose.yml`). Versi modern menggunakan perintah `docker compose` (tanpa tanda hubung).

---

## E

**eza**
Pengganti modern untuk perintah `ls`. Mendukung warna, ikon (dengan Nerd Font), tree view, dan tampilan metadata yang lebih informatif. Ditulis dalam Rust.

---

## F

**Fail2ban**
Framework untuk mencegah serangan brute force dengan cara memonitor log service (seperti SSH) dan secara otomatis memblokir alamat IP yang menunjukkan perilaku mencurigakan menggunakan aturan iptables. Dijalankan sebagai systemd service.

**fastfetch**
Tool untuk menampilkan informasi sistem (OS, CPU, RAM, uptime, dll.) di terminal saat login. Pengganti aktif untuk neofetch yang sudah tidak dikembangkan. Ditulis dalam C, jauh lebih cepat dari neofetch.

**Free Tier**
Lihat: *Always Free*

---

## G

**Git**
Version control system. Digunakan untuk melacak perubahan kode, clone repository, dan push ke layanan seperti GitHub.

---

## H

**Home Region**
Region OCI yang dipilih saat membuat akun. Semua resource Always Free hanya bisa dibuat di Home Region. Tidak bisa diubah setelah akun jadi.

**htop**
Tool monitoring interaktif untuk CPU, RAM, dan proses yang berjalan. Lebih visual dari `top` bawaan Linux. Install via `apt install htop`.

---

## I

**Ingress Rule**
Aturan firewall yang mengizinkan traffic masuk ke instance dari luar. Dikonfigurasi di OCI Security List. Lawan dari egress rule (traffic keluar).

**iptables**
Firewall bawaan Linux kernel. OCI Ubuntu image datang dengan iptables yang sudah dikonfigurasi untuk keamanan. Harus diatur secara spesifik per port — jangan di-flush keseluruhan karena bisa memutus koneksi iSCSI.

**iSCSI**
*Internet Small Computer System Interface*. Protokol yang digunakan OCI untuk menghubungkan boot volume ke instance melalui jaringan. Jika koneksi iSCSI putus (misal karena iptables di-flush), instance bisa tidak bisa boot.

---

## M

**Mesh VPN**
Jenis VPN di mana setiap perangkat terhubung langsung ke perangkat lain (peer-to-peer), bukan semuanya melewati satu server central. Tailscale menggunakan arsitektur mesh. Latensi lebih rendah dan tidak ada single point of failure.

---

## N

**ncdu**
*NCurses Disk Usage*. Tool interaktif untuk menganalisis penggunaan disk per direktori. Berguna untuk menemukan file besar yang menghabiskan storage. Install via `apt install ncdu`.

**Nerd Font**
Font yang sudah dipatch dengan ikon tambahan (ikon file, git, OS, dll.). Digunakan oleh eza dan tema Oh My Bash berbasis powerline. Harus diinstall di terminal emulator di laptop/PC kamu, bukan di server.

**NVM**
*Node Version Manager*. Tool untuk menginstall dan mengelola beberapa versi Node.js secara bersamaan. Lebih fleksibel dari `apt install nodejs` karena memungkinkan switch versi per project.

---

## O

**OCPU**
*Oracle Compute Unit*. Satuan CPU yang digunakan OCI. 1 OCPU setara dengan 2 vCPU (virtual CPU). Ampere A1 menggunakan OCPU sebagai satuan.

**Oh My Bash**
Framework shell yang menambahkan tema, plugin, dan autocompletion ke bash. Diinstall di atas bash yang sudah ada tanpa mengganti shell.

**OpenCode**
AI coding agent open-source berbasis terminal. Menyediakan TUI (Terminal User Interface) untuk berinteraksi dengan model AI dalam konteks kode. Mendukung 75+ provider.

---

## P

**Pay-As-You-Go (PAYG)**
Tier akun OCI di atas Always Free. Memberikan akses ke semua layanan OCI dengan penagihan berdasarkan pemakaian. Jika penggunaan masih di dalam limit Always Free, tidak ada tagihan. Disarankan untuk menghindari idle reclamation dan mendapat prioritas resource ARM.

**PM2**
Process Manager untuk Node.js. Menjaga aplikasi tetap berjalan di background, auto-restart jika crash, dan konfigurasi auto-start setelah reboot.

**Private Key**
File kriptografi yang digunakan untuk autentikasi SSH. Pasangannya adalah public key yang disimpan di server. File ini harus dijaga kerahasiaannya dan tidak boleh dibagikan. Permission yang benar: `chmod 400`.

---

## R

**Region**
Lokasi geografis data center OCI. Setiap region terdiri dari beberapa Availability Domain. Always Free resource hanya tersedia di Home Region.

**RTK (Rust Token Killer)**
CLI proxy yang mengkompresi output perintah shell sebelum masuk ke context AI. Mengurangi penggunaan token 60–90% untuk perintah seperti `git status`, `npm install`, dll. Ditulis dalam Rust, binary tunggal tanpa dependensi.

---

## S

**Security List**
Firewall di level subnet OCI. Aturan di Security List berlaku untuk semua instance dalam subnet tersebut. Salah satu dari dua lapisan firewall yang harus dikonfigurasi (lapisan lain: iptables di OS).

**SSH**
*Secure Shell*. Protokol untuk akses remote ke server secara terenkripsi. Default port 22. Gunakan private key untuk autentikasi, bukan password.

**SSH Config** (`~/.ssh/config`)
File konfigurasi client SSH. Memungkinkan pembuatan alias untuk koneksi SSH — misalnya `ssh oci-agent` instead of `ssh -i ~/key.key ubuntu@100.x.x.x`.

**Subnet**
Subdivisi dari VCN. Instance OCI dibuat di dalam subnet. Ada dua jenis: public subnet (instance bisa dapat IP publik) dan private subnet (hanya bisa diakses dari dalam VCN).

---

## T

**Tailscale**
Layanan mesh VPN berbasis WireGuard. Setiap perangkat yang terdaftar mendapat IP internal `100.x.x.x` yang bisa saling berkomunikasi secara terenkripsi. Tier gratis mendukung hingga 100 perangkat.

**Tailscale SSH**
Fitur Tailscale yang menggantikan SSH key authentication dengan Tailscale identity. Diaktifkan dengan flag `--ssh` saat menjalankan `tailscale up`. Akses dikontrol via Tailscale Admin Console.

**Tenancy**
Nama untuk akun/organisasi di OCI. Setiap tenancy punya namespace sendiri untuk semua resource. Nama tenancy dipilih saat registrasi dan tidak bisa diubah.

**tmux**
Terminal multiplexer. Memungkinkan beberapa "window" terminal dalam satu session SSH, dan session tetap berjalan meski koneksi SSH putus. Berguna untuk proses panjang seperti instalasi atau build.

**TUI**
*Terminal User Interface*. Antarmuka visual yang berjalan di dalam terminal, menggunakan karakter untuk membuat layout. OpenCode menggunakan TUI untuk interface chat-nya.

---

## U

**Ubuntu**
Distribusi Linux yang digunakan dalam kursus ini. OCI menyediakan image Ubuntu `aarch64` resmi untuk instance ARM. Versi yang didukung: 22.04 LTS dan 24.04 LTS.

**ufw**
*Uncomplicated Firewall*. Frontend untuk iptables. Tidak direkomendasikan di OCI Ubuntu karena bisa bertabrakan dengan aturan iptables bawaan Oracle dan memutus koneksi iSCSI boot volume.

---

## V

**VCN**
*Virtual Cloud Network*. Jaringan virtual privat di OCI, setara dengan VPC di AWS. Setiap VCN punya subnet, routing table, internet gateway, dan security list sendiri. OCI Always Free mencakup 2 VCN.

**VNIC**
*Virtual Network Interface Card*. Interface jaringan virtual yang di-attach ke instance OCI. Setiap instance minimal punya satu VNIC yang terhubung ke subnet.

---

## W

**wget**
Alat CLI untuk mendownload file dari URL. Mirip dengan curl tapi lebih sederhana untuk download file langsung. Contoh: `wget https://example.com/file.zip`.

**WireGuard**
Protokol VPN modern yang digunakan Tailscale di bawah hood. Lebih cepat dan lebih sederhana dari OpenVPN atau IPSec. Enkripsi menggunakan ChaCha20.

---

## Z

**Zero Trust**
Model keamanan di mana tidak ada perangkat atau user yang dipercaya secara default, bahkan dari dalam jaringan internal. Tailscale mengimplementasikan prinsip ini — setiap akses harus diautentikasi, bukan sekadar "di jaringan yang benar".
