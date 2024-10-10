# manajemen-basisdata-2


|Nama | NIM | Kelas |
|-|-|-|
|Aditya Putra Wijaya | 312210207 | TI 22 A2 |

# Membuat databse management_hotel dan melakukan aksi
## 1. Membuat database

### Membuat Database

CREATE DATABASE management_hotel

### Membuat table kamar

CREATE TABLE `kamar` (
  `id_kamar` int(11) NOT NULL,
  `nomor_kamar` varchar(10) NOT NULL,
  `tipe_kamar` varchar(50) DEFAULT NULL,
  `harga_per_malam` decimal(10,2) DEFAULT NULL,
  `status` varchar(20) DEFAULT 'tersedia'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

### Membuat table pelanggan

CREATE TABLE `pelanggan` (
  `id_pelanggan` int(11) NOT NULL,
  `nama` varchar(100) NOT NULL,
  `alamat` varchar(255) DEFAULT NULL,
  `no_telepon` varchar(20) DEFAULT NULL,
  `email` varchar(100) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

### Membuat table pembayaran

CREATE TABLE `pembayaran` (
  `id_pembayaran` int(11) NOT NULL,
  `id_reservasi` int(11) DEFAULT NULL,
  `tanggal_pembayaran` date DEFAULT NULL,
  `jumlah` decimal(10,2) DEFAULT NULL,
  `metode_pembayaran` varchar(50) DEFAULT NULL,
  `status_pembayaran` varchar(20) DEFAULT 'pending'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

### Membuat table reservasi

CREATE TABLE `reservasi` (
  `id_reservasi` int(11) NOT NULL,
  `id_pelanggan` int(11) DEFAULT NULL,
  `id_kamar` int(11) DEFAULT NULL,
  `tanggal_checkin` date DEFAULT NULL,
  `tanggal_checkout` date DEFAULT NULL,
  `total_harga` decimal(10,2) DEFAULT NULL,
  `status_reservasi` varchar(50) DEFAULT 'dipesan'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


## 2. Melakukan aksi terhadap database
### 1. Laporan Semua Kamar dan Statusnya

SELECT id_kamar, nomor_kamar, tipe_kamar, harga_per_malam, status
FROM kamar
ORDER BY nomor_kamar;

![task 1](https://github.com/user-attachments/assets/f87a3356-5163-42dc-acfc-2ac5d9d3e10f)

### 2. Laporan Reservasi yang Sedang Berlangsung

SELECT r.id_reservasi, p.nama AS nama_pelanggan, k.nomor_kamar, r.tanggal_checkin,
r.tanggal_checkout, r.total_harga, r.status_reservasi
FROM reservasi r
JOIN pelanggan p ON r.id_pelanggan = p.id_pelanggan
JOIN kamar k ON r.id_kamar = k.id_kamar
WHERE r.status_reservasi = 'dipesan' AND CURDATE() BETWEEN r.tanggal_checkin AND
r.tanggal_checkout;

![task 2](https://github.com/user-attachments/assets/5f61bf39-cfb6-4d13-922c-dd069a6642b2)

### 3. Laporan Riwayat Pembayaran Pelanggan

SELECT p.id_pembayaran, p.id_reservasi, pel.nama AS nama_pelanggan,
p.tanggal_pembayaran, p.jumlah, p.metode_pembayaran, p.status_pembayaran
FROM pembayaran p
JOIN reservasi r ON p.id_reservasi = r.id_reservasi
JOIN pelanggan pel ON r.id_pelanggan = pel.id_pelanggan
ORDER BY p.tanggal_pembayaran DESC;

![task 3](https://github.com/user-attachments/assets/00905c54-f90c-4720-acca-ce47f1159882)

### 4. Laporan Pendapatan dari Reservasi

SELECT SUM(p.jumlah) AS total_pendapatan
FROM pembayaran p
WHERE p.status_pembayaran = 'selesai';

![task4](https://github.com/user-attachments/assets/60727132-273f-472e-a0ae-918a01c8870e)

### 5.Laporan Ketersediaan Kamar Berdasarkan Tipe

SELECT tipe_kamar, COUNT(*) AS jumlah_kamar_tersedia
FROM kamar
WHERE status = 'tersedia'
GROUP BY tipe_kamar;

![task5](https://github.com/user-attachments/assets/4b5c4f1c-900a-4955-854f-8c57ef753712)
