# MÔN PHÁT TRIỂN ỨNG DỤNG VỚI MÃ NGUỒN MỞ
---
## Họ tên: Trần Thị Thu Hà
## MSSV: K225480106009
## Lớp K58KTP.01
---
## Bài tập 3: Sử dụng wordpress để tạo website
## Deadline: 23h59 ngày 12 tháng 5 năm 2026.
---
# YÊU CẦU:
## Triển khai WordPress bằng Docker trên ubuntu
1. SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ TẠO docker ccompose chứa:
- Mariadb: sử dụng image: mariadb:latest để làm hệ quản trị csdl cho wordpress
- Phpmyadmin: sư dụng image: phpmyadmin:latest để đăng nhập vào mariadb rồi tạo csdl trống (chỉ để xem, ko cần tạo bảng từ đây, wordpress sẽ làm hết)
- WordPress: Sử dụng image: wordpress:latest, truyền các tham số môi trường cho wordpress là các thông tin truy cập csdl mariadb, tạo bởi Phpmyadmin
  
2. Yêu cầu: sau khi có 3 service này trong file docker-compose.yml :
- Cấu hình để hệ thống chạy
- Sử dụng cloudflare tunnel để public web này lên 1 sub-domain
- Tạo 1 bài viết trong wordpress giới thiệu về bản thân sinh viên: thông tin cá nhân, sở thích, ... bài viết có thể chứa hình ảnh, âm thanh, video, ...
- Tạo 1 bài viết trong wordpress giới thiệu về ngành học mà em yêu thích trong trường TNUT. bài viết phải chứa hình ảnh, video, ...
- Nhận xét việc sử dụng mã nguồn mở wordpress để tạo website (tốn công sức thế nào, dễ/khó dùng ra sao, tốn kém tài nguyên(ssh/ram) của máy chủ ra sao,....)
---

# BÀI LÀM
---
# 1. Giới thiệu đề tài
Trong bài tập này, hệ thống WordPress được triển khai bằng Docker Compose trên Ubuntu với các thành phần:

- MariaDB: hệ quản trị cơ sở dữ liệu
- phpMyAdmin: giao diện quản lý database
- WordPress: hệ quản trị nội dung mã nguồn mở

Ngoài ra, hệ thống được public ra Internet bằng Cloudflare Tunnel thông qua sub-domain.

---

# Cấu trúc dự án 
```
~/thuha_wordpress/
├── docker-compose.yml       # File cấu hình các dịch vụ Docker
├── .env                     # File lưu trữ biến môi trường (mật khẩu, tên DB)
├── README.md                # Hướng dẫn chi tiết cách chạy và thông tin bài tập
└── data/                    # Thư mục chứa dữ liệu bền vững (tự động tạo khi chạy)
    ├── db_data/             # Dữ liệu của MariaDB
    └── wp_data/             # Mã nguồn và ảnh của WordPress
```
---

# CÁC BƯỚC CÀI ĐẶT 
## Bước 1. Tạo thư mục dự án 
```bash
# Tạo thư mục dự án
mkdir thuha_wordpress
# Di chuyển vào thư mục dự án
cd thuha_wordpress
# Tạo thư mục data
mkdir -p data/db_data data/wp_data
```

<img width="464" height="82" alt="image" src="https://github.com/user-attachments/assets/d7e11297-8118-462a-a840-ca0b9f44096c" /> <br><br>

<img width="747" height="165" alt="image" src="https://github.com/user-attachments/assets/7c2e7d45-b8bb-4bff-ae9d-e75fcf35f83b" />


## Bước 2. Tạo và cấu hình file `docker-compose.yml`

```
nano docker-compose.yml
```

File `docker-compose.yml` sẽ được cấu hình gồm các service:
- Mariadb: Sử dụng để  làm hệ quản trị csdl cho wordpress
- Phpmyadmin: Giao diện để đăng nhập vào mariadb, chỉ cần tạo csdl còn lại wordpress sẽ làm hết
- Wordpress: Truyền các tham số môi trường cho wordpress là các thông tin truy cập csdl mariadb, tạo bởi Phpmyadmin

<img width="971" height="822" alt="image" src="https://github.com/user-attachments/assets/5f042475-123f-4b99-b20b-160c46d51b1b" /> <br><br>

<img width="971" height="490" alt="image" src="https://github.com/user-attachments/assets/b2f6a592-50c6-43d1-b1f0-7828d2b6009f" />

## Bước 3. Khởi động hệ thống 
```bash
# Chạy tất cả các service để pull dịch vụ
docker compose up -d
```

<img width="1003" height="261" alt="image" src="https://github.com/user-attachments/assets/d84df3bd-3eaa-4788-9aa2-c9767a7821cc" />

Kiểm tra trạng thái các container: 
```
docker ps
```

Nếu có lỗi, xem logs
```
docker compose logs -f wordpress
```

<img width="1849" height="172" alt="image" src="https://github.com/user-attachments/assets/4447f24b-cbbe-456d-8e1f-21e5be91ec09" />

---

# TRUY CẬP VÀO CÁC DỊCH VỤ
## Truy cập  PhpMyadmin
```
192.168.100.2:9090
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d1f5060b-80dd-4e3c-b9b6-55a5aec70a7a" />

## Truy cập trang wordpress
```
192.168.100.2:9000 
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e862e87a-5702-4e98-8d41-d94e5bfab298" />

---

# THIẾT LẬP WORDPRESS BAN ĐẦU VÀ TẠO BÀI ĐĂNG
## Thiết lập wordpress
1. Truy cập vào `192.168.100.2:9000 ` trên trình duyệt 
2. Chọn ngôn ngữ

<img width="964" height="1011" alt="image" src="https://github.com/user-attachments/assets/6ca1ebd4-36b5-4788-a404-b47b0b37672e" /> <br>

4. Điền thông tin quản trị
- Tên trang web (Site title)
- Username: Tên user admin (Ví dụ như: Admin_wordpress)
- Password: Thiết lập mật khẩu mạnh
- Email: Nhập email của bạn

<img width="1919" height="1031" alt="image" src="https://github.com/user-attachments/assets/b816674c-6f7a-43a0-a680-ab76e53c4824" /> <br>
5. Nhấn **Cài đặt wordpress*** sau đó đăng nhập vào trang quản trị
<img width="1875" height="684" alt="image" src="https://github.com/user-attachments/assets/719a0fd3-6101-43ed-a3cd-a92edd5f79c0" /> <br>

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f461420e-e88a-4960-b515-12c362b1d82d" />

## TẠO HAI BÀI ĐĂNG
Quy trình tạo bài đăng
1. Sau khi đăng nhập vào trang Wordpress -> Chọn **Bài viết** -> **Thêm bài viết**
2. Tiêu đề: Ghi tiêu đề bài viết
3. Viết nội dung
4. Có thể thêm ảnh hoặc video bằng cách chọn **(+)** -> Chọn hình ảnh/video -> Tải lên
5. Nhấn **Đăng**

<img width="1914" height="974" alt="image" src="https://github.com/user-attachments/assets/95024ba4-4bae-499a-8690-d8f6b829d131" />

### Bài viết 1: Giới thiệu về bản thân
<img width="1917" height="946" alt="image" src="https://github.com/user-attachments/assets/3a2b579c-1989-44bc-8825-409c84cfd6f2" /> <br>

<img width="1887" height="954" alt="image" src="https://github.com/user-attachments/assets/1e5f43b4-281e-470b-91aa-cb4f504b2329" /> <br>

<img width="1902" height="949" alt="image" src="https://github.com/user-attachments/assets/ef1e9516-d716-42e2-969b-fc18c6c1ceca" /> <br>

<img width="1703" height="961" alt="image" src="https://github.com/user-attachments/assets/e50c9fae-5561-4487-9d23-965c0c5be785" /> <br>

<img width="1760" height="958" alt="image" src="https://github.com/user-attachments/assets/f04d3ee0-84ae-43bf-8456-d659cda20b77" />

### Bài viết 2: Giới thiệu ngành Kỹ thuật máy tính
<img width="1919" height="994" alt="image" src="https://github.com/user-attachments/assets/70027e14-3e07-48de-920c-46fab2705fcf" /> <br>

<img width="1918" height="966" alt="image" src="https://github.com/user-attachments/assets/0ce895dc-ae6d-4649-865d-cfdf0a91b7b8" /> <br>

<img width="1900" height="982" alt="image" src="https://github.com/user-attachments/assets/ee8041bd-ce3a-48ab-a6eb-391bc42475f4" /> <br>

<img width="1828" height="961" alt="image" src="https://github.com/user-attachments/assets/29a11f76-f729-42ca-a6de-d7a7a907f16a" />

---

## Sử dụng Claudflare tunnel để public web 














