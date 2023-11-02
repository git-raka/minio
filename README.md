# minio

```
wget https://dl.min.io/server/minio/release/linux-amd64/minio
```
```
chmod +x minio
```
```
sudo mv minio /usr/local/bin/
```
```
mkdir ~/minio
```
```
sudo chown user:user /usr/local/bin/minio
```
```
sudo nano /etc/default/minio
```
```
sudo nano /etc/default/minio
```
```

MINIO_ROOT_USER="minioadmin"
MINIO_VOLUMES="/home/bsp/minio"
MINIO_OPTS="-C /etc/minio --address 192.168.18.237:9000 --console-address :9001"
MINIO_ROOT_PASSWORD="minioadmin"
```
```
sudo nano /etc/systemd/system/minio.service
```
```
[Unit]
Description=MinIO
Documentation=https://docs.min.io
Wants=network-online.target
After=network-online.target
AssertFileIsExecutable=/usr/local/bin/minio

[Service]
WorkingDirectory=/usr/local/

User=bsp
Group=bsp
ProtectProc=invisible

EnvironmentFile=/etc/default/minio
ExecStartPre=/bin/bash -c "if [ -z \"${MINIO_VOLUMES}\" ]; then echo \"Variable MINIO_VOLUMES not set in /etc/default/minio\"; exit 1; fi"
ExecStart=/usr/local/bin/minio server $MINIO_OPTS $MINIO_VOLUMES

# Let systemd restart this service always
Restart=always

# Specifies the maximum file descriptor number that can be opened by this process
LimitNOFILE=1048576

# Specifies the maximum number of threads this process can create
TasksMax=infinity

# Disable timeout logic and wait until process is stopped
TimeoutStopSec=infinity
SendSIGKILL=no

[Install]
WantedBy=multi-user.target

# Built for ${project.name}-${project.version} (${project.name})
```

```
sudo systemctl daemon-reload
```
```
sudo systemctl enable minio
```
```
sudo systemctl start minio
```
