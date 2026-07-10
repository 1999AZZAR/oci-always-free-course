# Modul 3: Membuat Instance ARM Ampere

Resource paling berharga di OCI Always Free adalah instance **Ampere A1 Compute**. Kamu bisa mendapatkan total 12 GB RAM (Free Tier baru) atau hingga 24 GB RAM (Pay-As-You-Go / Akun lama) secara gratis.

## Spesifikasi Ideal (Golden Ratio)
Untuk menjalankan agent seperti OpenClaw dengan mulus, buat instance sesuai limit akun kamu:
- **Image:** Canonical Ubuntu 22.04 atau 24.04 (AArch64).
- **Shape:** VM.Standard.A1.Flex.
- **OCPU & Memory:**
  - **Akun Free Tier Baru (Mulai 15 Juni 2026):** 2 OCPU dan 12 GB RAM.
  - **Akun Pay-As-You-Go / Akun Lama:** 4 OCPU dan 24 GB RAM.
- **Boot Volume:** 50 GB - 100 GB (Total gratis adalah 200 GB).

## Langkah-langkah
1. Masuk ke OCI Console -> **Compute** -> **Instances**.
2. Klik **Create Instance**.
3. **Name**: Beri nama (misal: `agent-prod-01`).
4. **Image and Shape**:
   - Klik **Change Image** -> Pilih Ubuntu.
   - Klik **Change Shape** -> Pilih **Ampere (ARM)** -> VM.Standard.A1.Flex. Geser slider OCPU dan RAM sesuai jenis akun kamu (2 OCPU / 12 GB RAM untuk Free Tier, atau 4 OCPU / 24 GB RAM jika didukung).
5. **Networking**: Pilih Virtual Cloud Network (VCN) default. Pastikan "Assign a public IPv4 address" dipilih.
6. **Add SSH Keys**:
   - Pilih **Generate a key pair for me**.
   - **WAJIB**: Download Private Key (.key) dan simpan di tempat aman. Kamu tidak akan bisa mendownloadnya lagi nanti.
7. **Boot Volume**: Biarkan default atau ubah ke 100 GB jika ingin lebih lega.
8. Klik **Create**.

## Catatan "Out of Capacity"
Jika muncul error *Out of Capacity*, artinya resource ARM di region tersebut sedang habis terpakai orang lain. 
**Tips:**
- Coba kecilkan spesifikasi instance (misal ke 1 OCPU dan 6 GB RAM) agar lebih mudah mendapatkan alokasi.
- Upgrade akun ke **Pay As You Go**. Oracle tidak akan menagih selama pemakaian di bawah limit Always Free, tapi kamu akan mendapatkan prioritas lebih tinggi untuk alokasi resource.
