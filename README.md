# 🛍️ E-Commerce Website (Node.js + React) — E-Commerce-Website

Website thương mại điện tử **chỉ bán Laptop / PC & linh kiện máy tính** (theo yêu cầu đề tài).  
Repo: `https://github.com/tranhohoangvu/E-Commerce-Website.git`  
Demo video: `https://youtu.be/UqbkiGuqmX0`

---

## ✨ Tính năng chính

### Người dùng
- Xem danh sách sản phẩm, **lọc / tìm kiếm / phân trang**
- Xem chi tiết sản phẩm, chọn **biến thể (variants)** (nếu có)
- Giỏ hàng realtime (tăng/giảm/xóa không reload)
- Áp dụng **coupon** (mã 5 ký tự gồm chữ & số, có giới hạn số lần dùng)
- Checkout **guest** hoặc đã đăng nhập
- Thanh toán **COD** hoặc **VNPAY Sandbox**
- Nhận **email xác nhận** (dev: xem trên MailHog)
- Xem **lịch sử đơn hàng**, trạng thái đơn
- **Tích điểm** (loyalty) theo đơn hàng

### Admin
- Dashboard (thống kê đơn hàng / doanh thu / người dùng…)
- Quản lý **users / products / orders / coupons**

---

## 🧱 Tech Stack
- **Frontend**: React + Vite + Tailwind (`frontend/`)
- **Backend**: Node.js + Express (`backend/`)
- **Database**: MongoDB
- **Reverse proxy / Load balancer**: Nginx (`nginx/`)
- **Email dev inbox**: MailHog
- **Payment**: VNPAY Sandbox
- **Auth**: JWT + Google OAuth
- (Tuỳ chọn) **AI Chatbot**: Gemini API

---

## 📁 Cấu trúc thư mục

```
.
├── docker-compose.yml
├── nginx/
│   ├── Dockerfile
│   └── nginx.conf
├── backend/
│   ├── Dockerfile
│   ├── database/
│   │   ├── export-products.js
│   │   └── import-products.js
│   └── src/
│       ├── server.js
│       ├── config/
│       ├── controllers/
│       ├── middleware/
│       ├── models/
│       ├── routes/
│       └── services/
├── frontend/
│   ├── Dockerfile
│   └── src/
│       ├── components/
│       ├── screens/
│       ├── hooks/
│       ├── contexts/
│       ├── lib/
│       └── utils/
└── .github/workflows/
    └── deploy.yml
```

---

## ✅ Yêu cầu trước khi chạy
- Node.js >= 18 (nếu chạy local)
- Docker + Docker Compose (khuyến nghị)
- Git

---

## 🚀 Chạy nhanh bằng Docker Compose (khuyến nghị)

> Mục tiêu: giảng viên có thể chạy project chỉ bằng `docker compose up -d`.

```bash
git clone https://github.com/tranhohoangvu/E-Commerce-Website.git
cd E-Commerce-Website

docker compose up -d
docker compose logs -f
```

### Các địa chỉ thường dùng
> Tuỳ cấu hình `docker-compose.yml` & `nginx.conf`, nhưng project đang theo hướng:
- Website: `http://localhost`
- Backend API (qua Nginx): `http://localhost/api`
- Swagger UI: `http://localhost/api/docs`
- MailHog UI: `http://localhost:8025`

---

## 🧪 Chạy local (dev mode, không Docker)

### Backend
```bash
cd backend
npm install
npm run dev
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

---

## 🔐 Cấu hình biến môi trường (.env)

> **Không commit `.env` lên GitHub.**  
> Nên tạo file mẫu `.env.example` để chia sẻ cho nhóm.

### 1) Backend: `backend/.env`
Các biến thường dùng (đặt theo đúng key mà backend đang đọc):

```env
PORT=4000
NODE_ENV=development

# MongoDB
MONGODB_URI=<YOUR_MONGODB_URI>

# JWT
JWT_SECRET=<YOUR_JWT_SECRET>
JWT_EXPIRES_IN=15m
JWT_REFRESH_SECRET=<YOUR_JWT_REFRESH_SECRET>
JWT_REFRESH_EXPIRES_IN=7d

# Google OAuth
GOOGLE_CLIENT_ID=<YOUR_GOOGLE_CLIENT_ID>

# CORS + Frontend
CORS_ORIGIN=http://localhost:5173
FRONTEND_URL=http://localhost:5173

# Email (Development - MailHog)
MAIL_HOST=mailhog
MAIL_PORT=1025
MAIL_FROM="Ecom Laptop <noreply@ecomlaptop.com>"

# VNPAY (Sandbox)
VNP_TMN_CODE=<YOUR_VNP_TMN_CODE>
VNP_HASH_SECRET=<YOUR_VNP_HASH_SECRET>
VNP_URL=https://sandbox.vnpayment.vn/paymentv2/vpcpay.html
VNP_RETURN_URL=http://localhost:5173/payment/vnpay/return
VNP_IPN_URL=http://your-domain.com/api/payment/vnpay/ipn

# (Optional) Gemini
GEMINI_API_KEY=<YOUR_GEMINI_API_KEY>
```

### 2) Frontend: `frontend/.env`
```env
VITE_API_BASE_URL=http://localhost/api
VITE_GOOGLE_CLIENT_ID=<YOUR_GOOGLE_CLIENT_ID>
```

> Nếu bạn chạy local dev không qua Nginx, có thể dùng:
```env
VITE_API_BASE_URL=http://localhost:4000
```

---

## 💳 Test VNPAY Sandbox (tham khảo)
- Test card: `9704198526191432198`
- OTP: bất kỳ 6 số

---

## 📦 Scripts hữu ích

### Backend
```bash
npm run dev
npm run seed
npm run create-admin
npm run export:products
npm run import:products
```

### Frontend
```bash
npm run dev
npm run build
npm run preview
```

---

## 📚 API Docs (Swagger)
- Local backend: `http://localhost:4000/api/docs`
- Qua Nginx: `http://localhost/api/docs`

---

## 🔁 Scaling (nếu docker-compose có cấu hình service backend stateless)
Ví dụ:
```bash
docker compose up -d --scale backend=3
docker compose logs -f backend
```

---

## 🤖 CI/CD (GitHub Actions → Docker Hub)
Workflow: `.github/workflows/deploy.yml`  
Mỗi lần push lên nhánh `main`, hệ thống sẽ:
- Build Docker image backend + frontend
- Push lên Docker Hub

Cần set secrets trong GitHub repo:
- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`

---

## 🔗 Links
- GitHub Repository: `https://github.com/tranhohoangvu/E-Commerce-Website.git`
- Demo Video: `https://youtu.be/UqbkiGuqmX0`
