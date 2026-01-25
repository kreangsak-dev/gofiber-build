# Go Fiber build on windows for run on linux server
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

## Run on Linux
```
chmod +x myapp-linux
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
