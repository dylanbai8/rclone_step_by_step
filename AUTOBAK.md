
## 本地目录 /home/backup
## 远程目录 gdrive:backup

## ▚ 复制备份：

```
rclone copy /home/backup gdrive:backup
```

## ▚ 增量备份：

```
rclone copy /home/backup gdrive:backup --ignore-existing
```

## ▚ 从网盘下载：

```
rclone copy gdrive:backup /home/backup
```

## ▚ 剪切备份：

```
rclone move /home/backup gdrive:backup
```

## ▚ 同步目录：

```
rclone sync /home/backup gdrive:backup
```

## ▚ 同步两个网盘：

```
rclone sync gdrive2:backup gdrive1:backup
```


# 配置定时 增量备份 任务

备份目录 /www/wwwroot

目标网盘/目录 gdrive:backup

## ▚ 配置systemd服务

```
touch /etc/systemd/system/auto_back.service

cat <<EOF > /etc/systemd/system/auto_back.service
[Unit]
Description=Auto Backup
[Service]
Type=simple
ExecStart=/usr/local/bin/rclone copy /www/wwwroot gdrive:backup --ignore-existing
User=root
EOF
```

## ▚ 配置systemd定时器

```
touch /etc/systemd/system/auto_back.timer

cat <<EOF > /etc/systemd/system/auto_back.timer
[Unit]
Description=Daily sync local files to google drive
[Timer]
OnBootSec=5min
OnUnitActiveSec=1d
TimeoutStartSec=1h
Unit=auto_back.service
[Install]
WantedBy=multi-user.target
EOF
```

OnBootSec表示开机后五分钟后启动

OnUnitActiveSec表示每隔1天执行一次

TimeoutStartSec表示脚本执行后1小时后检查结果，防止备份时间过长，脚本认为没有响应，认为任务失败而中断任务

## ▚ 启用定时器并加入开机启动

```
systemctl enable auto_back.timer && systemctl start auto_back.timer

```


