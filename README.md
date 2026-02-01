# Go Fiber
## Install Setup
```
# โปรเจคส่วนตัว
go mod init myapp

# ถ้าจะ publish ขึ้น GitHub
go mod init github.com/username/project-name

# ติดตั้ง dependencies
go get github.com/gofiber/fiber/v2

# จัดระเบียบ dependencies
go mod tidy 
```

## Live reload with Air
```
https://github.com/air-verse/air
```
```
go install github.com/air-verse/air@latest
air init

# run go project
air
```
### .air.toml
```
[build]
  cmd = "go build -o ./tmp/main.exe ./cmd/server"
  delay = 200
  exclude_dir = ["assets", "tmp", "vendor", "testdata", "node_modules", "web"]
```

# Go Fiber Build on windows for run on linux server
## Build on windows with PowerShell
``` PowerShell
$env:GOOS = "linux"
$env:GOARCH = "amd64"
$env:CGO_ENABLED = "0"
go build -o myapp-linux main.go

คำอธิบาย:
$env:GOOS = "linux": บอก Compiler ว่าเป้าหมายคือ Linux
$env:GOARCH = "amd64": บอกสถาปัตยกรรม CPU (ส่วนใหญ่ Server ใช้ amd64 ถ้าเป็น Raspberry Pi ให้ใช้ arm64)
$env:CGO_ENABLED = "0": ปิดการใช้ C Library เพื่อให้ไฟล์ทำงานได้ทุกที่โดยไม่ต้องลง Library เพิ่ม (Static Linking)
-o myapp-linux: ตั้งชื่อไฟล์ output ว่า myapp-linux (จะไม่มีนามสกุล .exe)
```

## Build on Macbox M4 ARM
```
GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -ldflags="-s -w" -o myapp main.go
```

## Run on Linux
```
sudo chmod +x myapp-linux
./myapp-linux
```

## Create Service
```
sudo nano /etc/systemd/system/gofiber-app.service

sudo systemctl daemon-reload
sudo systemctl enable gofiber-app
sudo systemctl start gofiber-app
sudo systemctl restart gofiber-app
sudo systemctl stop gofiber-app
```

### in Service file
```
[Unit]
Description=GoFiber Web Application
After=network.target

[Service]
User=root
Group=root

WorkingDirectory=/home

ExecStart=/home/nurse-table

Restart=always
RestartSec=5

// Environment Variables (ถ้าต้องการใส่ตรงนี้แทน .env)
// Environment=PORT=3000
// Environment=GO_ENV=production

[Install]
WantedBy=multi-user.target
```
