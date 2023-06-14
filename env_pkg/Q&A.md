<!--
 * @FilePath: \undefinedd:\git\HomeAssistant_Plus\env_pkg\Q&A.md
 * @brief: 
 * @details: 
 * @author: Lews Hammond
 * @Date: 2023-04-29 11:39:28
 * @LastEditTime: 2023-06-14 19:48:14
 * @LastEditors: Lews Hammond
-->
安装问题
===

* 1. 安装Supervisor提示[warn] Could not find /etc/default/grub or /boot/firmware/cmdline.txt failed to switch to cgroup v1
A：修改sudo nano /boot/cmdline.txt，在最后加systemd.unified_cgroup_hierarchy=false lsm=apparmor 保存后重启

* 2. 笔记本PVE系统设置合盖休眠
A: 编辑/etc/systemd/logind.conf文件，大家用顺手的编辑器即可。找到#HandleLidSwitch这一行，意思是合上笔记本上盖后的行为，默认suspend，修改为ignore（即合盖不休眠）。然后还要去掉前面的#。保存文件。
重启systemd服务，如下命令：
service systemd-logind restart
关闭屏幕
上一步做了以后，你会发现屏幕还是亮着的，
a. 安装vbetool软件：apt-get install vbetool
b. 用此命令关闭显示器：vbetool dpms off
如果你想打开显示器用命令：vbetool dpms on
至此大功告成，就是重启后必须再输一次此命令。

* 3. NTFS挂载
A: lsblk查看硬盘设备；
在/mnt下创建挂载目录
mount -t ntfs3 /dev/xxx /mnt/xxx
在PVE创建目录存储即可

* 4. PVE安装TrueNAS
A: CPU虚拟化请务必选择qemu64,使用默认的kvm64会造成系统无法正常关机（关机后会重新启动）
网卡虚拟化我选择了vmxnet3（能跑万兆），Virtio的话貌似识别不到网卡

* 5. PVE安装HomeAssistant
A: 详见https://www.jianshu.com/p/e02ae80a0b38

* 6. PVE直通硬盘
A: ls -l /dev/disk/by-id 查看设备描述符
qm set VMID -sata1 /dev/disk/by-id/硬盘识别符
删除直通：qm set VMID -delete sata1

* 7. Ubuntu Servers安装qemu-guest-agant
A: sudo apt install qemu-guest-agent 
systemctl start qemu-guest-agent
systemctl enable qemu-guest-agent

* 8. PVE 7.4.1 更新软件源
A: cp /etc/apt/sources.list /etc/apt/sources.list.bak
sed -i.bak "s#ftp.debian.org/debian#mirrors.aliyun.com/debian#g" /etc/apt/sources.list
sed -i "s#security.debian.org#mirrors.aliyun.com/debian-security#g" /etc/apt/sources.list
echo "deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription" >>  /etc/apt/sources.list
wget https://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bullseye.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg
echo "deb https://mirrors.ustc.edu.cn/proxmox/debian/pve bullseye pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list

* 9. NTFS在linux挂载只读
A: sudo ntfsfix /dev/xxx
sudo umount /dev/xxx
sudo mount -o rw /dev/xxx

* 10. NextCloud部署
A: UbuntuServices安装宝塔平台、一键部署，安装nextcloud



