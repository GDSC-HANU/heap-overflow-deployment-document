Đây là document hướng dẫn deploy hệ thống Heap Overflow. 

Xem hệ thống live tại www.heap-overflow.com

## Tổng quan kiến trúc hệ thống

```
      user
       |
       v
heap-overflow-fe (server front-end)
       | 1:1
       v
heap-overflow-be (server back-end)
       | 1:m
       v
heap-overflow-judger (server để chấm code của user, có thể scale ra nhiều node)
       | 1:m
       v
heap-overflow-virtual-machine (server để compile, interpret code cho các ngôn ngữ lập trình, có thể scale ra nhiều node)
```

- `heap-overflow-fe`: service front end, gọi đến `heap-overflow-be` thông qua Restful API, Socket
- `heap-overflow-be`: service back end, cung cấp các API cho front-end để thực hiện các nghiệp vụ của hệ thống
- `heap-overflow-judger`: khi user submit code, service này sẽ gọi đến `heap-overflow-virtual-machine` để chạy code của user với từng test case, lấy ra output, so sánh với output đúng của test case, cuối cùng trả về kết quả
- `heap-overflow-virtual-machine`: service dùng để compile & interpret code cho các ngôn ngữ lập trình


## Yêu cầu phần cứng

Trong mỗi file hướng dẫn cho từng service đều có yêu cầu phần cứng riêng, dưới đây là yêu cầu phần cứng tổng thể cho toàn bộ hệ thống nếu chạy tất cả service trên cùng 1 server

```
RAM: tối thiểu 8 GB
CPU: tối thiểu 4 Core
SSD: tối thiểu 30 GB
```

## Hướng dẫn chi tiết từng service:
- [heap-overflow-fe](fe.md)
- [heap-overflow-be](be.md)
- [heap-overflow-judger](judger.md)
- [heap-overflow-virtual-machine](virtual-machine.md)

