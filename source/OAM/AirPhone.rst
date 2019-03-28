AirPhone Environment
=======================================


基础
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
电口：对应bbx的端口
光口：光纤交换机

Base topology
~~~~~~~~~~~~~~~~~~~~~

..figure::/_static/pic/Communication/airphone.png
 :width:12.0cm

Constuctions:

1. OAM VM
2. Aireframe (airphone L2/L3)
3. switch 光交换机
4. Airframe (EPC+LTE)
5. Airscale (gNB) contains ABIK/ASIK)
6. ASIA + ABIK


Apply for HW
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

apply location :  `OneLab <https://onelab.eecloud.dynamic.nsn-net.net/onelab/index.php?m=reserve&a=index>`_

gNB: ASIK + ABIL + AMIA + GPS(FYGB/C)
airphone BB1: ASIA + ABIK + AMIA
airphone BB2: Airframe computer node

.. warning::
所谓ABIL,ABIC,ABIK的最主要差异是最后一位代表变体的版本，即型号，实际作用可能应用于不同层，即L1、L2
AMIA: Airscale System Module Subrack Indoor variant A , New name for FMID


New Step
~~~~~~~~~~~~~~~~~~~
1. 申请硬件环境
2. 进实验室选定插口位置
3. 提交tickets，用于申请连线以及上电操作和申请大网IP等
4. 安装对应的软件


Set up
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
AirPhone环境包含BB3/BB2/BB1 , 其中 oamvm为BB3， airframe 为BB2 ， ASIA+ABIK为BB1


BB2 :

1. 登陆实验室鉴权中心 作用以及登陆的用户密码？  --jwang@nsn-intra / 电脑密码
2. BMC ip 这个是什么，是申请的BB2硬件么？     --申请硬件时邮件回复的带的BMC的ip
3. Jviewer是干啥的？                        -- 通过浏览器直接输入ip登陆 admin/admin 之后，在 select Remote Control--→select Console Redirection--→Java Console
下载一个jnlp文件，后直接启动即可登陆连接
4. ip is provide? 申请的时候邮件会回复回来么?  --需要系统刷完之后才能操作，先执行5.2
5. bb2的盘拔到任一有操作系统可以登陆的服务器？  --随意一台还是必须，之前已经练好的环境下操作？
6. 放到controller，路径还是硬件？             -- 服务器的一个目录而已，随意建之后执行操作
7. 转包，借助命令？                           -- 通过工具进行转包
8. 转完包后执行dd复制文件到BB2中，之后回来执行5.1的设置ip部分


下载对应的airframe包
wget https://artifactory-espoo1.ext.net.nokia.com/artifactory/mnp5g-central-public-local/System_Release/4.930012.87/AirFrameVirtualized-4.930012.87.qcow2
wget https://artifactory-espoo1.ext.net.nokia.com/artifactory/mnp5g-central-public-local/System_Release/4.930012.87/Addons-4.930012.87.txz
