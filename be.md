## Yêu cầu phần cứng tối thiểu (chạy cả service, database, queue):
```
- RAM: tối thiểu 3 GB
- CPU: tối thiểu 2 Core
- SSD: tối thiểu 30 GB
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
git clone https://github.com/HANU-GDSC/heap-overflow-be.git
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
sudo ufw allow 5000/tcp     # port for Socket, change if needed
```


## Các công nghệ đi kèm

Hệ thống back end cần chạy cùng MySQL (v8.0.31+) và RabbitMQ (v3.11.2+)

1. Cài đặt MySQL

```
sudo apt install mysql-server
sudo systemctl start mysql.service
sudo mysql_secure_installation
sudo mysql -u root -p
CREATE DATABASE tên_database_cho_service_back_end;
CREATE USER 'username_cho_service_back_end'@'ip_của_service_back_end' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'username_cho_service_back_end'@'ip_của_service_back_end' WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit
```

Mở port nếu chạy MySQL trên server riêng

```
sudo ufw allow 3306/tcp
```

2. Cài đặt Docker để chạy RabbitMQ

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
```

3. Chạy RabbitMQ bằng Docker

Mở port nếu chạy RabbitMQ trên server riêng

```
sudo ufw allow 5672/tcp
sudo ufw allow 15672/tcp
```

Chạy RabbitMQ

```
screen
docker pull rabbitmq:3-management
docker run --rm -it -p 15672:15672 -p 5672:5672 rabbitmq:3-management
```

Thoát server

## Khởi chạy project

1. Trỏ đến thư mục project

```
cd heap-overflow-be/
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

server.port=8080                                                    # port để chạy service back end

# ===============================
# DATABASE
# ===============================

spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=                                              # url để connect đến server MySQL, ví dụ jdbc:mysql://localhost:3306/heapoverflow
spring.datasource.username=                                         # username của MySQL
spring.datasource.password=                                         # password của MySQL

# ===============================
# JPA / HIBERNATE
# ===============================

spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
spring.jpa.properties.hibernate.enable_lazy_load_no_trans=true
spring.mvc.pathmatch.matching-strategy=ANT_PATH_MATCHER

# ===============================
# PRACTICE PROBLEM
# ===============================

practiceproblem.problem.runningsubmission.serverip=                  # ip của server chạy service back end
practiceproblem.problem.runningsubmission.port=                      # socket port, front end dùng để lấy thông tin running submission

# ===============================
# CODER AUTH
# ===============================

coderauth.session.tokensecret=                                        # secret key dùng để authorize Coder

# ===============================
# SWAGGER
# ===============================

springdoc.swagger-ui.path=/swagger-ui.html                              
springdoc.swagger-ui.operationsSorter=method

# ===============================
#RABBITMQ
# ===============================

spring.rabbitmq.host=localhost                                        # ip của server chạy RabbitMQ
spring.rabbitmq.port=5672                                             # port của server chạy RabbitMQ
spring.rabbitmq.username=guest                                        # username của server chạy RabbitMQ
spring.rabbitmq.password=guest                                        # password của server chạy RabbitMQ
```

5. Chạy project

```
screen
mvn spring-boot:run
```

Thoát server
