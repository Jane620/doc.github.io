EEcloud安装开发环境
===================================


Setup
~~~~~~~~~~~~~~~~~~~~~~~~

1. 选择nesc-ubuntu14.04 作为镜像源
2. 进入系统后安装samba，linux共享工具
3. apt-get update && install samba samba-common samba-client
4. 检查samba服务和重启操作等   service smbd status / service smbd restar
5. 修改配置 vi /etc/samba/smb.conf 去掉profiles中的;
6. 修改 其中的目录，调整为自己想要的，例如 /var/tmp/gitlab
7. EE Cloud平台中安全组添加端口规则，增加ipv4的入口 139/445/8091，默认选择CDIR
8. 在windows的文件资源管理器下输入\\linux-ip 自动进入对应的共享目录
