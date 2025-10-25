# EProject - Phase 1

EProject - Phase 1
```
EProject - Phase 1/
├── .github/workflows/
│   └── ci.yml                 # CI/CD pipeline
├── auth/
│   ├── src/
│   │   ├── controllers/       # Auth business logic
│   │   ├── routes/            # API endpoints
│   │   ├── services/          # Core auth services
│   │   └── test/              # Auth tests
│   ├── Dockerfile
│   └── package.json
├── product/
│   ├── src/
│   │   ├── controllers/       # Product business logic
│   │   ├── routes/            # API endpoints
│   │   ├── services/          # Core product services
│   │   └── test/              # Product tests
│   ├── Dockerfile
│   └── package.json
├── order/                     # Future service
├── api-gateway/               # Future service
├── docker-compose.yml         # Local development
└── README.md
```

## Mô tả ngắn gọn
EProject là một hệ thống microservices gồm các service chính:
- `auth` — Service xác thực (đăng nhập, đăng ký, JWT…)
- `product` — Service quản lý sản phẩm
- `order` — Service xử lý đơn hàng (dự định)
- `api-gateway` — Gateway cho client (dự định)

Hệ thống dùng MongoDB làm database và RabbitMQ làm message broker. Mỗi service có Dockerfile riêng và được cấu hình trong `docker-compose.yml` để dễ triển khai cục bộ.

## Yêu cầu
- Docker & Docker Compose (project sử dụng compose v3.8+)
- Node.js (nếu muốn chạy từng service cục bộ)
- npm hoặc yarn (để cài dependencies khi chạy local)

## Ports mặc định
(Các port có thể được thay đổi bằng `.env` hoặc `docker-compose.yml`.)
- MongoDB (container): 27017 (ví dụ ánh xạ host 37017:27017)
- RabbitMQ:
  - AMQP: 5672
  - Management UI: 15672
- Product service: 3001 (mặc định)
- API Gateway: 3003 (mặc định)
- Auth / Order: kiểm tra file cấu hình tương ứng để biết port thực tế

## Biến môi trường (ví dụ)
Mỗi service nên có file `.env` hoặc sử dụng biến môi trường cấu hình trong `docker-compose.yml`. Dưới đây là ví dụ các biến phổ biến:

| Biến | Mô tả |
|------|-------|
| MONGO_URI | URI kết nối MongoDB (ví dụ: mongodb://mongo:27017/eproject) |
| RABBITMQ_URL | URL RabbitMQ (ví dụ: amqp://rabbitmq:5672) |
| JWT_SECRET | Khóa bí mật cho JWT |
| PORT | Port chạy service |

Gợi ý: Tạo file `*.env.example` cho từng service (ví dụ `product/.env.example`, `auth/.env.example`) để hướng dẫn thiết lập.

## Chạy toàn bộ hệ thống (khuyến nghị)
Từ thư mục gốc của repo:

Chạy ở foreground:
```bash
docker-compose up --build
```

Chạy ở chế độ nền (detached):
```bash
docker-compose up --build -d
```

Dừng và xóa container, network:
```bash
docker-compose down
```

Chỉ khởi động dependencies (ví dụ MongoDB và RabbitMQ) khi phát triển:
```bash
# Chỉ bật mongo và rabbitmq
docker-compose up -d mongo rabbitmq
```

## Chạy từng service cục bộ (phát triển / debug)
Mỗi service có thể chạy độc lập:

Ví dụ chạy service product:
```bash
cd product
npm install
# hoặc yarn
npm start
# hoặc script phát triển, ví dụ npm run dev (nếu có)
```

Tương tự cho `auth`, `order`, `api-gateway`. Đảm bảo các biến môi trường trỏ tới MongoDB / RabbitMQ đang chạy (cục bộ hoặc container).

## Chạy test
- Mỗi service có thể có script test riêng (ví dụ `npm test`).
- Nếu repo root có script test tổng hợp, có thể chạy từ root.

Ví dụ chạy test cho product:
```bash
cd product
npm install
npm test
```

Ví dụ chạy test từ root (nếu có cấu hình):
```bash
npm test
```

## Debugging & Tips
- Kiểm tra logs:
  - Docker: `docker-compose logs -f <service-name>`
  - Node: console logs hoặc debugger (ví dụ VS Code)
- Kiểm tra kết nối DB/RabbitMQ nếu service không hoạt động (URI, network trong docker-compose).
- Nếu thay đổi cấu trúc code, rebuild image: `docker-compose up --build`.

## Gợi ý cải tiến
- Thêm file `.env.example` cho từng service.
- Thêm script dev (`npm run dev`) sử dụng nodemon.
- Mở rộng CI (GitHub Actions) để chạy build & test tự động.
- Thêm OpenAPI/Swagger hoặc Postman collection cho API docs.
- Thêm hướng dẫn deploy cho staging/production (k8s, docker stack, v.v.)

## Đóng góp
Mọi đóng góp rất hoan nghênh — vui lòng mở issue mô tả vấn đề/ý tưởng hoặc gửi PR theo chuẩn contribution của repo.

---

Tạo bởi team dự án — cập nhật README khi có thay đổi cấu trúc, ports hoặc cách triển khai.
