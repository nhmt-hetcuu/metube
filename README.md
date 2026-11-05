# MeTube Docker Stack

Giao diện Web thân thiện để tải video và âm thanh từ YouTube, TikTok, Facebook, Twitter, SoundCloud và hàng trăm nền tảng khác dựa trên công cụ mã nguồn mở [yt-dlp](https://github.com/yt-dlp/yt-dlp). Tích hợp Watchtower tự động cập nhật bản mới nhất vào lúc 4:00 sáng hàng ngày.

---

## 🚀 Các dịch vụ trong Stack

| Dịch vụ | Image | Cổng Host | Chức năng |
|---|---|---|---|
| **metube** | `ghcr.io/alexta69/metube:latest` | `8002:8081` | Web UI & Backend tải video đa luồng |
| **watchtower** | `containrrr/watchtower:latest` | *Không mở* | Tự động cập nhật MeTube lúc 4:00 sáng hàng ngày |

---

## 📁 Cấu trúc thư mục

```text
.
├── .gitignore               # Loại trừ file video/âm thanh đã tải về
├── docker-compose.yml       # Khai báo stack dịch vụ MeTube & Watchtower
├── config/                  # Cấu hình lưu trữ và cài đặt yt-dlp
├── downloads/               # Thư mục chứa các file tải về trên máy host
└── README.md                # Tài liệu hướng dẫn này
```

---

## 🛠️ Hướng dẫn triển khai

### 1. Khởi chạy
```bash
docker compose up -d
```

### 2. Truy cập
- Mở trình duyệt và truy cập: `http://<IP-Server>:8002`
- Dán liên kết video hoặc danh sách phát (playlist) vào thanh tìm kiếm và bấm tải về.
- Các file sau khi tải xong sẽ nằm ngay tại thư mục `./downloads/` trên máy host.

---

## 🔄 Tự động cập nhật với Watchtower
Container Watchtower được cấu hình nhãn riêng biệt:
```yaml
labels:
  - com.centurylinklabs.watchtower.scope=metube
```
- Lịch trình: Chạy tự động vào lúc **4:00 sáng hàng ngày** (`0 0 4 * * *`).
- Tự động dọn dẹp các image cũ sau khi cập nhật thành công (`--cleanup`).

---

## 📋 Lệnh quản trị thường dùng

```bash
# Xem trạng thái container
docker compose ps

# Xem log hoạt động tải về
docker compose logs -f metube

# Khởi động lại dịch vụ
docker compose restart

# Dừng dịch vụ
docker compose down
```
