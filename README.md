# 🔥 Hệ thống Phát hiện Đám Cháy Tự Động Từ Video

Ứng dụng phát hiện đám cháy thông minh sử dụng YOLOv11 (UltraLytics) kết hợp FastAPI cho back-end và React cho front-end. Cho phép người dùng tải video từ máy hoặc YouTube để phát hiện và đánh dấu đám cháy theo thời gian thực, với kết quả được lưu trữ trên Cloudinary.

---

## 🚀 Tính năng nổi bật

- ✅ Đăng ký, đăng nhập và xác thực người dùng an toàn
- 🎥 Tải video từ máy tính hoặc link YouTube
- 🔥 Phát hiện đám cháy thời gian thực bằng mô hình YOLOv11 đã huấn luyện
- 🧠 Đánh dấu vùng đám cháy trong video tự động
- 📡 Truyền kết quả phát hiện trực tuyến bằng WebSocket
- ☁️ Lưu trữ video và ảnh trên Cloudinary
- 💻 Giao diện trực quan bằng React, Material-UI và Ant Design
- 🌐 Cung cấp đầy đủ RESTful API phục vụ mở rộng

---

## ⚙️ Yêu cầu hệ thống

- Python 3.8 trở lên
- Node.js 14+ và npm
- PostgreSQL
- Tài khoản Cloudinary
- PyTorch (khuyến khích sử dụng GPU có CUDA)

---

## 🛠️ Hướng dẫn cài đặt

### ⬇️ Cài đặt Back-end

```bash
# 1. Clone dự án
git clone https://github.com/dothaogiang/datamining
cd fire-detection

# 2. Tạo và kích hoạt môi trường ảo
python -m venv .venv
source .venv/bin/activate  # Hoặc .venv\Scripts\activate trên Windows

# 3. Cài đặt các dependencies
cd be
pip install -r requirements.txt
python install_ytdlp.py  # Bắt buộc để hỗ trợ video YouTube

# 4. Tạo file cấu hình môi trường
cp .env.example .env  # Windows: copy .env.example .env
# => Cập nhật thông tin PostgreSQL, Cloudinary, JWT, SMTP

# 5. Đặt file model YOLOv11 vào thư mục `model/`
# File: bestyolov11-27k.pt

# 6. Khởi tạo CSDL
alembic upgrade head

# 7. Tạo tài khoản admin
python create_admin.py

# 8. Khởi động server FastAPI
uvicorn app.main:app --reload
# => Truy cập: http://localhost:8000
```

### 🌐 Cài đặt Front-end

```bash
# 1. Truy cập thư mục front-end
cd ../fe

# 2. Cài đặt các gói
npm install

# 3. Chạy giao diện
npm start
# => Truy cập: http://localhost:3000
```

### 🧪 Cách sử dụng

Truy cập http://localhost:3000

1. Đăng ký hoặc đăng nhập

2. Tải video lên từ máy hoặc nhập URL YouTube

3. Chờ xử lý và xem kết quả phát hiện đám cháy

4. Quản lý lịch sử video và tài khoản cá nhân

### 📁 Cấu trúc dự án

```bash
fire-detection/
├─ be/                     # Back-end (FastAPI)
│   ├─ app/                # Mã nguồn chính
│   │   ├─ api/            # Các route và API endpoints
│   │   ├─ core/           # Cấu hình hệ thống
│   │   ├─ db/             # Kết nối và ORM
│   │   ├─ models/         # Mô hình cơ sở dữ liệu
│   │   ├─ schemas/        # Định nghĩa dữ liệu với Pydantic
│   │   ├─ services/       # Logic xử lý
│   │   └─ utils/          # Hàm tiện ích
│   ├─ model/              # Mô hình YOLOv11
│   ├─ migrations/         # Alembic migrations
│   ├─ .env                # Cấu hình môi trường
│   └─ requirements.txt    # Danh sách thư viện
├─ fe/                     # Front-end (React)
│   ├─ public/             # Tài nguyên tĩnh
│   ├─ src/                # Mã nguồn React
│   └─ package.json        # Thông tin dự án FE
└─ README.md               # Tài liệu dự án
```

## API Endpoints

### Authentication

- `POST /api/v1/auth/login-email`: Đăng nhập bằng email
- `POST /api/v1/auth/register`: Đăng ký tài khoản mới
- `POST /api/v1/auth/change-password`: Thay đổi mật khẩu
- `POST /api/v1/auth/verify-token`: Xác thực token

### Users

- `GET /api/v1/users/me`: Lấy thông tin người dùng hiện tại
- `PUT /api/v1/users/me`: Cập nhật thông tin người dùng hiện tại
- `GET /api/v1/users`: Lấy danh sách người dùng
- `POST /api/v1/users`: Tạo người dùng mới
- `GET /api/v1/users/{user_id}`: Lấy thông tin người dùng theo ID
- `DELETE /api/v1/users/{user_id}`: Xóa người dùng

### Videos

- `GET /api/v1/videos`: Lấy danh sách video người dùng tải lên
- `GET /api/v1/videos/all`: Lấy danh sách tất cả video
- `GET /api/v1/videos/{video_id}`: Lấy thông tin chi tiết video
- `DELETE /api/v1/videos/{video_id}`: Xóa video

### Notifications

- `GET /api/v1/notifications`: Lấy danh sách thông báo
- `POST /api/v1/notifications`: Tạo thông báo mới
- `GET /api/v1/notifications/settings`: Lấy cài đặt thông báo
- `POST /api/v1/notifications/settings`: Cập nhật cài đặt thông báo

### User History

- `GET /api/v1/history/me`: Lấy lịch sử hoạt động của người dùng hiện tại
- `GET /api/v1/history`: Lấy lịch sử hoạt động của tất cả người dùng
- `GET /api/v1/history/{user_id}`: Lấy lịch sử hoạt động của người dùng cụ thể

### WebSocket

- `WS /ws/videos/{video_id}`: Stream kết quả xử lý video theo thời gian thực

## Lưu trữ media

- Tất cả video và ảnh được lưu trữ trực tiếp trên Cloudinary
- Không có thư mục cục bộ cho việc lưu trữ video, chỉ sử dụng thư mục tạm trong quá trình xử lý
- Hệ thống tự động xóa tất cả các file tạm sau khi xử lý xong
