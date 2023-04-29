<!--
 * @FilePath: \undefinedd:\git\HomeAssistant_Plus\env_pkg\Q&A.md
 * @brief: 
 * @details: 
 * @author: Lews Hammond
 * @Date: 2023-04-29 11:39:28
 * @LastEditTime: 2023-04-29 21:24:29
 * @LastEditors: Lews Hammond
-->
安装问题
===

* 1. 安装Supervisor提示[warn] Could not find /etc/default/grub or /boot/firmware/cmdline.txt failed to switch to cgroup v1
A：修改sudo nano /boot/cmdline.txt，在最后加systemd.unified_cgroup_hierarchy=false lsm=apparmor 保存后重启

* 2. 笔记本合盖休眠
A: 编辑/etc/systemd/logind.conf文件，大家用顺手的编辑器即可。找到#HandleLidSwitch这一行，意思是合上笔记本上盖后的行为，默认suspend，修改为ignore（即合盖不休眠）。然后还要去掉前面的#。保存文件。
重启systemd服务，如下命令：
service systemd-logind restart
关闭屏幕
上一步做了以后，你会发现屏幕还是亮着的，
a. 安装vbetool软件：apt-get install vbetool
b. 用此命令关闭显示器：vbetool dpms off
如果你想打开显示器用命令：vbetool dpms on
至此大功告成，就是重启后必须再输一次此命令。
