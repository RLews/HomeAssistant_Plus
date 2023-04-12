<!--
 * @FilePath: \HomeAssistant_Plus\env_pkg\install_pkg\install_list.md
 * @brief: 
 * @details: 
 * @author: Lews Hammond
 * @Date: 2023-04-08 08:05:58
 * @LastEditTime: 2023-04-09 13:18:06
 * @LastEditors: Lews Hammond
-->
install list for bullseye
===

vnc
---
1. add start services `sudo nano /etc/init.d/vncserver`
```
# Required-Start:    $local_fs
# Required-Stop:     $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start/stop vncserver
### END INIT INFO
export USER='pi'     #用户名 pi
eval cd ~$USER
case "$1" in
  start)
    # 启动命令行。此处自定义分辨率、控制台号码或其它参数。
    su $USER -c '/usr/bin/vncserver -depth 24 -geometry 800x600 :1'
    echo "Starting VNCServer for $USER "
    ;;
  stop)
    su $USER -c '/usr/bin/vncserver -kill :1'
    echo "VNCServer stopped"
    ;;
  *)
    echo "Usage: /etc/init.d/vncserver {start|stop}"
    exit 1
    ;;
esac
exit 0
```
2. `sudo chmod 755 /etc/init.d/vncserver`
3. `sudo update-rc.d vncserver defaults`
4. `sudo reboot`

docker
---

1. `sudo curl -sSL https://get.docker.com | sh` install docker 
2. `sudo curl -L "https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`
3. `sudo chmod +x /usr/local/bin/docker-compose`
4. `sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose`
5. `docker-compose version`
6. 配置镜像加速 `sudo nano /etc/default/docker`
```
DOCKER_OPTS="--registry-mirror=https://docker.mirrors.ustc.edu.cn/"
DOCKER_OPTS="--registry-mirror=https://hub-mirror.c.163.com/"
DOCKER_OPTS="--registry-mirror=https://reg-mirror.qiniu.com"
```

home-assistant
---

1. install home-assistan in docker
```
sudo docker run -d \
  --name homeassistant \
  --privileged \
  --restart=unless-stopped \
  -e TZ=Asia/Shanghai \
  -v /home/pi/Workspaces/HomeAssistant/config:/config \
  --network=host \
  ghcr.io/home-assistant/home-assistant:stable
```

2. install dependent package
```
sudo apt install \
apparmor \
jq \
wget \
curl \
udisks2 \
libglib2.0-bin \
network-manager \
dbus \
lsb-release \
systemd-journal-remote -y
```

3. install os-agent `sudo dpkg -i xxx.deb` ; switch docker cgroup version `sudo nano /boot/cmdline.txt` 末尾添加 `systemd.unified_cgroup_hierarchy=false lsm=apparmor`

4. install supervised `sudo dpkg -i xxx.deb`

5. wait services start...

6. Enter HA bash `docker exec -it homeassistant bash`

7. Download HACS `wget -O - https://get.hacs.xyz | bash -`

8. reboot HA
