# Praktikum Instalasi Debian 13 (Mode Teks/Headless) dan Web Server Nginx

## Deskripsi Proyek
Repositori ini berisi panduan dan bukti pengerjaan instalasi sistem operasi Debian 13 dalam mode teks (headless) menggunakan VMware Workstation. Proyek ini juga mencakup instalasi dan konfigurasi web server Nginx, pembuatan halaman statis profil kelompok, serta konfigurasi NAT Port Forwarding agar server virtual (Guest) dapat diakses melalui browser pada sistem operasi Host.

---

## Langkah Pengerjaan dan Dokumentasi Screenshot

### 1. Pemilihan Perangkat Lunak (Software Selection)
Pada tahap akhir instalasi sistem dasar Debian, instalasi dikonfigurasi sebagai server murni tanpa antarmuka grafis (GUI). Pada menu pemilihan perangkat lunak, tanda bintang (*) dikosongkan pada opsi desktop environment. Tanda bintang (*) hanya diaktifkan untuk komponen **SSH Server** dan **standard system utilities**.

![Screenshot 1: Software Selection](01-software-selection.png)

### 2. Login Pertama Kali Sistem Operasi Debian 13
Setelah instalasi selesai dan sistem di-reboot, bukti otentikasi login pertama kali direkam menggunakan akun pengguna biasa (user non-root) yaitu `kelompok1`. Proses login dilakukan pada mesin dengan hostname `server-SI2025A`.

![Screenshot 2: Prompt Login Pertama Kali](02-debian-login.png)

### 3. Pemeriksaan Status Operasional Service Daemon Nginx
Setelah masuk kembali ke dalam sistem dan Nginx terinstal, layanan web server Nginx dijalankan. Verifikasi operasional dilakukan melalui perintah `systemctl status nginx`. Visualisasi antarmuka terminal menampilkan keluaran (output) log systemctl yang memiliki indikator teks berwarna hijau bertuliskan **active (running)**.

![Screenshot 3: Status Operasional Nginx](03-nginx-status.png)

### 4. Proses Penyuntingan File HTML via Editor Teks Nano
Halaman web profil kelompok dibuat dengan mengedit file statis utama di `/var/www/html/index.html`. Layar terminal mendokumentasikan editor teks Nano yang memuat baris-baris kode HTML sebelum modifikasi berkas tersebut disimpan. 

![Screenshot 4: Edit HTML via Nano](04-edit-html.png)

### 5. Jendela Form Aturan NAT Settings Port Forwarding VMware
Agar server web pada sistem operasi Guest dapat diakses dari Windows Host, dikonfigurasi fitur Port Forwarding. Tangkapan layar ini menunjukkan dialog box NAT Settings VMware Workstation Pro yang merekam pengaturan pemetaan dari **Host Port 8080** yang diarahkan menuju alamat IP Guest Debian pada **Virtual Machine Port 80**.

![Screenshot 5: NAT Settings Port Forwarding](05-nat-settings.png)

### 6. Verifikasi Hasil Akses Profil Kelompok Lewat Browser Komputer Host
Tahap terakhir adalah pengujian. Bukti konklusif didapat berupa halaman profil "Kelompok 1 - SI2025A" yang berhasil dirender melalui browser pada komputer Host. Akses dilakukan dengan membuka URL `http://localhost:8080`.

![Screenshot 6: Akses Browser Host](06-browser-host.png)

---

## Kode Sumber dan Struktur Repositori
Pastikan direktori repositori ini memuat file-file berikut:
1. `README.md` : Panduan dokumentasi proyek ini.
2. `index.html` : Berkas sumber asli yang berisikan kode HTML profil kelompok.
3. Berkas gambar screenshot (`01-software-selection.png` hingga `06-browser-host.png`).
