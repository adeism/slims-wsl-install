# 🚀 Panduan Singkat: Menyiapkan WSL dan Ubuntu di Windows

Dokumen ini menjelaskan cara menginstal Windows Subsystem for Linux (WSL) dan distribusi Ubuntu di Windows 10/11, serta cara mengakses terminalnya. Ini adalah langkah prasyarat sebelum melakukan instalasi aplikasi Linux seperti SLiMS di lingkungan WSL.

---

## ✅ Langkah 1: Menginstal WSL (Windows Subsystem for Linux)

WSL adalah fitur inti Windows yang memungkinkan Anda menjalankan lingkungan Linux.

1.  **Buka PowerShell atau Command Prompt sebagai Administrator:**
    *   Klik kanan pada tombol Start Menu.
    *   Pilih "Windows PowerShell (Admin)" atau "Terminal (Admin)" atau "Command Prompt (Admin)".

2.  **Jalankan Perintah Instalasi:**
    *   Ketik perintah berikut di jendela Admin PowerShell/CMD dan tekan `Enter`:
        ```powershell
        wsl --install
        ```
    *   Perintah ini akan mengunduh dan menginstal komponen WSL terbaru, serta **biasanya secara default juga menginstal distribusi Ubuntu**. Tunggu prosesnya selesai.

3.  **Restart Komputer:**
    *   Setelah instalasi selesai, **restart komputer Anda** agar semua perubahan diterapkan.

---

## 🐧 Langkah 2: Menginstal Ubuntu (Jika Belum Terinstal Otomatis)

Jika perintah `wsl --install` tidak otomatis menginstal Ubuntu, atau jika Anda ingin menginstal versi tertentu:

**Pilih Salah Satu Metode Berikut:**

*   **Metode A: Melalui Microsoft Store (Cara Termudah)**
    1.  Buka **Microsoft Store** di Windows.
    2.  Cari "Ubuntu". Anda akan melihat beberapa versi (misalnya, Ubuntu 22.04 LTS, Ubuntu 20.04 LTS). Pilih salah satu (versi LTS terbaru direkomendasikan).
    3.  Klik tombol "Get" atau "Install" dan tunggu proses unduh serta instalasi selesai.

*   **Metode B: Melalui Command Line (Setelah WSL Aktif)**
    1.  Buka **PowerShell** atau **Command Prompt** (tidak perlu sebagai Admin kali ini).
    2.  Untuk melihat daftar distribusi Linux yang tersedia, ketik:
        ```powershell
        wsl --list --online
        ```
    3.  Untuk menginstal Ubuntu (misalnya versi default), ketik:
        ```powershell
        wsl --install -d Ubuntu
        ```
        (Ganti `Ubuntu` dengan nama spesifik dari daftar jika perlu, misal `Ubuntu-22.04`).

**Penting: Konfigurasi Awal Ubuntu**
*   Setelah Ubuntu terinstal (baik otomatis maupun manual), **buka aplikasi Ubuntu** dari Start Menu untuk pertama kalinya.
*   Anda akan diminta untuk menunggu beberapa saat untuk inisialisasi.
*   Kemudian, Anda akan diminta untuk **membuat username dan password baru** khusus untuk lingkungan Ubuntu ini. **Ingat baik-baik username dan password ini!** Ini berbeda dari akun Windows Anda.

---

## 💻 Langkah 3: Mengakses Terminal Ubuntu

Setelah Ubuntu terinstal dan dikonfigurasi, Anda perlu membuka terminalnya untuk menjalankan perintah Linux:

*   **Cara 1: Melalui Start Menu**
    1.  Buka **Start Menu** Windows.
    2.  Ketik "Ubuntu".
    3.  Klik aplikasi **Ubuntu** yang muncul.

*   **Cara 2: Melalui Command Prompt atau PowerShell**
    1.  Buka Command Prompt atau PowerShell.
    2.  Ketik perintah:
        ```powershell
        wsl
        ```
    3.  Tekan `Enter`. Anda akan langsung masuk ke shell default Ubuntu Anda.

Anda sekarang akan melihat prompt perintah Linux (biasanya seperti `username@NAMA_KOMPUTER:~$`), menandakan Anda sudah berada di dalam terminal Ubuntu di WSL.

---

🎉 **Anda Siap!** 🎉

Dengan WSL dan Ubuntu terinstal serta terminal dapat diakses, Anda kini siap untuk melanjutkan ke langkah berikutnya, seperti menginstal SLiMS atau perangkat lunak Linux lainnya.
