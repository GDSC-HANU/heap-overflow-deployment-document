## Yêu cầu phần cứng tối thiểu:
```
- RAM: tối thiểu 1 GB
- CPU: tối thiểu 1 Core
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
git clone https://github.com/HANU-GDSC/heap-overflow-judger.git
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
sudo ufw allow 8080/tcp     # port for Restful API (for monitoring), change if needed
```


## Các công nghệ đi kèm

Hệ thống sử dụng MySQL và RabbitMQ mà hệ thống back end dùng, nếu chưa deploy thì xem file hướng dẫn deploy back end tại [đây](be.md)



## Khởi chạy project

1. Trỏ đến thư mục project

```
cd heap-overflow-judger/
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
server.ip=localhost                                                 # ip của server chạy service này
# remove this to start rest api                                     
spring.main.web-application-type=none                               # comment dòng này nếu không muốn chạy Restful API server

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
# CORE PROBLEM
# ===============================

runningsubmission.maxthread=2                                        # số thread tối đa dùng để judge code của Coder
runningsubmission.scanratemillis=5000                                # tần suất scan submission mới của Coder trong database (millisecond)
runningsubmission.scanlocksecond=300                                 # thời gian khóa submission của Coder để judge (second), mục đích để tránh case 2 thread cùng judge một submission

runningsubmission.vmurls=                                            # danh sách urls của các service heap-overflow-virtual-machine, ngăn cách bởi ',' ví dụ: "http://someipaddress1:2358,http://someipaddress2:2358" 
runningsubmission.vmtoken=heapoverflow                               # AUTHN_TOKEN được config khi deploy service heap-overflow-virtual-machine (config cho các service này bắt buộc phải giống nhau), mặc định là "heapoverflow"
runningsubmission.vmuser=heapoverflow                                # AUTHZ_TOKEN được config khi deploy service heap-overflow-virtual-machine (config cho các service này bắt buộc phải giống nhau), mặc định là "heapoverflow"
runningSubmission.vmdeletesubmission=true                            # heap-overflow-virtual-machine sẽ lưu lại data mỗi lần compile & interpret code, set 'true' nếu muốn xóa data đó (luôn để 'true', trừ khi có job tự xóa db cùng server với virtual-machine) 

# ===============================
#RABBITMQ
# ===============================

spring.rabbitmq.host=localhost                                       # ip của server chạy RabbitMQ
spring.rabbitmq.port=5672                                            # port của server chạy RabbitMQ
spring.rabbitmq.username=guest                                       # username của server chạy RabbitMQ
spring.rabbitmq.password=guest                                       # password của server chạy RabbitMQ
```

5. Chạy project

```
screen
mvn spring-boot:run
```

Thoát server
