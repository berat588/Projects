# Projects
---
  # RakipRadar

  Türk moda perakende sektöründe rakip fiyatlarını otomatik izleyen,
  fiyat düşüşlerini tespit eden ve HTML rapor üreten Python CLI aracı.

  ## İzlenen Markalar

  | Marka          | Kaynak  | Durum        |
  |----------------|---------|--------------|
  | Koton          | Gerçek  | ✅ Canlı veri |
  | LC Waikiki     | Simüle  | 🔵 Segment bazlı |
  | Zara           | Simüle  | 🔵 Segment bazlı |
  | Bershka        | Simüle  | 🔵 Segment bazlı |
  | Massimo Dutti  | Simüle  | 🔵 Segment bazlı |
  | DeFacto        | Simüle  | 🔵 Segment bazlı |

  > LC Waikiki, Zara, Bershka, Massimo Dutti ve DeFacto siteleri
  > bot koruması (Akamai) kullandığından fiyatları gerçek fiyat
  > segmentlerine göre modellenen simülasyon verisiyle temsil edilir.
  > Ürün linkleri her zaman orijinal siteye yönlendirir.

  ## Kurulum

  ```bash
  git clone https://github.com/kullanici/rakipradar.git
  cd rakipradar
  pip install -r requirements.txt

  Kullanım

  1. Simülasyon sunucusunu başlat (ayrı terminal)

  python mock_server.py

  2. Fiyatları çek

  python main.py scrape

  3. HTML rapor üret

  python main.py report
  # → reports/report_YYYYMMDD_HHMMSS.html

  4. Alertları görüntüle

  python main.py alerts          # Son 100 alert
  python main.py alerts -n 20   # Son 20 alert

  5. Otomatik zamanlayıcı

  python main.py schedule
  # Her 6 saatte bir scrape çalışır

  CLI Komutları

  ┌───────────────┬─────────────────────────────────────────────┐
  │     Komut     │                  Açıklama                   │
  ├───────────────┼─────────────────────────────────────────────┤
  │ scrape        │ Tüm kaynaklardan fiyat çek, SQLite'a kaydet │
  ├───────────────┼─────────────────────────────────────────────┤
  │ report        │ HTML rapor oluştur                          │
  ├───────────────┼─────────────────────────────────────────────┤
  │ schedule      │ 6 saatte bir otomatik çalıştır              │
  ├───────────────┼─────────────────────────────────────────────┤
  │ alerts [-n N] │ %10+ fiyat düşüş alertlarını listele        │
  └───────────────┴─────────────────────────────────────────────┘

  Alert Mantığı

  - Her scrape'de ürünün önceki fiyatı veritabanından sorgulanır
  - Fiyat %10 veya daha fazla düşmüşse alerts tablosuna kaydedilir
  - Konsola uyarı basılır
  - HTML raporun başında ayrı bir "Fiyat Düşüş Alertları" tablosu yer alır

  Proje Yapısı

  rakipradar/
  ├── main.py          # CLI giriş noktası
  ├── scraper.py       # Fiyat çekme motoru
  ├── database.py      # SQLite CRUD işlemleri
  ├── reporter.py      # HTML rapor üreticisi
  ├── scheduler.py     # Otomatik zamanlayıcı
  ├── mock_server.py   # Simülasyon sunucusu
  ├── config.py        # Kaynak ve eşik ayarları
  └── requirements.txt

  Veritabanı

  products      (id, name, sku, category, source_url, created_at)
  price_history (id, product_id, price, currency, scraped_at)
  alerts        (id, product_id, old_price, new_price, change_pct, created_at)

  Ayarlar (config.py)

  ┌─────────────────────────┬────────────┬─────────────────────────┐
  │        Değişken         │ Varsayılan │        Açıklama         │
  ├─────────────────────────┼────────────┼─────────────────────────┤
  │ ALERT_THRESHOLD         │ 10.0       │ Alert eşiği (%)         │
  ├─────────────────────────┼────────────┼─────────────────────────┤
  │ SCHEDULE_INTERVAL_HOURS │ 6          │ Scrape aralığı          │
  ├─────────────────────────┼────────────┼─────────────────────────┤
  │ MOCK_SERVER_PORT        │ 8765       │ Simülasyon sunucu portu │
  └─────────────────────────┴────────────┴─────────────────────────┘
