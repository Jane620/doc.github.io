RobotFramework automatic testing framework
==================================================

Install & setup
~~~~~~~~~~~~~~~~~~~~~~~~~~

1. python2.7 (minimum 2.6)
2. sudo apt-get install python-wxgtk2.8 (contain wxPython2.8.12.1)
4. pip install -r requirement.txt
5. sudo apt install xvfb
6. input ride.py in commandline


HWSCT Environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
tips
1.  apt-get update 全是ign，无法更新， 当时由于是ubuntu17，而17被ubuntu移到old-release，则需要修改数据源
vi /etc/apt/source-list  (替换zesty为对应版本别名即可)
deb http://old-releases.ubuntu.com/ubuntu/ zesty main restricted universe multiverse
deb http://old-releases.ubuntu.com/ubuntu/ zesty-security main restricted universe multiverse
deb http://old-releases.ubuntu.com/ubuntu/ zesty-updates main restricted universe multiverse
deb http://old-releases.ubuntu.com/ubuntu/ zesty-proposed main restricted universe multiverse
deb http://old-releases.ubuntu.com/ubuntu/ zesty-backports main restricted universe multiverse
deb-src http://old-releases.ubuntu.com/ubuntu/ zesty main restricted universe multiverse
deb-src http://old-releases.ubuntu.com/ubuntu/ zesty-security main restricted universe multiverse
deb-src http://old-releases.ubuntu.com/ubuntu/ zesty-updates main restricted universe multiverse
deb-src http://old-releases.ubuntu.com/ubuntu/ zesty-proposed main restricted universe multiverse
deb-src http://old-releases.ubuntu.com/ubuntu/ zesty-backports main restricted universe multiverse

2. 无法通过ssh连接
apt-get install openssh-server ufw
ufw enable
ufw allow 22

3. 创建用户
groupadd sct
useradd -d /home/sct -g sct -m sct -s /bin/bash

4. 更改dns
5. 增加80端口给pb


常用密码：
NSN1,.asr!Q145

requirement.txt
::

  robotframework==3.0.4
  robotframework-sshlibrary==3.0.0
  robotframework-httplibrary==0.4.2
  requests==2.11.1
  robotframework-requests==0.4.5
  robotframework-selenium2library==1.7.4
  robotframework-xvfb==1.2.2
  robotframework-archivelibrary==0.3.2
  selenium==2.53.4
  Flask==0.12
  paramiko==2.4.1
  elasticsearch==6.2.0
  robotframework-ride==1.5.2.1


Grammar
~~~~~~~~~~~

[documentation] : explain the test case
[tags] : sign the test case in which UC
[Setup][Teardown] : testcase前后置处理
[Template] : point to a testcase template
[Timeout] : set the Timeout value being run the testcase

${param} :  单个变量
@{list} ： 列表变量


requirement_id
CP3-US
collabrator 提交reviwer，mandentory需要，也可以发会议
之后提交QC
手工建立case，lab，等手动测试完标记pass


HWSCT

执行命令准确，同CI一致
scf放入oam_sct，名字要对
Strans的节点名字要跟cu建heat的一致
