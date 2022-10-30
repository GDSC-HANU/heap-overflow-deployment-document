## Yêu cầu phần cứng tối thiểu:
```
- RAM: tối thiểu 1 GB
- CPU: tối thiểu 1 Core
- SSD không cần nhiều
- Chạy hệ điều hành Ubuntu
```

## Cài đặt môi trường

1. Cài đặt Git
2. Clone project từ Git

```
git clone https://github.com/HANU-GDSC/heap-overflow-fe.git
```

2. Cài đặt NVM (Node Version Manager)
3. Cài đặt Node `v18.8.0` bằng NVM

```
nvm install 18.8.0
```

4. Mở port 

```
sudo ufw allow 80/tcp
```

## Khởi chạy project

1. Sử dụng Node `v18.8.0` bằng NVM

```
nvm use 18.8.0
```

2. Pull project nếu cần cập nhật code mới

```
git pull
```

3. Cài đặt ENV cho project

```
nano .env.local
```
 Sau đó cài đặt theo template từ file mẫu `.env` ở thư mục gốc của project

4. Build bản production cho project

```
npm run build
```

5. Chạy project

```
serve -s dist
```

Project sẽ chạy tại port 3000