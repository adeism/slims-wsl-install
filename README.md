# slims-wsl-install
Installasi slims di WSL(Windows untuk Linux)



**Prasyarat:**

1.  Sudah menginstal WSL (Subsistem Windows untuk Linux) di Windows Anda.
2.  Sudah menginstal distribusi Ubuntu di dalam WSL (misalnya, Ubuntu 20.04, 22.04, atau yang terbaru).
3.  Memiliki akses ke terminal Ubuntu di WSL.

**Langkah-Langkah Instalasi (Dengan Git):**

Buka terminal Ubuntu Anda di WSL dan ikuti langkah-langkah berikut:

**Langkah 1: Perbarui Sistem**

```bash
sudo apt update
sudo apt upgrade -y
```

**Langkah 2: Instal Tumpukan LAMP (Apache, MySQL, PHP) dan Git**

Tambahkan `git` ke daftar paket yang diinstal.

```bash
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql php-cli php-gd php-intl php-xml php-curl php-mbstring git -y
```

*   `git`: Untuk mengunduh kode SLiMS dari repositori.

**Langkah 3: Amankan Instalasi MySQL**

```bash
sudo mysql_secure_installation
```

Setel password root MySQL dan jawab pertanyaan keamanan lainnya.

**Langkah 4: Buat Database dan Pengguna MySQL untuk SLiMS**

Masuk ke shell MySQL:

```bash
sudo mysql -u root -p
```

Di dalam shell MySQL, jalankan perintah berikut:

```sql
CREATE DATABASE slims_db;
CREATE USER 'slims_user'@'localhost' IDENTIFIED BY 'your_strong_password';
GRANT ALL PRIVILEGES ON slims_db.* TO 'slims_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

*Ganti* `'your_strong_password'` dengan password yang kuat.

**Langkah 5: Unduh SLiMS Senayan (v9 Bulian) Menggunakan Git**

Kita akan mengklon (clone) repositori SLiMS langsung ke direktori web root Apache. Pastikan direktori target `/var/www/html/slims` kosong atau tidak ada sebelumnya.

```bash
cd /var/www/html/
sudo git clone https://github.com/slims/slims9_bulian.git slims/
```

Ini akan membuat direktori `/var/www/html/slims/` dan mengunduh semua file dari repositori SLiMS Bulian ke dalamnya. Secara default, ini akan mengunduh branch `master`.

*Catatan:* Jika Anda ingin menginstal versi spesifik (misalnya v9.6.1), setelah `git clone`, Anda bisa masuk ke direktori `slims/` dan melakukan checkout ke tag yang diinginkan:
```bash
# (Opsional: Jika ingin versi spesifik seperti 9.6.1)
cd /var/www/html/slims/
sudo git checkout 9.6.1
# Kembali ke direktori sebelumnya jika perlu
# cd /var/www/html/
```
Untuk instalasi termudah, mengklon branch `master` langsung ke direktori target sudah cukup.

**Langkah 6: Atur Hak Akses File dan Direktori**

```bash
sudo chown -R www-data:www-data /var/www/html/slims/
sudo chmod -R 755 /var/www/html/slims/
# Berikan izin tulis untuk direktori tertentu yang dibutuhkan SLiMS
sudo chmod -R 775 /var/www/html/slims/files /var/www/html/slims/images /var/www/html/slims/repository
```
Langkah ini sama seperti sebelumnya, hanya sekarang diterapkan pada file hasil `git clone`.

**Langkah 7: Konfigurasi Apache (Enable Mod_Rewrite)**

```bash
sudo a2enmod rewrite
```

Edit file konfigurasi Apache (`sudo nano /etc/apache2/apache2.conf`) dan ubah `AllowOverride None` menjadi `AllowOverride All` di bagian `<Directory /var/www/>`.

```apache
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All # Ubah dari None menjadi All
    Require all granted
</Directory>
```
Simpan file.

Restart Apache:

```bash
sudo systemctl restart apache2
```

**Langkah 8: Konfigurasi PHP (Opsional tapi Disarankan)**

Edit file `php.ini` (misalnya `sudo nano /etc/php/8.1/apache2/php.ini`) dan setel `date.timezone`.

```ini
date.timezone = Asia/Jakarta
```
Hapus tanda titik koma (`;`). Simpan file.

Restart Apache:

```bash
sudo systemctl restart apache2
```

**Langkah 9: Akses Instalasi SLiMS melalui Browser Windows**

Buka browser di Windows dan akses `http://localhost/slims/` (atau IP WSL Anda, cek dengan `ip addr show eth0 | grep inet\ `).

**Langkah 10: Ikuti Wizard Instalasi SLiMS**

Ikuti langkah-langkah di browser seperti pada panduan sebelumnya:
*   Start Installation
*   Cek persyaratan (harusnya semua hijau)
*   Masukkan detail database (`localhost`, `slims_db`, `slims_user`, `your_strong_password`)
*   Test koneksi
*   Setel akun administrator awal
*   Selesaikan instalasi.

**Langkah 11: Hapus Direktori `install` (Penting untuk Keamanan!)**

Setelah instalasi selesai, hapus direktori `install` dari terminal WSL:

```bash
sudo rm -rf /var/www/html/slims/install/
```

**Selesai!**

Sekarang SLiMS Senayan v9 Bulian sudah terinstal di WSL Anda, diunduh langsung dari repositori Git. Metode ini sedikit lebih cepat karena tidak perlu mengunduh ZIP dan mengekstraknya secara terpisah.
