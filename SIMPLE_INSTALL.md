# 🐳 คู่มือติดตั้ง Docker Desktop

## ข้อกำหนดขั้นต่ำ
- Windows 10 64-bit / macOS Big Sur 11+ / Ubuntu 20.04+  
- RAM 4GB (แนะนำ 8GB)
- พื้นที่ว่าง 20GB

## 📥 ติดตั้ง

### Windows
1. **เปิด Virtualization ใน BIOS** (VT-x/AMD-V)
2. **ติดตั้ง WSL 2**
   ```cmd
   wsl --install
   wsl --set-default-version 2
   ```
3. **ดาวน์โหลดและติดตั้ง**  
   [Docker Desktop for Windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe)
4. เลือก "Use WSL 2" ตอนติดตั้ง

### macOS  
1. **ดาวน์โหลด**
   - [Apple Silicon (M1/M2/M3)](https://desktop.docker.com/mac/main/arm64/Docker.dmg)
   - [Intel Mac](https://desktop.docker.com/mac/main/amd64/Docker.dmg)
2. ลาก Docker.app ไป Applications

### Linux (Ubuntu)
```bash
# ติดตั้ง dependencies
sudo apt update
sudo apt install -y curl ca-certificates gnupg

# เพิ่ม Docker repository  
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker.gpg
echo "deb [signed-by=/usr/share/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list

# ติดตั้ง Docker
sudo apt update  
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# เพิ่ม user ใน docker group
sudo usermod -aG docker $USER
newgrp docker

# ติดตั้ง Docker Desktop (GUI)
wget -O docker-desktop.deb "https://desktop.docker.com/linux/main/amd64/docker-desktop-amd64.deb"
sudo apt install -y ./docker-desktop.deb
```

## ✅ ทดสอบการติดตั้ง
```bash
# ตรวจสอบเวอร์ชัน
docker --version
docker compose version

# ทดสอบรัน container
docker run hello-world

# ทดสอบ web server
docker run -d -p 8080:80 --name test-nginx nginx:alpine
# เปิด http://localhost:8080 
docker stop test-nginx && docker rm test-nginx
```

## 🔧 การแก้ปัญหา

### Windows
- **Docker ไม่เปิด**: รีสตาร์ท service หรือเครื่อง
- **WSL Error**: `wsl --update` และ `wsl --shutdown`
- **Virtualization**: เปิดใน BIOS และ Windows Features

### macOS  
- **Permission Error**: `sudo chown -R $(whoami) ~/.docker`
- **Intel Mac ช้า**: ลด RAM/CPU ใน Settings

### Linux
- **Permission Denied**: `sudo usermod -aG docker $USER`
- **Service ไม่ทำงาน**: `sudo systemctl start docker`

## 💡 คำสั่งพื้นฐาน
```bash
# จัดการ Container
docker run <image>              # รัน container
docker ps                       # ดู containers ที่ทำงาน
docker stop <container>         # หยุด container
docker rm <container>           # ลบ container

# จัดการ Image  
docker pull <image>             # ดาวน์โหลด image
docker images                   # ดูรายการ images
docker rmi <image>              # ลบ image

# ทำความสะอาด
docker system prune -a          # ลบข้อมูลที่ไม่ใช้ทั้งหมด

# Docker Compose
docker compose up -d            # เริ่มทุก services
docker compose down             # หยุดทุก services
```

## 🚀 เริ่มต้นใช้งาน
1. สร้าง `Dockerfile` สำหรับ app ของคุณ
2. ใช้ `docker compose` สำหรับ multi-container
3. อัปโหลด images ไป Docker Hub
4. เรียนรู้เพิ่มเติมที่ [docs.docker.com](https://docs.docker.com)

**Happy Containerizing! 🐳**