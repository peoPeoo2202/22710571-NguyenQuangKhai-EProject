# EProject - Phase 1

EProject - Phase 1/
├── .github/workflows/
│   └──ci.yml                 # CI/CD pipeline
├── auth/
│   ├── src/
│   │   ├── controllers/         # Auth business logic
│   │   ├── routes/             # API endpoints
│   │   ├── services/           # Core auth services
│   │   └── test/               # Auth tests
│   ├── Dockerfile
│   └── package.json
├── product/
│   ├── src/
│   │   ├── controllers/        # Product business logic
│   │   ├── routes/            # API endpoints
│   │   ├── services/          # Core product services
│   │   └── test/              # Product tests
│   ├── Dockerfile
│   └── package.json
├── order/                     # Future service
├── api-gateway/              # Future service
├── docker-compose.yml        # Local development
└── README.md

## Mô tả
Dự án là một hệ thống microservices gồm các service chính:
- `auth` — service xác thực
- `product` — service quản lý sản phẩm
- `order` — service xử lý đơn hàng
- `api-gateway` — gateway để client gọi vào các service

Hệ thống sử dụng MongoDB làm database và RabbitMQ làm message broker. Mỗi service có Dockerfile riêng và được cấu hình trong `docker-compose.yaml`.

## Kiến trúc & Ports mặc định
Các port mặc định (có thể thay đổi trong `docker-compose.yaml` hoặc `.env`):
- MongoDB (container): 27017, ánh xạ host:container ví dụ `37017:27017`
- RabbitMQ:
  - AMQP: 5672
  - Management UI: 15672
- Product service: 3001
- API Gateway: 3003
- (Nếu có, kiểm tra `auth` và `order` trong cấu hình để biết port thực tế)

Xem `docker-compose.yaml` để biết cấu hình chi tiết và các tên service.

## Yêu cầu
- Docker & Docker Compose (hỗ trợ `version: '3.8'` trong `docker-compose.yaml`)
- Node.js (nếu muốn chạy từng service cục bộ)
- (Tùy chọn) npm hoặc yarn để cài dependencies khi chạy local

## Chạy toàn bộ hệ thống (khuyến nghị)
Mở terminal (PowerShell, bash, v.v.) tại thư mục gốc của repo và chạy:

PowerShell:
```powershell
docker-compose up --build
```

Hoặc chạy ở chế độ nền (detached):
```powershell
docker-compose up --build -d
```

Dừng và xóa các container, network:
```powershell
docker-compose down
```

Ghi chú: Nếu muốn chạy chỉ các dependency (MongoDB và RabbitMQ) để phát triển cục bộ, có thể khởi chạy riêng:
```powershell
# Chỉ bật mongo và rabbitmq
docker-compose up -d mongo rabbitmq
```

## Chạy từng service cục bộ (phát triển)
Mỗi service có thể chạy độc lập để phát triển hoặc debug.

Ví dụ chạy `product` cục bộ:
```bash
cd product
npm install
npm start
```

Tương tự cho `auth`, `order`, `api-gateway` — vào thư mục tương ứng, cài dependencies và chạy `npm start` hoặc script phù hợp. Đảm bảo cấu hình biến môi trường trỏ tới MongoDB / RabbitMQ đang chạy.

## Chạy test
- Root repo có script test chung (`mocha`) trong `package.json` (nếu đã cấu hình).
- Mỗi service có thể có script test riêng (ví dụ `product` có `npm test`).

Ví dụ:
```bash
# Chạy test cho service product
cd product
npm install
npm test

# Hoặc chạy test toàn cục (nếu script tại root được cấu hình)
npm test
```

## Cấu hình môi trường
Các service sử dụng file `.env` (ví dụ `product/.env`, `auth/.env`, `order/.env`) và các biến này được tham chiếu trong `docker-compose.yaml`. Trước khi chạy trong môi trường production:
- Kiểm tra và cập nhật các biến môi trường: database URI, RabbitMQ URL, JWT secret, v.v.
- Tạo file `.env.example` cho từng service để dễ hướng dẫn thiết lập (khuyến nghị).

Ví dụ các biến phổ biến:
- MONGO_URI
- RABBITMQ_URL
- JWT_SECRET
- PORT

## Gợi ý cải tiến
- Thêm file `.env.example` cho từng service để dễ thiết lập.
- Thêm script phát triển (ví dụ `npm run dev` với nodemon).
- Thêm CI (GitHub Actions) để build và chạy test tự động khi push.
- Thêm hướng dẫn cấu hình cho môi trường production và staging.
- Thêm phần API docs / Postman collection hoặc OpenAPI spec.

## Liên hệ / Đóng góp
Nếu cần hỗ trợ hoặc muốn đóng góp, vui lòng mở issue hoặc PR trong repository này.

---

Tạo bởi team dự án. Cập nhật README khi có thay đổi cấu trúc hoặc ports.

