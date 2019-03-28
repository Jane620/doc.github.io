RobotFramework
================================

介绍
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ride的wxPython的安装问题
~~~~~~~~~~~~~~~~~~~~~~~~
添加源
echo "deb http://archive.ubuntu.com/ubuntu wily main universe" | sudo tee /etc/apt/sources.list.d/wily-copies.list
sudo apt update
apt install python-wxgtk2.8
rm /etc/apt/sources.list.d/wily-copies.list
apt update


依赖库
~~~~~~~~~~~~~~~~~~~~~~~~~
robotframework==3.0.4
robotframework-archivelibrary==0.4.0
robotframework-httplibrary==0.4.2
robotframework-requests==0.4.7
robotframework-ride==1.5.2.1
robotframework-selenium2library==1.7.4
robotframework-seleniumlibrary==3.1.1
robotframework-sshlibrary==3.0.0
robotframework-xvfb==1.2.2
selenium==2.53.4

Flask==0.12
paramiko==2.4.1
elasticsearch==6.2.0
requests==2.11.1


缺失部分lib需要安装
NbsLibrary  10.69.239.163 ute/ute 的/opt/pypiserver/packages/nbs-EIT-1.tar.gz    pip install nbs-EIT-1.tar.gz
再不行，则直接拷贝现有环境的site-package下的nbs ,NbsLibrary和nsbtool 三个目录到运行环境下
TAF?  `<https://confluence.int.net.nokia.com/display/CV/TAF+Environment+set+up>`_
ntplib
pytz
packaging
netifaces

安装NbsLibrary的时候遇到的一些问题
python-dateutil
pip install google  && pip install protobuf
pip isntall jinja2

常用命令
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
单独执行case

G_MP_HOST=10.108.154.69
G_NETACT_HOST=10.135.157.122
TESTCASE_NAME=$1 (传入的参数需要将空格去掉)

sudo pybot -L trace -v G_MP_HOST:${G_MP_HOST} -t ${TESTCASE_NAME} -d results suites



查看版本号

::

  pybot --version




基于虚拟现实的浏览器自动化测试操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
安装robotframwork-xvfb, 可以以后台方式启动浏览器进行selenium2library的相关操作
