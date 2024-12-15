# Building counter strike 1.6 Server - Final Project OS Server dan Sistem Admin 23.83.0991

### **Ringkasan Layanan yang Digunakan**
| **Layanan**   | **Fungsi**                                         |
|---------------|---------------------------------------------------|
| **SSH**       | Akses jarak jauh untuk pengelolaan server.         |
| **FTP**       | Upload file, seperti peta atau plugin server.      |
| **Web Server**| Menampilkan status server atau halaman info.       |
| **MySQL**     | Menyimpan data statistik pemain (opsional).        |
| **Firewall**  | Melindungi server dari akses yang tidak sah.       |


### Berikut adalah spesifikasi yang dapat diperoleh dari setup Counter-Strike 1.6 server yang dijelaskan sebelumnya, termasuk kebutuhan sistem minimum dan detail parameter server:

Spesifikasi Sistem Minimum (Server CS 1.6)
Sistem Operasi:

Ubuntu Server 18.04 atau lebih baru (bisa berjalan pada distribusi Linux lain).
Prosesor (CPU):

Minimal: Intel Pentium III atau setara (1 GHz atau lebih).
Disarankan: Intel Core i3 atau setara untuk performa lebih stabil dengan lebih banyak pemain.
Memori (RAM):

Minimal: 512 MB RAM (cukup untuk 10-12 pemain).
Disarankan: 1-2 GB RAM (untuk 20-32 pemain).
Ruang Penyimpanan (Storage):

Minimal: 2 GB ruang kosong (untuk file server dasar).
Disarankan: 5-10 GB (untuk peta tambahan, plugin, dan log server).
Koneksi Internet:

Minimal: 1 Mbps Upload (cukup untuk 8-10 pemain dengan ping stabil).
Disarankan: 10 Mbps Upload (untuk 32 pemain).
Catatan: Latensi rendah (<50 ms) lebih penting dibandingkan kecepatan bandwidth.

### **1. SSH (Secure Shell)**
SSH digunakan untuk mengelola server dari jarak jauh.

#### Langkah:
1. **Install OpenSSH Server**:
   ```bash
   sudo apt update
   sudo apt install openssh-server
   ```
2. **Konfigurasi SSH**:
   - File konfigurasi ada di `/etc/ssh/sshd_config`.
   - Pastikan SSH aktif:
     ```bash
     sudo systemctl enable ssh
     sudo systemctl start ssh
     ```
3. **Akses dari komputer lain**:
   ```bash
   ssh username@ip_address_server
   ```

---

### **2. FTP (File Transfer Protocol)**
FTP digunakan untuk mengunggah dan mengunduh file ke/dari server.

#### Langkah:
1. **Install vsftpd**:
   ```bash
   sudo apt install vsftpd
   ```
2. **Konfigurasi vsftpd**:
   - Edit file `/etc/vsftpd.conf` dan pastikan baris berikut aktif:
     ```bash
     write_enable=YES
     local_enable=YES
     ```
   - Restart layanan:
     ```bash
     sudo systemctl restart vsftpd
     ```
3. **Akses FTP**:
   - Gunakan aplikasi FTP client seperti FileZilla untuk mengunggah file ke server.

---

### **3. Web Server**
Web server digunakan untuk menampilkan informasi server (status server, player, dll.).

#### Langkah:
1. **Install Apache**:
   ```bash
   sudo apt install apache2
   ```
2. **Buat Halaman Info Server**:
   - Buat file di `/var/www/html/index.html`:
     ```html
     <html>
     <head><title>Server Counter-Strike 1.6</title></head>
     <body>
       <h1>Status Server</h1>
       <p>Server CS 1.6 sedang berjalan!</p>
     </body>
     </html>
     ```
   - Restart Apache:
     ```bash
     sudo systemctl restart apache2
     ```
3. **Akses halaman web**:
   - Buka browser dan akses `http://ip_address_server`.

---

### **4. MySQL (Database Server)**
MySQL digunakan untuk menyimpan statistik pemain (opsional).

#### Langkah:
1. **Install MySQL**:
   ```bash
   sudo apt install mysql-server
   ```
2. **Konfigurasi MySQL**:
   - Masuk ke MySQL:
     ```bash
     sudo mysql
     ```
   - Buat database:
     ```sql
     CREATE DATABASE cs_stats;
     ```
   - Buat user:
     ```sql
     CREATE USER 'cs_user'@'localhost' IDENTIFIED BY 'password';
     GRANT ALL PRIVILEGES ON cs_stats.* TO 'cs_user'@'localhost';
     FLUSH PRIVILEGES;
     ```
3. **Gunakan plugin AMX Mod X**:
   - AMX Mod X memiliki plugin yang dapat menyimpan statistik pemain ke MySQL.

---

### **5. Firewall**
Firewall digunakan untuk melindungi server dari akses yang tidak diinginkan.

#### Langkah:
1. **Install dan Konfigurasi UFW**:
   ```bash
   sudo apt install ufw
   ```
2. **Buka Port yang Diperlukan**:
   - Counter-Strike 1.6 default port: **27015**.
   - SSH default port: **22**.
   - FTP port: **21**.
   - Web server port: **80**.
   ```bash
   sudo ufw allow 22/tcp
   sudo ufw allow 21/tcp
   sudo ufw allow 80/tcp
   sudo ufw allow 27015/udp
   sudo ufw enable
   ```
3. **Periksa Status**:
   ```bash
   sudo ufw status
   ```

---

### **Setup Counter-Strike 1.6 Server**

#### 1. Download dan Setup HLDS (Half-Life Dedicated Server)
1. Buat folder untuk server:
   ```bash
   mkdir ~/cs16server
   cd ~/cs16server
   ```
2. Download HLDS binary untuk Linux dari sumber resmi atau gunakan tool alternatif seperti [hldsupdatetool](https://github.com/dgibbs64/linuxgsm) untuk mendownload file server.

3. Ekstrak dan jalankan server:
   ```bash
   ./hlds_run -game cstrike +maxplayers 16 +map de_dust2 -port 27015
   ```

#### 2. Install Plugin AMX Mod X
1. Download AMX Mod X dari situs resminya.
2. Ekstrak dan pasang plugin ke dalam folder `addons/amxmodx` pada server.

---

### **Menguji Server**
1. **Jalankan Server**:
   ```bash
   ./hlds_run -game cstrike +maxplayers 16 +map de_dust2 -port 27015
   ```
2. **Akses Server**:
   - Gunakan client Counter-Strike 1.6 dan masukkan IP server.

---

DATE 12-15-2024
