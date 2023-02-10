## Yêu cầu phần cứng tối thiểu:
```
- RAM: tối thiểu 1 GB
- CPU: tối thiểu 1 Core
- SSD: không cần nhiều
- Chạy hệ điều hành Ubuntu với quyền root
```

## Cài đặt môi trường

1. Cập nhật hệ điều hành

```
sudo apt-get update
sudo apt-get upgrade
```

2. Cài đặt Git

```
sudo apt install git
```

3. Clone project từ Git

```
git clone https://github.com/HANU-GDSC/heap-overflow-fe.git
```

4. Cài đặt NVM (Node Version Manager)

```
sudo apt install curl 
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash 
source ~/.profile 
```

5. Cài đặt Node `v18.8.0` bằng NVM

```
nvm install 18.8.0
```

6. Mở port 

```
sudo ufw allow 80/tcp
```

7. Cài đặt Nginx để redirect port 80 về port 3000 (port mà project sẽ chạy) vì khi request bằng domain, mặc định sẽ vào port 80
```
sudo apt install nginx
sudo ufw allow 'Nginx HTTP'
sudo systemctl start nginx
cd /etc/nginx/conf.d
nano domain.conf
```

Paste config sau:

```
server {
        listen 80;
        server_name www.heap-overflow.com; #change to your domain name
 
        location / {
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_pass http://checkip.dyndns.com/;  #change to your internal server IP
                proxy_redirect off;
        }
}
```

Đóng file:

```
Ctrl + X
Gõ Y để save
```

Restart Nginx:

```
sudo systemctl reload nginx
```



## Khởi chạy project

1. Trỏ tới thư mục của project:

```
cd heap-overflow-fe/
```

2. Sử dụng Node `v18.8.0` bằng NVM

```
nvm use 18.8.0
```

3. Pull project nếu cần cập nhật code mới

```
git pull
```

4. Cài đặt ENV cho project

```
nano .env.local
```

Paste và điều chỉnh các thông số sau:

```
VUE_APP_API = http://backendip:backendport      # thay 'backendip', 'backendport' bằng ip, port của server chạy service heap-overflow-be
VUE_APP_URL = backendip                         # thay 'backendip' bằng ip của server chạy service heap-overflow-be
VUE_APP_PORT = 8080                             # đây chỉ là port khi build dev, không ảnh hưởng việc build production
VUE_APP_SOCKET_PORT = 5000                      # port chạy socket của heap-overflow-be
DANGEROUSLY_DISABLE_HOST_CHECK = true           # không thay đổi giá trị này
```

5. Tải về các dependency

```
npm i
npm install element-plus --save
```

6. Build bản production cho project

```
npm run build
```

7. Chạy project

```
npm install -g serve (nếu chưa cài serve)
serve -s dist
```

Project sẽ chạy tại port 3000
