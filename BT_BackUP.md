
## 1.安装 rclone

```
略
```

## 2.挂载 onedrive

```
先下载windows版rclone
配置完成onedrive后 复制配置文件到linux
```

## 3.宝塔添加定时任务

```
mysqldump -uroot -p123qwe --events --ignore-table=mysql.events --all-databases > /www/wwwroot/mysite.sql

/usr/local/bin/rclone sync /www/wwwroot odrive:backup --progress
```

## 可选：戳个日期

```
echo "backup_`date +%F`" > /www/wwwroot/backup_`date +%F`.txt
```

## 特例

```
mysqldump -uroot -pQq130130 --events --ignore-table=mysql.events --all-databases > /www/wwwroot/mysite.sql
rm -rf /www/wwwroot/backup_`date -d yesterday +%F`.txt
echo "backup_`date +%F`" > /www/wwwroot/backup_`date +%F`.txt
/usr/local/bin/rclone sync /www/wwwroot odrive:backup --progress
```
