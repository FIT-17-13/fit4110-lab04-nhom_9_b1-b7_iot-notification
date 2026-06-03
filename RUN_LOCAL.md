# RUN_LOCAL.md – Hướng dẫn chạy Lab 04 (Nhóm 9)

Tài liệu này hướng dẫn chi tiết cách clone repo, khởi chạy service IoT Notification của Nhóm 9 trong Docker và thực thi kiểm thử tự động.

---

## 1. Clone repo

```bash
git clone <repo-url-cua-nhom-9>
cd FIT4110_lab04_docker_packaging
```

---

## 2. Cài dependencies cho Newman/Prism/Spectral

Cài đặt các công cụ Node.js cần thiết để chạy test và xuất report:

```bash
npm install
```

---

## 3. Build Docker image

Đóng gói source code thành Docker image với tên gọi dành riêng cho Nhóm 9:

```bash
docker build -t fit4110/nhom9-service:lab04 .
```

---

## 4. Run container

Khởi chạy container từ image vừa build, map port 8000 và sử dụng file biến môi trường mẫu:

```bash
docker run --rm \
  --name fit4110-nhom9-lab04 \
  -p 8000:8000 \
  --env-file .env.example \
  fit4110/nhom9-service:lab04
```

Mở một terminal mới, kiểm tra container đã sống và phản hồi Healthcheck:

```bash
curl http://localhost:8000/health
```

(Nếu dùng PowerShell trên Windows có thể dùng:)

```powershell
Invoke-WebRequest -Uri "http://localhost:8000/health"
```

### Kết quả mong đợi:

```json
{
  "status": "ok",
  "service": "iot-notification",
  "version": "1.0.0"
}
```

---

## 5. Chạy Newman test trên container

Thực thi bộ test Postman bắn thẳng vào container đang chạy:

```bash
npm run test:local
```

Report sẽ được sinh tự động tại:

```
reports/newman-lab04-local.xml
reports/newman-lab04-local.html
```

---

## 6. Dừng container

Nếu không dùng cờ `--rm` hoặc muốn chủ động tắt container:

```bash
docker stop fit4110-nhom9-lab04
```

---

## 7. Lệnh nhanh (Makefile)

Nếu máy đã cài sẵn `make`, có thể dùng các lệnh rút gọn sau:

```bash
make build
make run
make test-docker
make stop
```

---

## 8. Bằng chứng hoàn thành (Evidences)

Dưới đây là các minh chứng Nhóm 9 đã hoàn thành tốt các tiêu chí của Lab 04:

1. Báo cáo kiểm thử Newman (Pass 100%)
2. Log khởi chạy Container thành công
3. Tag Docker Image

---
