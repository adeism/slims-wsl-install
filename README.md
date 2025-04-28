
# 🚀 Instalasi SLiMS di WS
Panduan ini akan memandu Anda menginstal **SLiMS (Senayan Library Management System)** menggunakan **WSL (Windows Subsystem for Linux)** dengan metode **Git**, membuatnya lebih mudah untuk mendapatkan versi terbaru.

---

### 📋 **Prasyarat Wajib:**

Sebelum memulai, pastikan Anda sudah:

*   ✅ Menginstal WSL di Windows.
*   ✅ Menginstal distribusi Ubuntu (misalnya, 20.04, 22.04+) di WSL.
*   ✅ Memiliki akses ke terminal Ubuntu di WSL.

---

### 🛠️ **Langkah-Langkah Instalasi (Metode Git):**

Buka terminal Ubuntu Anda dan ikuti petualangan instalasi ini!

**1️⃣ ⬆️ Perbarui Sistem Anda**
*   Jaga sistem Ubuntu Anda tetap *fresh* dan aman!

    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```

**2️⃣ 📦 Instal Kebutuhan Utama (LAMP + Git)**
*   Kita butuh Web Server (Apache), Database (MySQL), Bahasa Pemrograman (PHP), dan tentu saja Git.

    ```bash
    sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql php-cli php-gd php-intl php-xml php-curl php-mbstring git -y
    ```
    *   `git`: Alat keren untuk mengunduh kode SLiMS langsung dari sumbernya.

**3️⃣ 🔒 Amankan Instalasi MySQL**
*   Lindungi database Anda dari akses yang tidak diinginkan.

    ```bash
    sudo mysql_secure_installation
    ```
    *   Ikuti petunjuknya: setel password `root` MySQL yang kuat dan jawab pertanyaan keamanan.

**4️⃣ 🗄️ Buat Database & Pengguna SLiMS**
*   Siapkan "rumah" khusus untuk data SLiMS di MySQL.

    Masuk ke MySQL:
    ```bash
    sudo mysql -u root -p
    ```
    *   Masukkan password root MySQL yang baru saja Anda buat.

    Jalankan perintah SQL berikut (ganti `your_strong_password`!):
    ```sql
    CREATE DATABASE slims_db;
    CREATE USER 'slims_user'@'localhost' IDENTIFIED BY 'your_strong_password'; -- <-- Ganti dengan password kuat!
    GRANT ALL PRIVILEGES ON slims_db.* TO 'slims_user'@'localhost';
    FLUSH PRIVILEGES;
    EXIT;
    ```

**5️⃣ 📥 Unduh SLiMS 9 Bulian via Git**
*   Ambil kode SLiMS terbaru langsung dari repositori GitHub ke direktori web server.

    ```bash
    cd /var/www/html/
    # Pastikan direktori 'slims' belum ada atau kosong
    sudo git clone https://github.com/slims/slims9_bulian.git slims/
    ```
    *   Ini akan membuat folder `/var/www/html/slims/` dan mengisinya dengan file SLiMS.

    > **💡 Tips:** Ingin versi spesifik (misal v9.6.1)? Setelah `clone`, lakukan:
    > ```bash
    > # (Opsional) Pindah ke direktori SLiMS
    > cd /var/www/html/slims/
    > # Checkout versi yang diinginkan
    > sudo git checkout 9.6.1
    > # Kembali jika perlu
    > # cd /var/www/html/
    > ```
    > Untuk instalasi standar, `git clone` saja sudah cukup.

**6️⃣ 🛡️ Atur Hak Akses File**
*   Berikan izin yang tepat agar web server (Apache) bisa membaca dan menulis file SLiMS.

    ```bash
    sudo chown -R www-data:www-data /var/www/html/slims/
    sudo chmod -R 755 /var/www/html/slims/
    # Izin tulis khusus untuk direktori tertentu:
    sudo chmod -R 775 /var/www/html/slims/files /var/www/html/slims/images /var/www/html/slims/repository
    ```

**7️⃣ 🔧 Konfigurasi Apache (Mod_Rewrite)**
*   Aktifkan fitur penting agar URL SLiMS lebih rapi.

    ```bash
    sudo a2enmod rewrite
    ```
    Edit file konfigurasi Apache:
    ```bash
    sudo nano /etc/apache2/apache2.conf
    ```
    Cari bagian `<Directory /var/www/>` dan ubah `AllowOverride None` menjadi `AllowOverride All`:
    ```apache
    <Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride All # <-- Pastikan ini All!
        Require all granted
    </Directory>
    ```
    *   Simpan file (Ctrl+O, Enter) dan keluar (Ctrl+X).

    Restart Apache agar perubahan diterapkan:
    ```bash
    sudo systemctl restart apache2
    ```

**8️⃣ ⚙️ Konfigurasi PHP (Opsional, tapi Penting!)**
*   Atur zona waktu agar sesuai dengan lokasi Anda.

    Cek versi PHP Anda (misal, outputnya 8.1, 8.2, 8.3):
    ```bash
    php -v
    ```
    Edit file `php.ini` (ganti `8.x` dengan versi Anda):
    ```bash
    sudo nano /etc/php/8.x/apache2/php.ini # <-- Ganti 8.x
    ```
    Cari baris `;date.timezone =`, hapus tanda titik koma (`;`) di awal, dan setel zona waktu:
    ```ini
    date.timezone = Asia/Jakarta
    ```
    *   Simpan file (Ctrl+O, Enter) dan keluar (Ctrl+X).

    Restart Apache lagi:
    ```bash
    sudo systemctl restart apache2
    ```

**9️⃣ 🖥️ Mulai Instalasi via Browser**
*   Buka browser favorit Anda di Windows.
*   Kunjungi alamat: `http://localhost/slims/`
    *   Jika `localhost` tidak berfungsi, cari tahu IP WSL Anda dengan perintah `ip addr show eth0 | grep "inet\ "` di terminal Ubuntu, lalu gunakan IP tersebut (misal: `http://172.x.x.x/slims/`).

**🔟 ✨ Ikuti Wizard Instalasi SLiMS**
*   Sekarang bagian yang mudah di browser:
    1.  Klik **"Start Installation"**.
    2.  Periksa persyaratan sistem (seharusnya semua hijau ✅).
    3.  Masukkan detail database yang Anda buat di Langkah 4:
        *   Database Host: `localhost`
        *   Database Name: `slims_db`
        *   Database Username: `slims_user`
        *   Database Password: `your_strong_password` (yang Anda setel)
    4.  Klik **"Test Connection"**. Seharusnya berhasil!
    5.  Buat akun **administrator** SLiMS (username dan password baru untuk login SLiMS).
    6.  Klik **"Run the Installation"** dan tunggu sebentar.

**1️⃣1️⃣ ⚠️ Hapus Direktori `install` (WAJIB!)**
*   **SANGAT PENTING UNTUK KEAMANAN!** Setelah instalasi selesai, segera hapus folder `install`.

    Kembali ke terminal Ubuntu WSL:
    ```bash
    sudo rm -rf /var/www/html/slims/install/
    ```

---

### 🔄 **Opsi: Memulihkan Database yang Sudah Ada**

Punya backup database SLiMS (`.sql` atau `.sql.gz`)? Anda bisa memulihkannya *setelah* membuat database kosong di Langkah 4 dan *sebelum* mengakses wizard instalasi di Langkah 9.

Ganti detail berikut dengan milik Anda:
*   `/path/to/your/backup.sql.gz`: Lokasi file backup `.gz` Anda.
*   `/path/to/your/backup.sql`: Lokasi file backup `.sql` Anda.
*   `slims_user`: Nama pengguna database SLiMS Anda.
*   `slims_db`: Nama database SLiMS Anda.

**Untuk file `.sql.gz`:**
```bash
zcat /path/to/your/backup.sql.gz | mysql -u slims_user -p -h localhost slims_db
```

**Untuk file `.sql`:**
```bash
mysql -u slims_user -p -h localhost slims_db < /path/to/your/backup.sql
```
*   Anda akan diminta memasukkan password `slims_user`.

---

## 🎉 **Selesai! Selamat!** 👍

SLiMS Senayan v9 Bulian kini berhasil terinstal di WSL Anda menggunakan Git. Metode ini tidak hanya cepat tetapi juga memudahkan jika Anda ingin memperbarui SLiMS di kemudian hari (`git pull`).

Selamat mengelola perpustakaan Anda!
