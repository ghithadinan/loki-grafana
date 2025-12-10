# 📘 Monitoring WSO2 Logs dengan Loki, Promtail, dan Grafana

Repository ini berisi konfigurasi Docker Compose untuk membaca log WSO2 menggunakan **Promtail**, menyimpannya di **Loki**, dan menampilkannya di **Grafana**.

---

## 📁 1. Persiapan Direktori (From Zero)

Jalankan perintah berikut untuk membuat semua folder yang dibutuhkan oleh Loki dan WSO2:

```bash
# Hapus folder lama jika ada
sudo rm -rf loki-data
sudo rm -rf wso2-logs

# Buat struktur folder baru untuk Loki
mkdir -p loki-data/chunks
mkdir -p loki-data/index
mkdir -p loki-data/rules
mkdir -p loki-data/wal
mkdir -p loki-data/cache

# Buat folder log untuk WSO2
mkdir wso2-logs
sudo chmod -R 777 ./wso2-logs

# Berikan permission agar Loki dapat menulis
sudo chmod -R 777 loki-data
```

> **Note:** Folder `cache` wajib ada agar Loki tidak error: `mkdir /loki/cache: permission denied`.

Pastikan folder `loki-data` dan `wso2-logs` berada **di directory yang sama** dengan `docker-compose.yml`.

---

## 🐳 2. Menjalankan Docker Compose

Setelah file konfigurasi siap, jalankan:

```bash
docker-compose down
docker-compose up -d
```

Cek apakah semua container sudah berjalan:

```bash
docker ps
```

---

## 📊 3. Mengakses Grafana

Buka Grafana di browser:

👉 **[http://localhost:3000](http://localhost:3000)**

Login default:

* **Username:** admin
* **Password:** admin

---

## 🔧 4. Menambahkan Loki sebagai Data Source di Grafana

1. Buka menu **Connections → Data Sources**
2. Tambah **Loki**
3. Masukkan URL:

   ```
   http://loki:3100
   ```
4. Klik **Save & Test**

Jika berhasil, akan muncul pesan **"Data source is working"**.

---

## 📜 5. Query Log di Grafana

Untuk melihat log WSO2:

```
{job="wso2"}
```

Jika log belum muncul, pastikan:

* Folder `wso2-logs` sudah berisi file `.log`
* Container Promtail tidak error (`docker logs promtail`)
* Permission folder sudah benar

---

## 🛠 6. Troubleshooting

### ❌ Loki mati setelah start

Biasanya karena **permission** atau folder belum dibuat.

Periksa log Loki:

```bash
docker logs loki
```

### ❌ Promtail tidak membaca log

Periksa:

```bash
docker logs promtail
```

Pastikan path mapping sesuai seperti:

```yaml
- ./wso2-logs:/home/wso2carbon/wso2am-4.3.0/repository/logs
```

isi mount folder `wso2-logs` merupakan isi dari log aplikasi wso2 atau bisa di mounting dengan menambahkan di docker compose <br/>
```yaml
volumes: 
- path/loki-grafana/wso2-logs:/home/wso2carbon/wso2am-4.5.0/repository/logs
