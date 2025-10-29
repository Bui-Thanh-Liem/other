# Hướng Dẫn Triển Khai Docker Swarm

## 1. Chuẩn bị

- Ít nhất 2 máy chủ (1 manager, 1+ worker)
- Docker Engine đã cài đặt (phiên bản 1.12+)
- Mở các port cần thiết:

| Port | Giao thức | Mô tả |
|------|------------|-------|
| TCP 2377 | TCP | Cluster management |
| TCP/UDP 7946 | TCP/UDP | Node communication |
| UDP 4789 | UDP | Overlay network traffic |

---

## 2. Khởi tạo Swarm trên Manager Node

### Trên máy Manager:
```bash
docker swarm init --advertise-addr <MANAGER-IP>
```

- Lệnh này sẽ trả về token để join worker nodes  
- Ví dụ:
```bash
docker swarm join --token SWMTKN-1-xxxxx <MANAGER-IP>:2377
```

---

## 3. Thêm Worker Nodes

### Trên các máy Worker:
```bash
docker swarm join --token <TOKEN> <MANAGER-IP>:2377
```

---

## 4. Kiểm tra cluster

### Trên Manager node:
```bash
docker node ls
```

---

## 5. Deploy service

Ví dụ deploy nginx:
```bash
docker service create --name web --replicas 3 -p 80:80 nginx
```

Kiểm tra service:
```bash
docker service ls
docker service ps web
```

---

## 6. Scale service
```bash
docker service scale web=5
```

---

## 7. Update service
```bash
docker service update --image nginx:alpine web
```

---

## 8. Các lệnh quản lý hữu ích

Xem thông tin swarm:
```bash
docker info
```

Lấy token join (nếu mất):
```bash
docker swarm join-token worker
docker swarm join-token manager
```

Promote worker thành manager:
```bash
docker node promote <NODE-ID>
```

Remove node:
```bash
docker node rm <NODE-ID>
```

Leave swarm (từ node đó):
```bash
docker swarm leave
```

---

## 9. Triển khai Stack

Deploy:
```bash
docker stack deploy -c docker-compose.yml myapp
```

Kiểm tra stack:
```bash
docker stack ls
docker stack services myapp
```

---

## 10. Quản lý và cập nhật Service trong Swarm

Scale service:
```bash
docker service scale myapp_backend=5
```

Update image (Rolling Update):
```bash
docker service update --image your-dockerhub-username/myapp-backend:v2 myapp_backend
```

Xem chi tiết service:
```bash
docker service inspect myapp_backend
```

Remove stack:
```bash
docker stack rm myapp
```

---

## ✅ Kết luận

Bạn đã có thể:
- Khởi tạo cluster Docker Swarm hoàn chỉnh
- Thêm node, deploy và scale service
- Cập nhật image theo kiểu rolling update
- Quản lý stack ứng dụng dễ dàng

