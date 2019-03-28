Communication-Related Knowledge
===============================

Architecture
~~~~~~~~~~~~~~~~~~~~~~

.. figure:: /_static/pic/Communication/5GCloud.png
  :width: 12.0cm

logical & physical

.. figure::/_static/pic/Communication/logicalandphysical.png
 :width: 12.0cm

SW Internal Intetface
.. figure::/_static/pic/Communication/ru+gnb.png
 :width: 12.0cm


4G
~~~~~~~~~~~~~~~~~~~~~~~~~~~
TD-LTE & FDD-LTE
LTE :  long term evolution


5G
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
5G: 5th Generation ,第五代
eNB: 用于4g的基站，涵软硬件
gNB: 用于5g的基站，涵软硬件
CU: central unit , 通用部分功能 , 控制一个或者多个DU通过F1接口进行交互
DU: distributed unit , 非通用部分功能, 支持多个cell


版本区别
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
5G18A受限于L1 capacity， L2-RT限制ASIK板的核心数
5G19至少达到ABIL-L1 两倍的容量，得益于 2 x Xeon D-1548 将ASIK 移到 ABIL，以增加L2-RT的能力
5G的突出表现建立在MZ框架上，SOAM的方式在平稳过渡




其他相关
~~~~~~~~~~~~~~~~~~~~~~~~~~~
空口: 移动设备和基站之间通讯的接口协议
BTS: Basci transfer station，基站
SDN ：软件定义网络 ,software defined network
vRAN：虚拟化无线电接入 , virtual radio access network
NFV: 网络功能虚拟化， network function virtulization
vIMS:虚拟化IP多媒体服务，virtual IP media service
vEPC：虚拟演进分组核心网，virtual Evolved Packet Core
EPS: evolved packet system , eps = epc + eutran
BIM: MZframe框架的一个类内存数据库，配置文件
CM: configuration managerment，配置管理, 目前跑在siteoam上
FM: fault managerment，错误管理，目前跑在racoam上N
AM: accounting management
PM: performance management
S: security , 以上简称FCAPS
RCP: radio cloud platform
RCPbm: radio cloud platform on bare metal
RP: radio platform
P-BIM: 硬件BIM，即profile
R-BIM: 资源BIM,即scf file , 只在siteoam上可读A
sAFe:
Quiz:
