## Yêu cầu phần cứng tối thiểu:
```
- RAM: tối thiểu 3 GB
- CPU: tối thiểu 2 Core
- SSD: không cần nhiều
- Chạy hệ điều hành Ubuntu với quyền root
```


# Cài đặt môi trường

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
git clone https://github.com/HANU-GDSC/heap-overflow-virtual-machine.git
```

4. Cài đặt môi trường cho Java (trong trường hợp Ubuntu chưa có sẵn)

```
sudo apt install default-jre
sudo apt install default-jdk
```

5. Cài đặt Maven build tool

```
sudo apt install maven
```

6. Mở port

```
sudo ufw allow 8080/tcp     # port for Restful API, change if needed
sudo ufw allow 2358/tcp     # port for Restful API (for jugde), change if needed
```

7. Cài đặt Docker

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl start docker
sudo systemctl status docker
```

8. Cài đặt Docker Compose

```
sudo apt-get update
sudo apt-get install docker-compose-plugin
```



## Khởi chạy project

1. Trỏ đến thư mục project

```
cd heap-overflow-virtual-machine/
```

2. Pull project nếu cần cập nhật code mới

```
git pull
```

3. Tải về các dependency

```
mvn install
```

4. Cài đặt ENV cho project

Mở file config

```
nano src/main/resources/application.properties 
```

Paste và điều chỉnh các thông số sau:

```
# ===============================
# SERVER
# ===============================

server.port=8080                                                    # port dành cho Restful API
# remove this to start rest api                                     
spring.main.web-application-type=none                               # comment dòng này nếu không muốn chạy Restful API server
```

5. Chạy project

```
screen
sudo mvn spring-boot:run
```

Để kiểm tra project đã chạy chưa bằng cách telnet vào port 2358 (sử dụng máy khác telnet vào)

Nếu chưa telnet được -> project vẫn đang khởi động, đợi thêm 1 lúc rồi thử lại

Nếu sau một lúc lâu vẫn chưa telnet được -> có lỗi xảy ra
