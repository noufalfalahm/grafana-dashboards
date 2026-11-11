# Enterprise & Infrastructure Grafana Dashboard Stack

Repositori ini berisi kumpulan konfigurasi ekspor skema dashboard Grafana (`dashboard.grafana.app/v2`) untuk pemantauan server Linux, kontainer Docker, jaringan, Zabbix data source, serta ketersediaan website berbasis Blackbox Exporter.

## 📋 Daftar Dashboard

1. **Pusat Monitoring Lengkap: Server, Website & Docker** (`Pusat Monitoring Lengkap_ Server, Website & Docker-1794386001760.json`)
   - **Fungsi:** Dashboard utama dan terpadu untuk memantau kesehatan server, performa kontainer Docker, ketersediaan website, serta sistem *alerting* otomatis via Discord.
   - **Fitur:** Panduan membaca dashboard, ringkasan kesehatan sistem (CPU, RAM, Uptime), panel alert aktif, pemantauan website & SSL, metrik Node Exporter mendalam, statistik Docker, hingga kesehatan target Prometheus.

2. *Node Exporter Full* (`Node Exporter Full-1794367577975.json`)
   - **Fungsi:** Pemantauan mendalam metrik server Linux (Node Exporter).

3. *Disk Space Utilization Trend* (`Disk Space Utilization Trend (Tren Penggunaan Disk)-1794367545319.json`)
   - **Fungsi:** Monitoring tren ruang penyimpanan dan integrasi metrik Zabbix.

4. *Enterprise Monitoring Stack / Coba Coba* (`COBA COBA-1794367533190.json`)
   - **Fungsi:** Dashboard alternatif terpadu pemantauan host, Docker Engine, dan website.

## 🛠️ Prasyarat (Prerequisites)

Sebelum mengimpor dashboard ini, pastikan infrastruktur pemantauan kamu telah terpasang komponen berikut:
- **Grafana** (v11.x / v13.x+)
- **Prometheus** (sebagai DataSource utama)
- **Node Exporter** (pada target server Linux)
- **cAdvisor** (opsional, untuk metrik kontainer Docker)
- **Blackbox Exporter** (opsional, untuk probe website & SSL)
- **Zabbix Server & Plugin Grafana Zabbix** (opsional, jika menggunakan integrasi Zabbix)

## 🚀 Cara Import ke Grafana

1. Buka instance **Grafana** kamu melalui peramban web.
2. Di menu navigasi sebelah kiri, klik **Dashboards** > **New** > **Import**.
3. Pilih opsi **Upload dashboard JSON file** dan unggah file JSON dari repositori ini (misalnya file Pusat Monitoring Lengkap).
4. Sesuaikan nama dashboard dan pilih **Data Source** (Prometheus) yang sesuai dengan lingkungan kamu.
5. Klik **Import**.

## 💻 Panduan Menjalankan Monitoring Stack (Docker Compose)

Jika kamu menjalankan stack ini menggunakan Docker Compose, pastikan port exporter terekspos dengan benar:

```yaml
version: '3.8'
services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    ports:
      - "9100:9100"
  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    container_name: cadvisor
    ports:
      - "8080:8080"
