# 一步一步安装 rclone 并挂载 google drive 为磁盘

## ▚ 安装源和一些基本组件和依赖

```
安装源（可选）
yum -y install epel-release

安装组件和依赖
yum -y update
yum -y install wget curl unzip fuse fuse-devel
```
## ▚ 下载 Rclone (amd64) 并解压

```
下载
wget https://downloads.rclone.org/rclone-current-linux-amd64.zip

解压
unzip rclone-current-linux-amd64.zip && chmod 0755 ./rclone-*/rclone  && cp ./rclone-*/rclone /usr/local/bin/  && rm -rf ./rclone-*
```

## ▚ 运行 Rclone 开始配置

```
运行
rclone config

配置
1.输入 n 新建，输入自定义名称 如：Gdrive
2.输入 12 （Google Drive）
3.之后的选项全部默认（client_id、client_secret 都留空直接回车）
.....

4.提示设置权限 选择1 （Full access all files, Excluding Application Data Folder）
5.之后的选项全部默认（root_folder_id、service_account_file 都留空直接回车）
.....

6.是否编辑高级配置 选择n （Edit advanced config）
7.是否自动完成配置 选择n （Say N if you are working on a remote or headless machine or Y didn't work）
8.按照提示，在浏览器中打开url获取token。完成配置。

9.是否为团队版网盘 选择n （Configure this as a team drive）
10.完成配置 选择y （Yes this is OK）
11.退出 选择q
```

## ▚ 设置开机自启动

```
添加挂载点
mkdir -p /google

配置systemd自启动
touch /etc/systemd/system/rclone.service

cat <<EOF > /etc/systemd/system/rclone.service
[Unit]
Description=Rclone_Server
After=network.target
Wants=network.target
[Service]
Type=simple
ExecStart=/usr/local/bin/rclone mount Gdrive: /google --allow-other --allow-non-empty --vfs-cache-mode writes
RestartPreventExitStatus=23
Restart=always
User=root
[Install]
WantedBy=multi-user.target
EOF

开启自启动
systemctl enable rclone
```

## ▚ 重启 rclone 载入配置文件

```
systemctl daemon-reload && systemctl restart rclone
```

## ▚ 查看是否挂载成功

```
df -h
```

# 一步一步安装 cloud torrent 实现bt下载

## ▚ 安装cloud-torrent

```
curl https://i.jpillora.com/cloud-torrent! | bash
```

## ▚  设置开机自启动

```
配置systemd自启动
touch /etc/systemd/system/cloud-torrent.service

cat <<EOF > /etc/systemd/system/cloud-torrent.service
[Unit]
Description=Cloud-torrent_Server
After=network.target
Wants=network.target
[Service]
Type=simple
ExecStart=/usr/local/bin/cloud-torrent --port 8989 --config-path /usr/local/bin/cloud-torrent.json --title "Cloud Torrent" --auth "admin:123456"
RestartPreventExitStatus=23
Restart=always
User=root
[Install]
WantedBy=multi-user.target
EOF

systemctl enable cloud-torrent
```

## ▚ 重启 cloud-torrent 载入配置文件

```
systemctl daemon-reload && systemctl restart cloud-torrent
```

## ▚ 浏览器登录

```
登录地址和端口
http://ip:8989

用户名密码
admin 123456

点开右上角设置
手动修改下载路径为：/google
```

## 本教程示例中 挂载点为 /google 挂载磁盘名称为 Gdrive

# 卸载所有

## ▚ 卸载 rclone

```
rm -rf /etc/systemd/system/rclone.service
rm -rf /usr/local/bin/rclone

rm -rf /root/.cache/rclone
rm -rf /root/.config/rclone

rm -rf /google
```

## ▚ 卸载 cloud-torrent

```
rm -rf /etc/systemd/system/cloud-torrent.service
rm -rf /usr/local/bin/cloud-torrent

rm -rf /usr/local/bin/cloud-torrent.json
```

