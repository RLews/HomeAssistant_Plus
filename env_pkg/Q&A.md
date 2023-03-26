安装问题
===

* 1. 安装Supervisor提示[warn] Could not find /etc/default/grub or /boot/firmware/cmdline.txt failed to switch to cgroup v1
A：修改sudo nano /boot/cmdline.txt，在最后加systemd.unified_cgroup_hierarchy=false lsm=apparmor 保存后重启

