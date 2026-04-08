# triger-and-view
Kelompok 8
Anggota :
- Carel Azami
- Muhammad Zaky
- Rahmadina Jogi
# 🎬 Database Bioskop (Trigger & View)

Project ini isi latihan database MySQL pake **XAMPP (MariaDB)**.
Isinya:

* tabel film & transaksi
* trigger (before & after)
* log transaksi
* view (termasuk join)

---

## ⚙️ Setup

Jalankan di CMD XAMPP:

```sql
create database bioskop;
use bioskop;
```

---

## 🗂️ Tabel

### film

* id_film (PK)
* judul
* harga

### transaksi

* id_transaksi (PK)
* id_film
* jumlah_tiket
* total_harga

### log_transaksi

* id_log (PK)
* aksi
* waktu (otomatis)

---

## 📥 Data Awal

```sql
INSERT INTO film (judul, harga) VALUES
('Avengers', 50000),
('Spiderman', 45000),
('Batman', 40000),
('Joker', 38000);

INSERT INTO transaksi (id_film, jumlah_tiket) VALUES
(1, 2),
(2, 3),
(3, 1);
```

---

## 🔥 Trigger

Yang dipake ada 6:

* BEFORE INSERT → hitung total harga + validasi tiket
* AFTER INSERT → log
* BEFORE UPDATE → validasi tiket
* AFTER UPDATE → log
* BEFORE DELETE → log
* AFTER DELETE → log

Contoh:

```sql
CREATE TRIGGER after_insert_transaksi
AFTER INSERT ON transaksi
FOR EACH ROW
INSERT INTO log_transaksi (aksi)
VALUES ('AFTER INSERT');
```

---

## 🧪 Testing

```sql
INSERT INTO transaksi (id_film, jumlah_tiket) VALUES (1, 2);

UPDATE transaksi SET jumlah_tiket = -5 WHERE id_transaksi = 1;

DELETE FROM transaksi WHERE id_transaksi = 2;
```

---

## 📊 Hasil Log (sesuai yang muncul di CMD)

```text
+--------+---------------+---------------------+
| id_log | aksi          | waktu               |
+--------+---------------+---------------------+
|      1 | AFTER INSERT  | 2026-04-08 14:19:53 |
|      2 | AFTER UPDATE  | 2026-04-08 14:20:00 |
|      3 | BEFORE DELETE | 2026-04-08 14:20:06 |
|      4 | AFTER DELETE  | 2026-04-08 14:20:06 |
+--------+---------------+---------------------+
```

---

## 👁️ View

### 1. view_transaksi

```sql
CREATE VIEW view_transaksi AS
SELECT id_transaksi, jumlah_tiket, total_harga
FROM transaksi;
```

Hasil:

* nampilin transaksi aja

---

### 2. view_detail_transaksi (JOIN)

```sql
CREATE VIEW view_detail_transaksi AS
SELECT 
    transaksi.id_transaksi,
    film.judul,
    transaksi.jumlah_tiket,
    transaksi.total_harga
FROM transaksi
JOIN film ON transaksi.id_film = film.id_film;
```

Hasil:

* tampil nama film + transaksi

---

## ⚠️ Catatan

* `total_harga` ada yang NULL karena:

  * data awal dimasukin sebelum trigger dibuat
* trigger cuma jalan kalau:

  * INSERT / UPDATE / DELETE baru

Kalau mau bener:

```sql
UPDATE transaksi SET jumlah_tiket = jumlah_tiket;
```

---

## 🎯 Kesimpulan

* trigger bikin database otomatis (hitung + log)
* log_transaksi nyimpen aktivitas
* view bikin query lebih simpel
* join bikin data lebih jelas (ga cuma id)

---

## 🧑‍💻 Note

Ini hasil langsung dari CMD (MariaDB XAMPP), jadi bukan teori doang 👍
