JENKINS CI
==========


ubuntu命令使用
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

查看系统版本
lsb_release -a



ubuntu安装
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. 申请两台cloud环境，建议8核32G Ram , master - slave
2. 备份deb源文件 sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
3. 添加源
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
3.1. 添加代理 apt-get -o Acquire::http::proxy="https://10.144.1.10:8080/" update   ??
4. apt-get update
5. 下载deb包或者apt-get install jenkins  ，如果有要求，则apt --fix-broken install
6. 如果无法安装jdk则从oracle官网下载tgz包，解压到任意文件夹下，添加ln  -s /yourpath/bin/java /usr/bin/java ,通过which java检测java安装情况
7. 跳过设置后如何找回admin密码， 可以查看/var/lib/jenkins/users/admin/config.xml 中修改加密串为 #jbcrypt:$2a$10$DdaWzN64JgUtLdvxWIflcuQu2fgrrMSAMabF5TSrGK5nXitqK9ZMS   (此为111111)
8.

jenkins其他相关
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. 注册一个公用账号  https://idm.int.net.nokia.com/IdmGuiWeb/flow/functionalEntry/register?execution=e2s1 ，例如ci.oam@nokiaxx
2. 等公用邮箱注册完成之后，申请网络账号 https://nsnsi.service-now.com/ess?id=sc_cat_item&sys_id=2f535360db146f0c012570600f96195a   （id可以查看1中返回的邮件）
3. 等待邮件回复后的域用户，进行基础的密码修改 https://pwc.int.net.nokia.com/upprod/
4. master机创建公密钥 ssh-keygen -t rsa , 拷贝id_rsa.pub 内容到 slave机的authorization中，测试ssh root@slave_ip
5. 给子节点的机器增加密码（if root） , 在master jenkins上添加凭证，采用ssh with password即可
6. 创建子节点，测试连通性
7. 登陆公用账号的gitlab，添加 jenkins服务器上生成的公钥 ,  https://baltig.nsn-net.net/users/sign_in
8. 登陆jenkins，凭证处添加domain，增加凭证，以SSH with private key的方式，添加jenkins服务器上的密钥
9. 新建一个job，测试凭证的有效性 , 保存不出现128错误即正常
10.添加全局groovy脚本以供pipeline使用，
11.
