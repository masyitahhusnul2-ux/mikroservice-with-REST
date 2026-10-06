# Monolith Microservices

Project praktikum **Paradigma Sistem untuk IT** yang membahas perbandingan arsitektur **Monolith** dan **Microservices** menggunakan Python, Flask, dan Requests.

Project ini terdiri dari aplikasi Monolith, Book Service, dan Order Service. Pada arsitektur Microservices, Book Service dan Order Service berjalan secara terpisah dan berkomunikasi menggunakan HTTP Request.

---

## 1. Teknologi yang Digunakan

- Python
- Flask
- Requests
- Visual Studio Code
- Postman
- Git dan GitHub

---

## 2. Struktur Folder

Struktur folder project:

```text
Monolith Microservices/
│
├── monolith_app.py
├── book_service.py
├── order_service.py
└── README.md
```

Keterangan:

- `monolith_app.py` → aplikasi Monolith.
- `book_service.py` → service untuk mengelola data buku.
- `order_service.py` → service untuk mengelola pesanan dan berkomunikasi dengan Book Service.
- `README.md` → dokumentasi project.

---

# 3. Persiapan

Pastikan Python sudah terinstal pada komputer.

Untuk mengecek Python, buka **Command Prompt, PowerShell, atau Terminal Visual Studio Code**, kemudian jalankan:

```bash
python --version
```

Jika Python sudah terinstal, akan muncul versi Python.

Contoh:

```text
Python 3.14.4
```

Install library yang dibutuhkan:

```bash
pip install Flask requests
```

Untuk memastikan Flask sudah terinstal:

```bash
pip show Flask
```

Untuk memastikan Requests sudah terinstal:

```bash
pip show requests
```

---

# 4. Membuka Project

Buka folder project menggunakan Visual Studio Code.

Folder project:

```text
Monolith Microservices
```

Pastikan di dalam folder terdapat:

```text
monolith_app.py
book_service.py
order_service.py
README.md
```

---

# BAGIAN 1 — MONOLITH

## 5. Menjalankan Monolith

Buka terminal pada Visual Studio Code.

Jika posisi terminal masih berada di:

```text
D:\semester 5\Paradigma sistem untuk IT
```

masuk ke folder project dengan perintah:

```powershell
cd "Monolith Microservices"
```

Setelah itu posisi terminal menjadi:

```text
PS D:\semester 5\Paradigma sistem untuk IT\Monolith Microservices>
```

Kemudian jalankan:

```powershell
python monolith_app.py
```

Jika berhasil, akan muncul:

```text
Running on http://127.0.0.1:5000
```

Artinya aplikasi Monolith berjalan pada port **5000**.

Alamat:

```text
http://localhost:5000
```

> Jangan tutup terminal yang sedang menjalankan `monolith_app.py` selama proses pengujian.

---

# 6. Pengujian Monolith Menggunakan Postman

## 6.1 Mengecek Data Buku

Buka Postman.

Pilih:

```text
GET
```

Masukkan URL:

```text
http://localhost:5000/books
```

Kemudian klik **Send**.

Hasil yang diharapkan:

```json
[
    {
        "id": 1,
        "stock": 5,
        "title": "Belajar Flask"
    }
]
```

---

## 6.2 Membuat Pesanan

Pada Postman pilih:

```text
POST
```

Masukkan URL:

```text
http://localhost:5000/orders
```

Kemudian pilih:

```text
Body
→ raw
→ JSON
```

Masukkan:

```json
{
    "book_id": 1
}
```

Klik **Send**.

Hasil yang diharapkan:

```json
{
    "id": 1,
    "book_id": 1,
    "status": "berhasil"
}
```

Pada tahap ini, fitur Buku dan Pesanan masih berada dalam satu aplikasi Monolith.

---

# BAGIAN 2 — BOOK SERVICE

## 7. Menjalankan Book Service

Buka **terminal baru**.

Jika posisi terminal berada di:

```text
D:\semester 5\Paradigma sistem untuk IT
```

masuk ke folder project:

```powershell
cd "Monolith Microservices"
```

Pastikan posisi terminal menjadi:

```text
PS D:\semester 5\Paradigma sistem untuk IT\Monolith Microservices>
```

Kemudian jalankan:

```powershell
python book_service.py
```

Jika berhasil, akan muncul:

```text
Running on http://127.0.0.1:5001
```

Book Service berjalan pada port **5001**.

Alamat:

```text
http://localhost:5001
```

> Jangan tutup terminal Book Service karena Order Service membutuhkan Book Service untuk melakukan komunikasi.

---

# 8. Pengujian Book Service Menggunakan Postman

## 8.1 Menampilkan Semua Buku

Pilih:

```text
GET
```

Masukkan URL:

```text
http://localhost:5001/books
```

Klik **Send**.

Hasil:

```json
[
    {
        "id": 1,
        "stock": 5,
        "title": "Belajar Flask"
    }
]
```

---

## 8.2 Menampilkan Buku Berdasarkan ID

Pilih:

```text
GET
```

Masukkan URL:

```text
http://localhost:5001/books/1
```

Klik **Send**.

Hasil:

```json
{
    "id": 1,
    "stock": 5,
    "title": "Belajar Flask"
}
```

---

# BAGIAN 3 — ORDER SERVICE

## 9. Menjalankan Order Service

Pastikan **Book Service masih berjalan pada port 5001**.

Buka **terminal baru**.

Jika posisi terminal berada di:

```text
D:\semester 5\Paradigma sistem untuk IT
```

masuk ke folder project:

```powershell
cd "Monolith Microservices"
```

Kemudian jalankan:

```powershell
python order_service.py
```

Jika berhasil, akan muncul:

```text
Running on http://127.0.0.1:5002
```

Order Service berjalan pada port **5002**.

Alamat:

```text
http://localhost:5002
```

Sekarang terdapat dua service yang berjalan:

```text
Book Service  → Port 5001
Order Service → Port 5002
```

---

# 10. Pengujian Komunikasi Antar-Service

Buka Postman.

Pilih:

```text
POST
```

Masukkan URL:

```text
http://localhost:5002/orders
```

Kemudian pilih:

```text
Body
→ raw
→ JSON
```

Masukkan:

```json
{
    "book_id": 1
}
```

Klik **Send**.

Jika Book Service berjalan dan buku tersedia, hasilnya:

```json
{
    "id": 1,
    "book_id": 1,
    "status": "berhasil"
}
```

Proses komunikasi antar-service:

```text
Client
   │
   │ POST /orders
   ▼
Order Service
Port 5002
   │
   │ HTTP Request
   │ GET /books/1
   ▼
Book Service
Port 5001
   │
   ▼
Data Buku
```

Order Service meminta informasi buku kepada Book Service menggunakan HTTP Request.

---

# BAGIAN 4 — EKSPERIMEN FAULT ISOLATION

## 11. Mematikan Book Service

Eksperimen dilakukan untuk mengetahui kondisi sistem ketika salah satu service mengalami kegagalan.

Cari terminal yang sedang menjalankan:

```text
python book_service.py
```

Kemudian tekan:

```text
CTRL + C
```

Book Service akan berhenti.

Kondisinya menjadi:

```text
Book Service
Port 5001
DOWN
```

Sedangkan Order Service tetap berjalan:

```text
Order Service
Port 5002
RUNNING
```

---

# 12. Mengirim Pesanan Kembali

Buka Postman.

Pilih:

```text
POST
```

Masukkan:

```text
http://localhost:5002/orders
```

Pada bagian Body pilih:

```text
raw
→ JSON
```

Masukkan:

```json
{
    "book_id": 1
}
```

Klik **Send**.

Hasil yang diharapkan:

```json
{
    "error": "Book Service sedang down!"
}
```

Hal tersebut menunjukkan bahwa **Order Service tetap berjalan meskipun Book Service mengalami kegagalan**.

Order Service masih dapat memberikan respons kepada pengguna, tetapi tidak dapat memproses pesanan karena tidak dapat berkomunikasi dengan Book Service.

---

# 13. Konsep Fault Isolation

Eksperimen tersebut menunjukkan konsep **Fault Isolation**, yaitu kegagalan pada satu service tidak menyebabkan service lainnya ikut berhenti secara keseluruhan.

Kondisinya:

```text
Book Service
Port 5001
    ↓
   DOWN

Order Service
Port 5002
    ↓
  RUNNING
```

Dengan Microservices, setiap service dapat berjalan secara terpisah.

---

# BAGIAN 5 — PERBANDINGAN MONOLITH DAN MICROSERVICES

## 14. Arsitektur Monolith

Pada arsitektur Monolith, fitur Buku dan Pesanan berada dalam satu aplikasi.

```text
┌──────────────────────────────┐
│        MONOLITH APP          │
│                              │
│   ┌────────┐   ┌─────────┐   │
│   │  Buku  │   │ Pesanan │   │
│   └────────┘   └─────────┘   │
│                              │
└──────────────────────────────┘
              │
              ▼
           Port 5000
```

Jika terjadi crash atau bug fatal pada aplikasi, fitur lainnya juga dapat ikut terganggu.

---

## 15. Arsitektur Microservices

Pada arsitektur Microservices, aplikasi dibagi menjadi beberapa service.

```text
┌─────────────────┐
│  Book Service   │
│    Port 5001    │
└────────┬────────┘
         │
         │ HTTP Request
         ▼
┌─────────────────┐
│ Order Service   │
│    Port 5002    │
└─────────────────┘
```

Book Service dan Order Service berjalan secara terpisah.

Jika salah satu service mengalami kegagalan, service lainnya tetap dapat berjalan.

---

# BAGIAN 6 — BAHAN DISKUSI

## 16. Pertanyaan 1

**Di kode Monolith, jika fitur Pesanan mengalami crash/bug yang fatal, apa yang terjadi pada fitur Buku? Bandingkan dengan versi Microservices.**

### Jawaban

Pada Monolith, fitur Buku dan Pesanan berada dalam satu aplikasi. Jika fitur Pesanan mengalami crash atau bug yang fatal, aplikasi secara keseluruhan dapat ikut terganggu sehingga fitur Buku juga berpotensi tidak dapat digunakan.

Sedangkan pada Microservices, Book Service dan Order Service berjalan secara terpisah. Jika Order Service mengalami crash, Book Service tetap dapat berjalan karena berada pada service yang berbeda.

---

## 17. Pertanyaan 2

**Pada versi Microservices, proses pengecekan stok buku menjadi sedikit lebih lambat karena membutuhkan HTTP Request. Apakah ada solusi untuk mengatasi masalah latensi jaringan ini di industri nyata?**

### Jawaban

Ada. Beberapa solusi yang dapat digunakan adalah menggunakan **caching**, menggunakan jaringan yang lebih cepat, atau menggunakan komunikasi asynchronous dengan message broker seperti **RabbitMQ** atau **Kafka**.

Caching dapat digunakan untuk menyimpan data yang sering digunakan sehingga sistem tidak selalu melakukan request ke service lain.

---

## 18. Pertanyaan 3

**Jika saat peluncuran toko buku traffic pencarian buku melonjak drastis sedangkan traffic pesanan biasa saja, layanan mana yang akan Anda scale-up atau perbanyak servernya?**

### Jawaban

**Book Service pada port 5001** yang akan di-scale-up karena menangani pencarian dan pengambilan data buku.

Order Service pada port 5002 tidak perlu diperbanyak jika traffic pesanan tetap normal.

Keuntungan Microservices adalah setiap service dapat di-scale secara independen sesuai kebutuhan traffic.

---

# 19. Ringkasan Port

| Service | File | Port | Fungsi |
|---|---|---:|---|
| Monolith | `monolith_app.py` | 5000 | Buku dan Pesanan |
| Book Service | `book_service.py` | 5001 | Mengelola data buku |
| Order Service | `order_service.py` | 5002 | Mengelola pesanan |

---

# 20. Urutan Menjalankan Project

## Monolith

Jika Terminal berada pada:

```text
D:\semester 5\Paradigma sistem untuk IT
```

jalankan:

```powershell
cd "Monolith Microservices"
python monolith_app.py
```

Monolith berjalan pada:

```text
http://localhost:5000
```

---

## Microservices

### Terminal 1 — Book Service

```powershell
cd "Monolith Microservices"
python book_service.py
```

Book Service berjalan pada:

```text
http://localhost:5001
```

### Terminal 2 — Order Service

Buka terminal baru:

```powershell
cd "Monolith Microservices"
python order_service.py
```

Order Service berjalan pada:

```text
http://localhost:5002
```

**Book Service harus dijalankan terlebih dahulu sebelum melakukan pengujian Order Service.**

---

# 21. Catatan Penting

1. Pastikan Python sudah terinstal.
2. Pastikan Flask dan Requests sudah di-install.
3. Pastikan perintah dijalankan dari folder `Monolith Microservices`.
4. Book Service harus aktif sebelum menguji Order Service.
5. Jangan menutup terminal yang sedang menjalankan service.
6. Gunakan `CTRL + C` untuk menghentikan service.
7. Pastikan port 5000, 5001, dan 5002 tidak sedang digunakan aplikasi lain.

---

# 22. Kesimpulan

Praktikum ini menunjukkan perbedaan antara arsitektur **Monolith** dan **Microservices** menggunakan Python Flask.

Pada arsitektur Monolith, fitur Buku dan Pesanan berada dalam satu aplikasi dan berjalan pada port 5000.

Sedangkan pada arsitektur Microservices, sistem dibagi menjadi beberapa service yang berjalan secara terpisah. Book Service berjalan pada port 5001 dan Order Service berjalan pada port 5002.

Order Service berkomunikasi dengan Book Service menggunakan HTTP Request untuk mendapatkan informasi buku.

Berdasarkan eksperimen Fault Isolation, ketika Book Service dimatikan, Order Service tetap berjalan dan dapat memberikan pesan kesalahan kepada client. Hal ini menunjukkan bahwa Microservices memiliki kelebihan dalam pemisahan service, isolasi kegagalan, dan kemampuan melakukan scaling secara independen sesuai kebutuhan sistem.