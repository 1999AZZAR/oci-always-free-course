# Modul 4: Konfigurasi VCN & Security Lists

Pikirkan Virtual Cloud Network (VCN) sebagai pagar rumah digitalmu. Security Lists adalah penjaga pintunya.

## 1. Virtual Cloud Network (VCN)
Saat membuat instance, OCI biasanya membuatkan VCN default. VCN terdiri dari:
- **Subnet**: Pembagian area di dalam network (biasanya Public Subnet agar bisa diakses internet).
- **Internet Gateway**: Pintu keluar masuk traffic internet.
- **Route Tables**: Aturan ke mana traffic harus pergi.

## 2. Security Lists (Ingress/Egress Rules)
Secara default, Oracle sangat ketat. Hampir semua port ditutup kecuali port 22 (SSH).

### Membuka Port (Ingress Rules)
Jika kamu ingin menjalankan aplikasi yang bisa diakses publik (misal: Web Server), kamu harus membuka portnya:
1. Pergi ke **Networking** -> **Virtual Cloud Networks**.
2. Pilih VCN kamu -> Pilih **Security Lists** -> **Default Security List**.
3. Klik **Add Ingress Rules**.
4. Isi data berikut:
   - **Source CIDR**: `0.0.0.0/0` (Artinya dari mana saja).
   - **IP Protocol**: `TCP`.
   - **Destination Port Range**: `80, 443` (Untuk HTTP/HTTPS).
5. Klik **Add**.

## 3. Firewall di Level OS (Ubuntu)
Selain Security List di OCI Dashboard, Ubuntu juga punya firewall sendiri (`iptables` atau `ufw`). Oracle sering kali menggunakan `iptables` secara default yang sangat restriktif.

**PENTING**: Jika kamu sudah buka port di OCI Dashboard tapi aplikasi tetap tidak bisa diakses, jalankan perintah ini di server:
```bash
# Mematikan iptables bawaan Oracle agar ufw bisa handle
sudo iptables -F
sudo netfilter-persistent save
```
Atau buka manual di iptables:
```bash
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
sudo netfilter-persistent save
```
