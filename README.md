# Dokumentasi Lengkap: Praktikum Instalasi Debian 13 (Mode Teks/Headless) dan Web Server Nginx di VMware Workstation

## 📝 Deskripsi Proyek
Repositori ini merupakan dokumentasi komprehensif dari tugas praktikum instalasi sistem operasi **Debian 13** yang dilakukan sepenuhnya dalam **mode teks (headless)** menggunakan perangkat lunak hypervisor **VMware Workstation**. 

Proyek ini bertujuan untuk mensimulasikan lingkungan server produksi sesungguhnya. Selain instalasi OS dasar, proyek ini juga mencakup tahap konfigurasi *sudo*, instalasi *web server* **Nginx**, pembuatan halaman web statis HTML khusus untuk profil kelompok, dan pengaturan **NAT Port Forwarding** agar *web server* lokal pada *Virtual Machine* (Guest) dapat diakses secara publik melalui jaringan mesin fisik (Host).

---

## 📁 Struktur Repositori
Repositori ini berisi berkas-berkas esensial yang mendokumentasikan keseluruhan proses praktikum:
- `README.md`: Berkas dokumentasi utama yang sedang Anda baca.
- `index.html`: **[BERKAS KODE SUMBER ASLI]** Berkas HTML asli yang memuat struktur *website* profil kelompok yang disebarkan pada web server Nginx.
- Direktori/berkas gambar tangkapan layar (screenshot) sebagai bukti pengerjaan setiap tahapan penting.

---

## ⚙️ Persyaratan Sistem (Prerequisites)
Sebelum mengimplementasikan panduan ini, pastikan spesifikasi sistem berikut telah terpenuhi:
1. **Hypervisor:** VMware Workstation Pro.
2. **ISO Image:** `debian-13.4.0-amd64-DVD-1.iso` (Disarankan versi *netinst* untuk instalasi ringan).
3. **Spesifikasi Virtual Machine:**
   - RAM: 2048 MB (Atau 4096 MB jika RAM Host >= 8GB).
   - Penyimpanan (Storage): 20 GB (Opsi: *Store virtual disk as a single file*).
   - Tipe Jaringan (Network): NAT.
4. **Firmware Host:** Fitur Virtualisasi Hardware (Intel VT-x atau AMD-V) wajib diaktifkan melalui BIOS/UEFI komputer Host.

---

## 🚀 Langkah-Langkah Pengerjaan dan Dokumentasi

### Tahap 1: Instalasi Sistem Operasi Debian 13 (Headless Server)
Pada tahap awal, *virtual machine* di-booting menggunakan berkas ISO Debian 13. Metode instalasi yang dipilih adalah **"Install" (mode teks)**—bukan *Graphical Install*—untuk efisiensi. Bahasa yang digunakan disetel ke bahasa Inggris dengan zona lokasi Indonesia. Skema partisi menggunakan *Guided - use entire disk* dengan opsi *All files in one partition*.

Hostname disetel menjadi `server-SI2025A` dengan pembuatan satu *user* administrator (`root`) dan satu *user* standar bernama `kelompok1`.

#### 📌 Screenshot 1: Konfigurasi Pemilihan Perangkat Lunak (Software Selection)
Bagian terpenting dalam membangun *headless server* adalah tahap pemilihan perangkat lunak. Demi menjaga sistem tetap ringan layaknya server sungguhan, **semua lingkungan antarmuka grafis (GUI) seperti "Debian desktop environment" dan "GNOME" tidak dicentang**. Pilihan hanya dibatasi pada **SSH Server** dan **standard system utilities**.
![Screenshot 1: Software Selection](01-software-selection.png)

### Tahap 2: Pasca-Instalasi dan Verifikasi Akses Sistem
Setelah proses instalasi dasar dan penulisan GRUB *bootloader* selesai, sistem akan melakukan *reboot* otomatis. 

#### 📌 Screenshot 2: Login Pertama Kali ke Sistem Operasi
Setelah proses *booting* usai, sistem menampilkan antarmuka berbasis antarmuka baris perintah (CLI). Login pertama kali berhasil diotentikasi menggunakan akun *user* biasa (non-root) yaitu `kelompok1` di dalam terminal mesin dengan hostname `server-SI2025A`.
![Screenshot 2: Prompt Login Pertama Kali](02-debian-login.png)

### Tahap 3: Pembaruan Sistem dan Instalasi Web Server Nginx
Setelah masuk ke dalam sistem, dilakukan eskalasi hak akses menjadi *super user* sementara waktu menggunakan akun root untuk:
1. Memperbarui repositori: `apt update && apt upgrade -y`
2. Menginstal *sudo*: `apt install sudo -y`
3. Memasukkan *user* kelompok ke grup sudo: `usermod -aG sudo kelompok1`
4. Melakukan *reboot* untuk menerapkan perubahan.

Setelah *login* kembali menggunakan akun `kelompok1`, paket esensial (*net-tools*, *curl*, *git*) dan **Nginx** diinstal menggunakan perintah `sudo apt install nginx -y`. Nginx kemudian dihidupkan dan dikonfigurasi agar otomatis berjalan saat *boot* via *systemctl*.

#### 📌 Screenshot 3: Pemeriksaan Status Operasional Service Nginx
Untuk memastikan Nginx telah berjalan sempurna, perintah `sudo systemctl status nginx` dieksekusi. Terminal sistem menampilkan informasi yang jelas dengan teks berwarna hijau bertuliskan **"active (running)"**, menandakan daemon *web server* telah siap melayani permintaan HTTP.
![Screenshot 3: Hasil Pemeriksaan Status Operasional Nginx](03-nginx-status.png)

### Tahap 4: Pembuatan Halaman Web (Profil Kelompok)
Secara bawaan (*default*), Nginx akan menampilkan halaman *Welcome to Nginx*. Untuk menyesuaikan dengan tugas praktikum, halaman bawaan tersebut diganti dengan identitas anggota kelompok.

#### 📌 Screenshot 4: Proses Penyuntingan File HTML via Editor Teks Nano
Menggunakan editor teks berbasis terminal, Nano, berkas konfigurasi *root web* disunting lewat perintah `sudo nano /var/www/html/index.html`. Layar berikut mendemonstrasikan proses modifikasi skrip HTML sebelum akhirnya disimpan ke dalam sistem. Di sinilah identitas nama kelompok, anggota, dan motto ("CLI is freedom") disematkan.
![Screenshot 4: Proses Penyuntingan HTML](04-edit-html.png)

*Catatan: File `index.html` asli dilampirkan secara terpisah di dalam repositori ini.*

### Tahap 5: Konfigurasi Jaringan & Port Forwarding
Server Debian 13 beroperasi di dalam jaringan NAT (Network Address Translation) VMware. Agar halaman HTML profil kelompok bisa diakses dari *browser* yang ada di sistem Windows/Linux fisik (Host), diperlukan mekanisme **Port Forwarding**. Alamat IP pada antarmuka *guest* (`ens33`) dicek menggunakan perintah `ip a`.

#### 📌 Screenshot 5: Pengaturan NAT Port Forwarding di VMware
Melalui menu *Virtual Network Editor* di VMware, konfigurasi pemetaan lalu lintas jaringan (NAT Settings) ditambahkan. Aturan yang dibuat menetapkan bahwa seluruh koneksi yang masuk ke komputer Host pada *port* **8080** akan langsung diteruskan ke alamat IP komputer Guest (Debian) menuju *port* **80** (HTTP Web Server standar Nginx).
![Screenshot 5: Jendela Form NAT Settings Port Forwarding](05-nat-settings.png)

### Tahap 6: Pengujian Akhir (*Testing*)
Langkah pungkas dari proyek ini adalah memverifikasi keterhubungan antara *browser* dari *host machine* ke *web server* Nginx yang tertanam di *virtual machine*.

#### 📌 Screenshot 6: Verifikasi Hasil Akses Profil Kelompok Lewat Browser Komputer Host
Browser pada sistem Host (misalnya Chrome atau Edge) dibuka dan diarahkan menuju URL `http://localhost:8080`. Halaman berhasil dimuat (di-render) dengan sempurna, menampilkan baris judul `Server Debian 13 Kelompok X` serta daftar anggotanya. Ini merupakan bukti konklusif bahwa *web server*, halaman *custom* HTML, dan aturan NAT *Port Forwarding* semuanya telah berfungsi dengan baik dan benar.
![Screenshot 6: Verifikasi Hasil Akses](06-browser-host.png)

---
*Dibuat untuk memenuhi persyaratan tugas praktikum administrasi server dan infrastruktur IT.*
