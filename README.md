# Docker-Network-Monitoring-Lab
modern IT altyapılarında, Sistem ve Network Operasyon Merkezlerinde (NOC) yaygın olarak kullanılan izleme (monitoring) ve alarm yönetim sistemlerinin Docker üzerinde simüle edilmesi amacıyla geliştirilmiştir. Proje kapsamında, mikroservis mimarisine uygun olarak Prometheus ve Grafana servisleri tek bir altyapıda (Docker Compose) entegre edilmiştir.



## 🛠️ Sistem Mimarisi ve Port Yapılandırması

Proje, `docker-compose.yml` dosyası aracılığıyla iki temel servisi aynı network üzerinde ayağa kaldırır:

| Servis Adı | Konteyner İsmi | Yayınlandığı Port (Localhost) | Görevi |
| :--- | :--- | :--- | :--- |
| **Prometheus** | `prometheus_server` | `9090` | Veri Toplama / Metrik Depolama |
| **Grafana** | `grafana_server` | `3000` | Görsel Panel / NOC Dashboard |

---

## 📂 Docker Compose Yapılandırması (`docker-compose.yml`)

Sistemlerin kalıcı veri saklayabilmesi (Data Persistence) için Docker Volume yapılandırması uygulanmıştır. Konteynerler silinse dahi geçmişe dönük grafik verileri kaybolmaz.

```yaml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus_server
    volumes:
      - prometheus_data:/prometheus
    ports:
      - "9090:9090"
    restart: always

  grafana:
    image: grafana/grafana:latest
    container_name: grafana_server
    ports:
      - "3000:3000"
    volumes:
      - grafana_data:/var/lib/grafana
    restart: always

volumes:
  prometheus_data:
  grafana_data:


##KURULUM VE ÇALIŞTIRMA ADIMLARI

1. Proje klasörünün içinde terminali açın
2.docker compose up -d (tüm monitoring alt yapısını arka planda başlatın)
3. docker ps (servislerin durumunu kontrol etmek için)
4. tarayıcınızdan Grafana paneline erişin (http://localhost:3000)

