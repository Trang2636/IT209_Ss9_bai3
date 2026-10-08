# Bài 3 – Viết Dockerfile đóng gói trang Web HTML tĩnh

## Mô tả

Đóng gói một trang HTML tĩnh đơn giản vào Docker Image sử dụng Nginx Alpine làm base image.

## Cấu trúc thư mục

```
ex3/
├── Dockerfile       # Tệp cấu hình Docker (2 lệnh)
├── index.html       # Trang HTML tĩnh
└── README.md        # Báo cáo bài tập
```

## Nội dung Dockerfile

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

**Giải thích:**

| Lệnh | Mô tả |
|-------|-------|
| `FROM nginx:alpine` | Sử dụng image Nginx phiên bản Alpine (nhẹ, ~5MB) làm nền tảng |
| `COPY index.html ...` | Sao chép file `index.html` vào thư mục mặc định chứa web của Nginx |

## Các bước thực hiện

### 1. Build Docker Image

```bash
docker build -t my-html-app:v1 .
```

**Kết quả:**

```
[+] Building 2.1s (7/7) FINISHED
 => [internal] load build definition from Dockerfile
 => [internal] load .dockerignore
 => [internal] load metadata for docker.io/library/nginx:alpine
 => [1/2] FROM docker.io/library/nginx:alpine
 => [2/2] COPY index.html /usr/share/nginx/html/index.html
 => exporting to image
 => => naming to docker.io/library/my-html-app:v1
```

### 2. Khởi chạy Container

```bash
docker run -d -p 8081:80 --name html-app my-html-app:v1
```

| Tham số | Mô tả |
|---------|-------|
| `-d` | Chạy container ở chế độ nền (detached) |
| `-p 8081:80` | Map cổng 8081 trên host tới cổng 80 trong container |
| `--name html-app` | Đặt tên container là `html-app` |

### 3. Kiểm tra kết quả

```bash
curl http://localhost:8081
```

**Kết quả mong đợi:**

```html
<h1>Hello Docker Session 09!</h1>
```

### 4. Dọn dẹp (tuỳ chọn)

```bash
# Dừng container
docker stop html-app

# Xoá container
docker rm html-app

# Xoá image
docker rmi my-html-app:v1
```

> ✅ Lệnh `curl` trả về đúng chuỗi `<h1>Hello Docker Session 09!</h1>` – xác nhận Docker Image đã được build và chạy thành công.
