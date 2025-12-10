# Loki Data Directory Setup

Jalankan perintah berikut untuk membuat direktori data Loki dan memberikan permission agar Loki dapat menyimpannya dengan benar:

```bash
mkdir -p loki-data/chunks
mkdir -p loki-data/index
mkdir -p loki-data/rules
mkdir -p loki-data/wal
mkdir wso2-logs
sudo chown -R 802:802 wso2-logs
sudo chmod -R 777 loki-data
```

Pastikan folder `loki-data` berada di directory yang sama dengan `docker-compose.yml` Anda.

🔄 Restart:

```
docker-compose down
docker-compose up -d
```

🟢 Setelah itu akses Grafana di:

👉 [http://localhost:3000](http://localhost:3000)
