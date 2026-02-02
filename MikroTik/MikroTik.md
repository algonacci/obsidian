# BAB 1: Konfigurasi Dasar Router & Internet Gateway

**Deskripsi Modul:**
Bab ini membahas langkah fundamental dalam membangun router MikroTik dari kondisi kosong (_factory reset_) hingga berfungsi sebagai gerbang internet (Gateway) untuk jaringan lokal. Metode yang digunakan adalah _DHCP Client_ (Jalur VIP), yang mensimulasikan koneksi internet rumahan pada umumnya.

---

## 🎯 1.1 Tujuan Pembelajaran

Setelah menyelesaikan bab ini, praktikan diharapkan mampu:

1.  Melakukan _Hard Reset_ konfigurasi MikroTik.
2.  Memahami konsep IP Address WAN vs LAN.
3.  Mengkonfigurasi Router agar terhubung ke Internet.
4.  Membangun jaringan lokal (LAN) dengan DHCP Server.
5.  Mengkonfigurasi NAT (Masquerade) dan DNS Caching.

---

## 🔌 1.2 Topologi & Persiapan

**Skema Jaringan:**

- **Sumber Internet (ISP/Modem)** --[kabel]--> **Ether1 (MikroTik)**
- **Laptop/PC (Admin)** --[kabel]--> **Ether2 (MikroTik)**

**Data Teknis:**
| Interface | Peran | Konfigurasi IP | Keterangan |
| :--- | :--- | :--- | :--- |
| **ether1** | WAN (Internet) | _Dynamic (DHCP)_ | Mendapat IP dari ISP |
| **ether2** | LAN (Lokal) | `192.168.88.1/24` | Gateway untuk Laptop |

---

## 🛠️ 1.3 Langkah Kerja

### Tahap 1: The Purge (Reset Konfigurasi)

Langkah ini wajib dilakukan untuk memastikan tidak ada konfigurasi bawaan pabrik yang mengganggu.

1.  Buka aplikasi **WinBox**.
2.  Masuk ke menu **System** > **Reset Configuration**.
3.  ✅ Centang opsi **No Default Configuration** (Sangat Penting!).
4.  Klik **Reset Configuration**, lalu pilih **Yes**.
5.  Router akan _reboot_. Koneksi WinBox akan terputus.

---

### Tahap 2: Stabilisasi Akses (Static IP Laptop)

Karena router kosong (tidak ada IP), laptop sering gagal terhubung. Kita harus mengatur IP Laptop secara manual agar koneksi stabil saat konfigurasi awal.

**Untuk Pengguna Linux (GNOME):**

1.  Buka **Settings** > **Network** > **Wired** (Klik ikon Gerigi).
2.  Pindah ke tab **IPv4**.
3.  Ubah Method: **Manual**.
4.  Isi konfigurasi sementara:
    - **Address:** `192.168.88.2`
    - **Netmask:** `255.255.255.0`
    - **Gateway:** `192.168.88.1`
5.  **Apply**, lalu Matikan & Nyalakan toggle Wired (**OFF** -> **ON**).

**Akses WinBox:**

1.  Buka WinBox, klik tab **Neighbors**, lalu **Refresh**.
2.  Klik pada **MAC Address** router (contoh: `18:FD:74:...`).
3.  Login: `admin`, Password: (kosong).

---

### Tahap 3: Mengamankan Router (Security)

Sangat berbahaya membiarkan router dengan password kosong. Kita wajib mengamankannya.

1.  Masuk menu **System** > **Users**.
2.  Double Click user **admin**.
3.  Klik tombol **Password**.
4.  Isi **New Password** dan **Confirm Password** (Contoh: `1234`).
5.  Klik **OK**, lalu **OK** lagi.
6.  **Tes Login:** Tutup WinBox, lalu coba login ulang dengan password baru.

---

### Tahap 4: Konfigurasi WAN (Internet)

Menghubungkan Router ke ISP agar Router bisa "online".

1.  Masuk menu **IP** > **DHCP Client**.
2.  Klik tanda tambah **(+)**.
3.  **Interface:** Pilih `ether1`.
4.  Pastikan opsi berikut tercentang:
    - Use Peer DNS
    - Use Peer NTP
    - Add Default Route: `yes`
5.  Klik **OK**.
    > _Indikator Sukses:_ Status berubah menjadi `bound` dan kolom IP Address terisi.

---

### Tahap 5: Konfigurasi LAN (Jaringan Lokal)

Memberikan identitas (IP Address) pada router untuk sisi jaringan lokal.

1.  Masuk menu **IP** > **Addresses**.
2.  Klik tanda tambah **(+)**.
3.  **Address:** Isi `192.168.88.1/24` (Wajib menggunakan prefix /24).
4.  **Interface:** Pilih `ether2`.
5.  Klik **OK**.

---

### Tahap 6: Network Address Translation (NAT)

Logika: IP Lokal (`192.168.x.x`) tidak dikenali di internet. NAT bertugas "membungkus" paket dari laptop menggunakan IP Publik router.

1.  Masuk menu **IP** > **Firewall** > Tab **NAT**.
2.  Klik tanda tambah **(+)**.
3.  **Tab General:**
    - Chain: `srcnat`
    - Out. Interface: `ether1` (Pintu keluar ke internet).
4.  **Tab Action:**
    - Action: `masquerade`.
5.  Klik **OK**.

---

### Tahap 7: Optimasi DNS (Caching)

Menjadikan router sebagai DNS Resolver agar proses browsing lebih cepat (low latency).

1.  Masuk menu **IP** > **DNS**.
2.  ✅ Centang **Allow Remote Requests**.
3.  Klik **OK**.

---

### Tahap 8: Distribusi Otomatis (DHCP Server)

Membuat layanan agar Laptop tidak perlu setting IP manual lagi selamanya.

1.  Masuk menu **IP** > **DHCP Server**.
2.  Klik tombol **DHCP Setup** (⚠️ Jangan tombol tambah `+`).
3.  **Select interface:** `ether2` -> Next.
4.  **DHCP Address Space:** `192.168.88.0/24` -> Next.
5.  **Gateway for DHCP Network:** `192.168.88.1` -> Next.
6.  **Addresses to Give Out:** (Default) -> Next.
7.  **DNS Servers:**
    - Pastikan isinya: `192.168.88.1`.
    - _Tujuan: Memaksa laptop menggunakan fitur DNS Cache router._
8.  **Lease Time:** (Default) -> Next -> **Completed**.

---

### Tahap 9: Pengujian Client (Laptop)

Mengembalikan settingan laptop ke mode otomatis dan memvalidasi koneksi.

**Setting Laptop (Linux):**

1.  Kembali ke **Settings** > **Network** > **IPv4**.
2.  Ubah Method kembali ke **Automatic (DHCP)**.
3.  **Apply**, lalu Restart koneksi Wired (**OFF** -> **ON**).
4.  Tunggu hingga status "Connected".

**Validasi Terminal:**
Buka terminal dan jalankan perintah berikut:

```bash
# 1. Cek Koneksi ke Internet Global
ping 8.8.8.8
# Hasil: Harus Reply. Jika RTO, cek Tahap 6 (NAT).

# 2. Cek DNS Resolver
ping google.com
# Hasil: Harus Reply dengan IP Address Google.

# 3. Cek Otoritas DNS
cat /etc/resolv.conf
# Hasil: "nameserver 192.168.88.1"
```

## 📝 1.4 Kesimpulan Bab 1
Pada tahap ini, Router MikroTik telah berhasil menjalankan fungsi dasar routing:

- Menerima koneksi internet dari ISP.
- Menerjemahkan alamat IP Privat ke Publik (NAT).
- Membagikan alamat IP secara otomatis ke klien (DHCP).
- Menyediakan layanan resolusi domain (DNS).

# BAB 2: Implementasi Wireless LAN (WiFi) & Multi-Segment Network

**Deskripsi Modul:**
Bab ini membahas cara mengaktifkan antarmuka nirkabel (_Wireless Interface_) pada router hAP ac lite untuk menyediakan akses internet bagi perangkat mobile (HP/Tablet). Praktikan akan mempelajari konsep _Network Segmentation_, di mana jaringan kabel dan nirkabel memiliki blok IP yang berbeda namun tetap dapat saling berkomunikasi melalui proses _routing_ otomatis.

---

## 🎯 2.1 Tujuan Pembelajaran

Setelah menyelesaikan bab ini, praktikan diharapkan mampu:

1.  Membuat profil keamanan (_Security Profile_) WPA2.
2.  Mengkonfigurasi interface `wlan1` sebagai Access Point (AP).
3.  Menerapkan manajemen IP Address untuk segmen jaringan baru.
4.  Mengkonfigurasi DHCP Server khusus untuk jaringan WiFi.
5.  Memahami konsep _Inter-VLAN/Inter-Network Routing_ dasar.

---

## 🔌 2.2 Topologi Logis

Berbeda dengan Bab 1, sekarang Router melayani dua jaringan berbeda:

**1. Jaringan Kabel (Admin)**

- **Interface:** `ether2`
- **Network:** `192.168.88.0/24`
- **Gateway:** `192.168.88.1`

**2. Jaringan WiFi (User/Tamu)**

- **Interface:** `wlan1`
- **Network:** `192.168.20.0/24`
- **Gateway:** `192.168.20.1`

> _Catatan: Penggunaan segmen IP yang berbeda (88.x dan 20.x) bertujuan untuk memudahkan manajemen dan isolasi trafik di kemudian hari._

---

## 🛠️ 2.3 Langkah Kerja

### Tahap 1: Membuat Kunci Pengaman (Security Profile)

Sebelum menyalakan pemancar, kita wajib membuat "kunci" agar WiFi tidak bisa diakses sembarang orang (Open Network).

1.  Masuk menu **Wireless**.
2.  Pilih tab **Security Profiles**.
3.  Klik tanda tambah **(+)**.
4.  Isi konfigurasi:
    - **Name:** `profile-rahasia` (Bebas).
    - **Mode:** `dynamic keys`.
    - **Authentication Types:** Centang ✅ `WPA2 PSK` (Standar keamanan modern).
    - **WPA2 Pre-Shared Key:** Isi password yang diinginkan (Min. 8 karakter).
5.  Klik **OK**.

---

### Tahap 2: Konfigurasi Pemancar (Interface Setup)

Mengaktifkan radio WiFi 2.4GHz dan menghubungkannya dengan profil keamanan tadi.

1.  Masih di menu **Wireless**, pindah ke tab **WiFi Interfaces**.
2.  Klik baris `wlan1` (biasanya berwarna abu-abu/mati).
3.  Klik tanda **Centang Biru (Enable)** di toolbar atas untuk mengaktifkan.
4.  Klik dua kali pada `wlan1` untuk mengedit.
5.  Pilih tab **Wireless** dan atur:
    - **Mode:** `ap bridge` (Berfungsi sebagai Access Point pusat).
    - **Band:** `2GHz-B/G/N` (Kompatibilitas luas untuk HP lama & baru).
    - **SSID:** Isi nama WiFi, contoh: `MikroTik-Lab`.
    - **Security Profile:** Pilih `profile-rahasia` (yang dibuat di Tahap 1).
    - **Frequency:** `auto` (atau pilih 2412/2462 jika ingin spesifik).
6.  Klik **OK**.

---

### Tahap 3: Memberi Identitas Baru (IP Address WiFi)

Karena WiFi adalah jaringan baru, ia membutuhkan "Kepala Suku" (Gateway) sendiri. Kita tidak boleh menggunakan IP `192.168.88.1` lagi karena IP itu milik Ether2.

1.  Masuk menu **IP** > **Addresses**.
2.  Klik tanda tambah **(+)**.
3.  Isi konfigurasi:
    - **Address:** `192.168.20.1/24` (Perhatikan angka 20).
    - **Interface:** `wlan1`.
4.  Klik **OK**.

---

### Tahap 4: Pelayanan Otomatis WiFi (DHCP Server)

Agar HP yang connect langsung mendapatkan IP Address tanpa perlu setting manual.

1.  Masuk menu **IP** > **DHCP Server**.
2.  Klik tombol **DHCP Setup** (⚠️ Jangan tombol tambah `+`).
3.  **Select interface:** Pilih `wlan1` -> Next.
4.  **DHCP Address Space:** `192.168.20.0/24` -> Next.
5.  **Gateway for DHCP Network:** `192.168.20.1` -> Next.
6.  **Addresses to Give Out:** (Default) -> Next.
7.  **DNS Servers:** `192.168.20.1` -> Next.
    - _Logika: Klien WiFi juga dipaksa bertanya DNS ke Router._
8.  **Lease Time:** (Default) -> Next -> **Completed**.

---

### Tahap 5: Validasi & Pengujian Routing

Membuktikan bahwa dua jaringan berbeda (Kabel & WiFi) dapat saling berkomunikasi.

**Langkah Pengujian:**

1.  Ambil Smartphone, nyalakan WiFi.
2.  Cari SSID `MikroTik-Lab`, masukkan password.
3.  Pastikan HP statusnya "Connected" dan bisa browsing internet.
4.  Cek Detail WiFi di HP, pastikan mendapat IP `192.168.20.xxx`.

**Uji Lintas Jaringan (Inter-Network Ping):**

1.  Buka Terminal di Laptop (yang terhubung kabel).
2.  Lakukan Ping ke IP HP (lihat IP di HP tadi, misal: `192.168.20.254`).
    ```bash
    ping 192.168.20.254
    ```
3.  **Hasil:** Harus **Reply**.

---

## 📝 2.4 Analisa & Kesimpulan Bab 2

1.  **Multi-Network:** Router kini mengelola dua network ID berbeda (`88.0/24` dan `20.0/24`).
2.  **Routing Otomatis:** Meskipun berbeda segmen, Laptop dan HP bisa saling Ping. Ini terjadi karena MikroTik otomatis membuat jalur (_Directly Connected Routes_) untuk semua interface yang dimilikinya.
3.  **Single NAT:** Baik klien Kabel maupun WiFi sama-sama bisa internetan menggunakan satu aturan NAT Masquerade yang dibuat di Bab 1.

# BAB 3: Manajemen Bandwidth & Quality of Service (QoS)

**Deskripsi Modul:**
Bab ini membahas implementasi _Bandwidth Management_ menggunakan fitur **Simple Queue**. Dalam jaringan yang digunakan bersama, satu pengguna yang melakukan aktivitas berat (download game/streaming 4K) dapat menghabiskan seluruh kapasitas bandwidth, menyebabkan _lag_ pada pengguna lain. Praktikan akan belajar cara membatasi alokasi _upload_ dan _download_ untuk segmen jaringan WiFi.

---

## 🎯 3.1 Tujuan Pembelajaran

Setelah menyelesaikan bab ini, praktikan diharapkan mampu:

1.  Memahami konsep dasar _Bandwidth Limiting_ (Traffic Shaping).
2.  Membuat aturan _Simple Queue_ untuk target spesifik.
3.  Membaca indikator visual trafik (Sistem Lampu Lalu Lintas) di WinBox.
4.  Memvalidasi hasil limitasi menggunakan _Speedtest_ dan Grafik Trafik.

---

## 🔌 3.2 Skenario Implementasi

Kita akan menerapkan kebijakan "Prioritas Admin":

1.  **Jalur Kabel (Laptop Admin - 192.168.88.x):**
    - **Status:** _Unlimited_ / _Bypass_.
    - **Kecepatan:** Full Speed sesuai ISP.
2.  **Jalur WiFi (Tamu/User - 192.168.20.x):**
    - **Status:** _Limited_.
    - **Kecepatan:** Dibatasi maksimal **Upload 1 Mbps** dan **Download 2 Mbps**.

---

## 🛠️ 3.3 Langkah Kerja

### Tahap 1: Membuat Aturan Limitasi (Simple Queue)

Kita akan membuat "polisi tidur" untuk memperlambat laju data di jaringan WiFi.

1.  Masuk menu **Queues** (ikon tumpukan piring hijau).
2.  Pastikan berada di tab **Simple Queues**.
3.  Klik tanda tambah **(+)**.
4.  Isi konfigurasi pada tab **General**:
    - **Name:** `Limit-WiFi-Tamu` (Bebas).
    - **Target:** Isi Network WiFi: `192.168.20.0/24`.
      - _Artinya: Aturan ini berlaku untuk SELURUH perangkat yang terhubung ke WiFi._
    - **Max Limit (Target Upload):** Pilih atau ketik `1M`.
    - **Max Limit (Target Download):** Pilih atau ketik `2M`.
5.  Klik **OK**.

> **Tips:** Posisi rule (urutan nomor 0, 1, dst) sangat berpengaruh. Simple Queue dibaca dari atas ke bawah. Rule yang lebih spesifik sebaiknya diletakkan di posisi atas.

---

### Tahap 2: Validasi Visual (Traffic Light)

MikroTik memberikan indikator warna pada ikon Queue untuk menunjukkan beban trafik secara _real-time_.

1.  Sambungkan HP ke WiFi `MikroTik-Lab`.
2.  Buka aplikasi/web **Speedtest** (misal: fast.com) di HP.
3.  Saat Speedtest berjalan, perhatikan ikon rule `Limit-WiFi-Tamu` di WinBox:
    - 🟢 **Hijau:** Pemakaian bandwidth masih rendah (0-50%).
    - 🟡 **Kuning:** Pemakaian mendekati batas limit (51-75%).
    - 🔴 **Merah:** Pemakaian mentok di batas limit (76-100%).

---

### Tahap 3: Validasi Grafik (Traffic Graph)

Melihat bukti empiris bahwa limitasi bekerja memotong trafik ("memangkas rumput").

1.  Di WinBox (menu Queues), klik dua kali rule `Limit-WiFi-Tamu`.
2.  Pindah ke tab **Traffic**.
3.  Jalankan Speedtest lagi di HP.
4.  **Analisa Grafik:**
    - Anda akan melihat garis grafik naik lalu **mendatar rata (flat)** di angka 2 Mbps (untuk download) atau 1 Mbps (untuk upload).
    - Bentuk grafik yang rata (seperti potongan rambut cepak) menandakan bahwa paket data yang melebihi batas sedang "dibuang" (_dropped_) atau ditahan (_queued_) oleh router.

---

## 📝 3.4 Analisa & Kesimpulan Bab 3

1.  **Efektivitas Limitasi:** Pengguna WiFi tidak akan bisa menyedot bandwidth melebihi 2 Mbps, sehingga sisa bandwidth ISP aman tersimpan untuk Laptop Admin.
2.  **Targeting:** Penggunaan `Target Address` dengan format Network (`/24`) sangat efisien karena satu aturan cukup untuk mengontrol puluhan user sekaligus.
3.  **Dampak User Experience:** User yang terkena limit mungkin akan merasakan _loading_ sedikit lebih lama, namun koneksi tetap stabil asalkan limit tidak terlalu kecil (di bawah 512kbps biasanya mulai terasa lambat untuk web modern).

# BAB 4: Firewall Filter (Keamanan Dasar & Blokir Konten)

**Deskripsi Modul:**
Jika diibaratkan, **Firewall Filter** adalah "Security Guard" atau Satpam jaringan. Fitur ini bertugas menyeleksi paket data yang masuk, keluar, atau sekadar lewat di router. Anda dapat memutuskan paket mana yang boleh lewat (_Accept_) dan paket mana yang harus dibuang/diblokir (_Drop_).

---

## 🎯 4.1 Tujuan Pembelajaran

Setelah menyelesaikan bab ini, praktikan diharapkan mampu:

1.  Memahami logika **Chain** (Input vs Forward).
2.  Melindungi Router dari serangan luar (Input Chain).
3.  Memblokir akses ke situs web tertentu (Forward Chain).
4.  Memahami urutan eksekusi aturan firewall (_Rule Order_).

---

## 🔌 4.2 Konsep Dasar: The Chains

Sebelum praktik, pahami 3 jenis rantai (Chain) utama:

1.  **Chain Input:** Traffic yang **tujuannya ke Router itu sendiri**.
    - *Contoh: Login WinBox, Ping ke Router.*
2.  **Chain Forward:** Traffic yang **hanya numpang lewat** dr satu ether ke ether lain.
    - *Contoh: Laptop browsing internet (Lewat dari Ether2 ke Ether1).*
3.  **Chain Output:** Traffic yang **berasal dari Router** keluar.
    - *Contoh: Router melakukan Ping ke Google.*

---

## 🛠️ 4.3 Langkah Kerja

### Tahap 1: Eksperimen Blokir Ping (Input Chain)

Kita akan mencoba mengabaikan siapa saja yang mencoba "menyapa" (Ping) router.

1.  Buka terminal laptop, coba Ping ke Router: `ping 192.168.88.1`.
    - *Hasil Normal: Reply.*
2.  Di WinBox, masuk menu **IP** > **Firewall** > Tab **Filter Rules**.
3.  Klik tanda tambah **(+)**.
4.  **Tab General:**
    - Chain: `input`
    - Protocol: `icmp` (Protokol Ping).
5.  **Tab Action:**
    - Action: `drop` (Buang paket).
6.  Klik **OK**.
7.  **Tes:** Coba Ping lagi dari laptop.
    - *Hasil: Request Timed Out (RTO).*
    - *Kesimpulan: Router pura-pura budeg/mati.*
8.  **Matikan Rule:** Klik rule tersebut, lalu klik tombol **Silang (X)**. Tes Ping lagi (harus Reply).

---

### Tahap 2: Blokir Situs Web (Forward Chain)

Skenario: Karyawan dilarang membuka situs `linux.org` (contoh situs HTTP/Plain) saat jam kerja.

1.  Buka browser di Laptop, pastikan bisa buka `linux.org`.
2.  Kembali ke WinBox (**IP** > **Firewall** > **Filter Rules**).
3.  Klik tanda tambah **(+)**.
4.  **Tab General:**
    - Chain: `forward` (Karena trafik dari laptop mau ke internet).
    - Protocol: `tcp`.
    - Dst. Port: `80,443` (Web traffic).
5.  **Tab Advanced:**
    - Content: `linux.org`.
6.  **Tab Action:**
    - Action: `drop`.
7.  Klik **OK**.
8.  **Tes:** Coba akses kembali atau refresh `linux.org`.
    - *Hasil: Loading akan berputar terus sampai error (Connection Timed Out).*
    - *Catatan: Metode blokir `content` efektif untuk situs sederhana. Untuk situs raksasa (YouTube/FB), diperlukan metode TLS Host atau Layer7 Protocol.*

---

### Tahap 3: Logika Urutan (Order of Rules)

Firewall membaca aturan dr **ATAS ke BAWAH**. Jika paket sudah cocok dengan aturan No. 0, dia TIDAK AKAN dicek di aturan No. 1.

**Studi Kasus: Whitelist Admin** ("Hanya Admin yang boleh Ping, User lain tidak boleh").

1.  Buat Rule Baru (Rule A): **Allow Admin**.
    - Chain: `input`
    - Src. Address: `192.168.88.2` (IP Laptop Admin).
    - Protocol: `icmp`.
    - Action: `accept`.
2.  Buat Rule Baru (Rule B): **Block All**.
    - Chain: `input`
    - Protocol: `icmp`.
    - Action: `drop`.
3.  **Atur Posisi:**
    - Pastikan Rule A (Accept) berada **DI ATAS** Rule B (Drop).
    - *Logika: "Apakah ini Admin? Ya -> Silakan lewat (Selesai). Jika Bukan Admin -> Cek aturan bawahnya -> Drop."*
4.  **Tes:**
    - Dari Laptop Admin (`88.2`): Ping Reply.
    - Dari HP via WiFi (`20.x`): Ping RTO.

---

## 📝 4.4 Analisa & Kesimpulan Bab 4

1.  **Selection:** Firewall memilah paket berdasarkan parameter (IP Asal, IP Tujuan, Protokol, Port, Konten, dll).
2.  **Chain Clarity:**
    - Mau blokir akses *ke* Router? Gunakan **Input**.
    - Mau blokir akses *klien ke internet*? Gunakan **Forward**.
3.  **Hierarchy:** Urutan rule sangat krusial. Rule yang bersifat khusus (Exception/Pengecualian) harus diletakkan di **paling atas**, sedangkan rule umum (Sapujagat) di bawah.
