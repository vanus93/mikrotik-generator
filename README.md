# MikroTik Script Generator

Aplikasi web satu halaman untuk menyusun script konfigurasi dasar MikroTik RouterOS (`.rsc`) melalui formulir dan melihat hasilnya secara langsung.

## Membuka aplikasi

Tidak perlu instalasi atau server web. Buka `index.html` dengan browser modern. Pastikan `style.css` dan `script.js` berada di folder yang sama. Aplikasi berjalan di browser dan tidak mengirim konfigurasi ke server.

## Fitur

- Memilih port WAN dari ether1 sampai ether5.
- Mengatur WAN otomatis dengan DHCP Client atau WAN statik dengan IP, gateway, dan DNS.
- Membuat beberapa jaringan LAN dengan alamat IP dan subnet masing-masing.
- Menggabungkan beberapa port ke dalam satu jaringan menggunakan bridge, atau memakai satu port langsung tanpa bridge.
- Menghitung rentang alamat DHCP dari IP dan subnet jaringan LAN.
- Mengaktifkan atau menonaktifkan DHCP Server, pengamanan service dan waktu, serta firewall.
- Membiarkan akun `admin` bawaan tetap aktif secara default, dengan opsi untuk menonaktifkannya.
- Melihat preview script secara langsung, menyalin script, dan mengunduh `config-mikrotik.rsc`.

## Cara menggunakan

1. Isi identitas router, username admin baru, dan password.
2. Pilih port WAN dan mode koneksi. Jika memilih statik, isi IP/subnet, gateway, serta DNS.
3. Atur **Jaringan LAN**. Buat satu jaringan untuk setiap kelompok perangkat yang ingin memakai IP network yang sama.
4. Aktifkan **Bridge** jika beberapa port perlu berada di jaringan yang sama. Pilih port-port tersebut. Jika bridge dimatikan, pilih satu port untuk jaringan itu.
5. Atur IP router/gateway untuk setiap jaringan LAN. Tambah atau hapus jaringan sesuai kebutuhan.
6. Pilih fitur tambahan yang ingin disertakan.
7. Tinjau seluruh preview, lalu salin atau unduh file `.rsc`.

### Contoh pembagian port

- ether2 dan ether3 dalam satu bridge dengan IP `192.168.10.1/24`.
- ether4 tanpa bridge dengan IP `192.168.20.1/24`.
- ether5 tanpa bridge dengan IP `192.168.30.1/24`.

Perangkat pada setiap jaringan mendapat rentang IP DHCP tersendiri jika DHCP diaktifkan. Pastikan port tidak dipakai oleh WAN atau lebih dari satu jaringan, dan subnet antarjaringan tidak bertabrakan.

## Isi konfigurasi yang dihasilkan

Script menyusun konfigurasi berikut secara berurutan:

1. Identity dan user admin baru. Akun `admin` bawaan tetap aktif, kecuali opsi penonaktifan dipilih.
2. (Jika dipilih) Menonaktifkan service Telnet, FTP, WWW, API, dan API-SSL, serta mengatur SNTP dan zona waktu Jakarta.
3. Bridge dan port anggotanya jika bridge diaktifkan, lalu IP untuk setiap jaringan LAN.
4. WAN melalui DHCP Client atau konfigurasi statik. Mode statik menambahkan default route melalui gateway yang dimasukkan.
5. (Jika dipilih) NAT masquerade, FastTrack, dan aturan blokir DNS dari port WAN.
6. (Jika DHCP diaktifkan) IP pool, DHCP Server, dan DHCP network untuk setiap jaringan LAN yang valid.

Pada mode DHCP, default route diperoleh dari DHCP server WAN. Script tidak menambahkan route statik dalam mode tersebut.

## Sebelum menerapkan script

- Periksa preview dan sesuaikan nama interface dengan model router serta konfigurasi yang sudah ada.
- Gunakan subnet unik untuk setiap jaringan LAN dan pastikan gateway berada di dalam subnetnya.
- Periksa rentang DHCP, terutama pada subnet kecil. Generator menerima prefix `/1` sampai `/30` dan memakai rentang mulai dari alamat network + 2 hingga broadcast - 1.
- Jika memilih menonaktifkan akun `admin` bawaan, uji login menggunakan user baru terlebih dahulu agar tidak kehilangan akses.
- Script ini membangun konfigurasi dasar. Tinjau kebijakan firewall dan kebutuhan akses manajemen sebelum menerapkannya pada jaringan produksi.
- Uji terlebih dahulu pada router uji atau gunakan Safe Mode/akses lokal agar perubahan jaringan tidak mengunci akses ke router.

## Berkas

- `index.html` — struktur halaman aplikasi.
- `style.css` — tampilan dan tata letak.
- `script.js` — generator script dan interaksi formulir.
- `README.md` — panduan ini.

Untuk GitHub Pages, unggah keempat file tersebut ke folder utama repository. Pastikan nama file tetap sama agar `index.html` dapat memuat `style.css` dan `script.js` melalui path relatif.
