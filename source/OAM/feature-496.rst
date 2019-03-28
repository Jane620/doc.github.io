RAP classical BTS  cloud DU


426 地址
540 cfm security
1082 sw download

093
863
Cloud环境搭建
Ci环境
TA


硬件不需要 , 机器在东信..申请实验室权限
只要申请资源
了解各种ip的使用
测试驱动




1. 调试case 4        重要紧急     要环境以及case4的后续支持  目前还要在原先EIT Regression环境进行调试 ，只要不涉及download部分的case
降级目前robot1环境，由于版本问题引发case4 错误
2. cloud环境搭建     重要不紧急   通读guide，检查哪些需要后续准备的
3. 编写phase2的case  重要不紧急   可以准备一下
4. feature 789/093/863          完成1,3后可以开始准备了解


EIT Regression环境
ASIK地址 10.108.154.69  toor4nsn / oZPS0POrRieRtu


HW-SCT 环境  /ffs/run
ASIK地址 192.168.255.1
control pc机： 5gRobot1.srnhz.nsn-rdnet.net
降级使用的账号(务传)： sct/NSN1,.asr!Q145
WebUIIP  robot1 - 10.108.154.66 / robot3 - 10.108.154.65
升级需要预定资源： http://maccimaster.emea.nsn-net.net/res/index.php
cd Yaft
python yaft.py -i 192.168.255.1 -z ../AirScaleASIK-4.83.58.zip
检查ip , /ffs/run/config  下network.json
执行完成后进入ASIK对应的ip下检查lxc-ls 服务是否启动，以及pgrep OAM

robot1   回退 72环境到58版本  下包，以及执行降级
robot3   作为对照组, 命令执行正常
