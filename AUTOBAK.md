
本地目录 /home/backup
远程目录 gdrive:backup

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


