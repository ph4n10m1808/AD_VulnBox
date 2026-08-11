# Docker Attack–Defense CTF Vulnbox

Môi trường Docker dùng để dựng nhiều vulnbox cho mô hình CTF Attack–Defense. Mỗi đội có một container Debian riêng, dịch vụ SSH và Docker daemon bên trong để triển khai hoặc vận hành challenge trong lab.

## Kiến trúc

| Thành phần | Dockerfile | Cổng SSH trên máy host | Cổng challenge |
|---|---|---:|---:|
| `team1-box` | `Dockerfile` | `2222` | `1000` |
| `team2-box` | `Dockerfile1` | `2223` | `1001` |
| `team3-box` | `Dockerfile2` | `2224` | `1002` |

Mỗi container có volume riêng cho `/var/lib/docker`. `docker-compose.yml` gắn cgroup của máy host và chạy container ở chế độ `privileged` để hỗ trợ Docker-in-Docker.

## Yêu cầu

- Máy Linux có Docker Engine và Docker Compose.
- Đủ tài nguyên để chạy đồng thời ba container Debian và các challenge bên trong.
- Mạng lab cô lập, không công khai các cổng SSH hoặc challenge ra Internet.

## Khởi chạy trong lab

Trước khi chạy, hãy thay toàn bộ mật khẩu mẫu trong các Dockerfile và rà soát port mapping.

```bash
docker compose up --build -d
docker compose ps
```

Dừng và xóa container:

```bash
docker compose down
```

Thêm `-v` nếu muốn xóa cả volume dữ liệu Docker của các đội.

## Cảnh báo bảo mật

Repo này cố ý tạo môi trường có mức đặc quyền cao để phục vụ CTF. Cấu hình hiện tại bật password authentication, chứa credential lab trong Dockerfile, dùng container `privileged` và gắn `/sys/fs/cgroup` từ host. Không dùng trong production, không tái sử dụng credential và không chạy trên máy chứa dữ liệu quan trọng.

Chỉ sử dụng trong hệ thống bạn sở hữu hoặc được cấp phép kiểm thử.

## Trạng thái

Đang được sử dụng như một lab CTF Attack–Defense. Repo chưa công bố license; không mặc định xem nội dung là phần mềm nguồn mở.
