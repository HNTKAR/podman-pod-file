# file pod

## Pod設定

|名称|値|備考|
|:-:|:-:|:-:|
|ポッド名|file||

### samba container

|名称|値|備考|
|:-:|:-:|:-:|
|コンテナ名|file-samba||
|ボリューム名|file-samba|file-samba.volumeで指定|
|netbios port|13137-13138/udp|sambaコンテナ内で指定|
|samba port|44445/tcp, 13139/tcp|sambaコンテナ内で指定|

### nginx container

|名称|値|備考|
|:-:|:-:|:-:|
|コンテナ名|file-nginx||
|ボリューム名|file-nginx|file-nginx.volumeで指定|
|port|8080/tcp|HTTP|
|port|8443/tcp|HTTPS|

## 実行スクリプト

### 共通

```sh
cd /Path/to/podman-pod-file
sudo firewall-cmd --permanent --new-service=user-samba
sudo firewall-cmd --permanent --service=user-samba --add-port=44445/tcp
sudo firewall-cmd --permanent --service=user-samba --add-port=13139/tcp
sudo firewall-cmd --permanent --service=user-samba --add-port=13137-13138/udp
sudo firewall-cmd --permanent --add-service=user-samba

sudo firewall-cmd --permanent --new-service=user-nginx
sudo firewall-cmd --permanent --service=user-nginx --add-port=44443/tcp
sudo firewall-cmd --permanent --service=user-nginx --add-port=58080/tcp
sudo firewall-cmd --permanent --add-service=user-nginx

sudo firewall-cmd --reload
mkdir -p /home/podman/run
```

### Quadlet使用時

```sh
mkdir -p $HOME/.config/containers/systemd/
cp Quadlet/* $HOME/.config/containers/systemd/
systemctl --user daemon-reload

systemctl --user start podman_build_file_samba
systemctl --user start podman_build_file_nginx
systemctl --user start podman_pod_file
```

### Quadlet非使用時

```bash
# Build Container
cd samba
podman build --tag file-samba --file Dockerfile .
cd ../nginx
podman build --tag file-nginx --file Dockerfile .

# Creeate Pod
podman pod create --replace --publish 13137-13138:13137-13138/udp --publish 13139:13139/tcp --publish 44445:44445/tcp --publish 8443:8443/tcp --publish 8080:8080/tcp --name file

# Start file-samba container
podman run --pod file --name file-samba --mount type=volume,source=file-samba,destination=/V --detach --replace file-samba
# Start file-nginx container
podman run --pod file --name file-nginx --mount type=volume,source=file-nginx,destination=/V --mount type=bind,source=/home/podman/run,destination=/run/podman --detach --replace file-nginx
```
