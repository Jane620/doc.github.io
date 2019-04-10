
prepare work
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. 确定机框位置   gNB-DU ：WD03   Airframe   RU ：WF16
2. 提AMIA - ticket  https://srvjira.int.net.nokia.com/browse/LSDSUP-415626
3. 申请DongXin Lab access permission


Schedule
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. AMIA 已申请并安装
2. ABIL申请通过，在东信仓库，先不插AMIA
3. ASIK + AEQA 目前运输中  , AEQA 横着放
4. CU 已完成，确定能否ping通
5. EPC + LTE 申请 和 安装  参照： https://srvjira.int.net.nokia.com/browse/LSDSUP-414425  其中VLAN 都做Backhaul ，IP则从oam ip pool取
6. Robot PC  + ASIK + Switch 的VLAN
7. 申请ASIK 上EIF光口
8. PB ip
9. Robot PC 2 VLAN   use internal VLAN  seperate with internal and internal_svr




Q&A
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
0. guide
1. 拓扑图
https://confluence.int.net.nokia.com/display/5GHAN/How+to+set+up+a+gNB+for+ECE+CI
https://confluence.int.net.nokia.com/display/5GCCT/Network+configuraiton
2. ip申请的情况   LMP EIF  eNB +EPC  已经包含
3. CU的 setup & config
4. pb LOCATION  : http://10.57.241.24/pb_info/


DU - ASIK SetUp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Download from link: https://hzisoj70.china.nsn-net.net/artifactory/PCI-model-5g-package-local/5G18A_R4_B4_NSA_4.1044.1/
AirScaleASIK-4.1044.1-R4-B4-NSA.zip

guide ： https://confluence.int.net.nokia.com/display/~matthiro/Configure+the+LMP+IP+address


If ASIK type is A101 HW, this steps should be followed:

plug out ABIL, only keep ASIK in AMIA, if NOT, ABIL loner upgrade will be failed
plug cable directly between local PC and ASIK LMP, config local PC IP address, such as: 192.168.255.126/255.255.255.0 same network segment
enable SSH access: visit WebUI by default LMP IP, such as: https://192.168.255.129
yaft new version to ASIK before config ASIK LMP IP, and update YAFT tool to the newest first
check new version has been yaft to both /ffs/fs1 and /ffs/fs2, you can yaft twice to ensure it.
--modify ASIK LMP IP on /ffs/fs1/ip-config  and /ffs/fs2/ip-config
--crasign /ffs/fs1/ip-config  and /ffs/fs2/ip-config
reboot ASIK

X type
Config LMP ip:  https://confluence.int.net.nokia.com/display/~matthiro/Configure+the+LMP+IP+address
SSH ASIK then ping ABIK ip: 192.168.253.20, if ABIK is pingable, that means ASIK has identified ABIK.
Download YAFT project：https://svne1.access.nsn.com/isource/svnroot/BTS_T_YAFT/trunk/
Download AirScaleASIK-4.10012.*.zip: https://esisoj70.emea.nsn-net.net/artifactory/mnp5g-central-public/System_Release/
Run YAFT command from CMD: python yaft.py -R -i 10.106.241.171 -z AirScaleASIK-4.1012.238.zip
SSH ASIK LMP to check the BBP version: cat /etc/fsmpsl_version, such as MB_PS_LFS_OS_2018_02_0025 (fctl)
SSH ASIK to set DU ip: sudo vi /ffs/run/network.json, change DU ip according to your environment,such as 172.18.5.30
其中最后一步的ip取frontaul段中对应f1Mplane地址
连线中 EIF对应的为后续CU的frontaul的VLAN，需要提供光口位置以及VLAN进行绑定


DU - ABIL SetUp  10.56.6.5
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. 先直连ASIK 后，arp检测，会发现一般会有2个连接，一个是我电脑的，另一个应该是自身的
2. 如果确定是出厂版本，则插入ABIL，查看对应的arp，此时会增加fsp-1-1a/b/c ，表示一个abil，如果有多个则会出现2a/b/c
3. 更新



CU - AirFrame SetUp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

controller: 10.57.137.252   cn1sg13/Sgnokia13!

1. download  Addons and AirFrame.qcow2
2  解压Addons，取NCIR下的5G18A？
3. 安装模拟RU (本次不涉及)
4. 进入用户授权模式，source ~/xxrc  (例如这次为cn1sg13rc)
5. 创建镜像 openstack image create --disk-format=qcow2 --container-format=bare --file=AirFrameVirtualized-4.1044.28-R4-B4-NSA.qcow2 ft4.1044.28 (NCIR上检查镜像)
6. 注意修改flavor.yaml中的default ，因为可能默认被使用了，创建 flavor栈 heat stack-create -f 5gbts_flavor.yaml ECE_CI_flavor_789 (NCIR上检查栈)
7. 修改5gbts_ext_net.env 中frontaul && backaul , 并创建 heat stack-create -f 5gbts_ext_net.yaml -e 5gbts_ext_net.env ECE_CI_ext_nets_789 (NCIR上检查网络)
8. 修改5gbts_heat.env中vf_name (例如本次为fasttrack-789) 、image、Flavors、oam_network、backhaul and fronthaul、internal_provider_networks，并增加extra_cloud_config_data
9. neutron net-list | grep fast ，出来第一列为id，将其写入heat.env中的backhaul and fronthaul
10.注意flavor中的资源情况，确保申请的满足要求，创建stack: heat stack-create -f 5gbts_heat.yaml -e 5gbts_heat.env  ECE_CI_Stack_789    # -e wa_and_robot.env
11.修改SCF文件，修改对应s1和x2 的地址

Causion:
1. ext_net fronthaul不要使用192.168.1.x，需要采用192.168.2.x , 同时

EPC + LTE VM setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. apply EPC+LTE VM 参见
https://srvjira.int.net.nokia.com/browse/LSDSUP-414425
https://srvjira.int.net.nokia.com/browse/LSDSUP-413643
2. 根据ticket对应的地址和用户登录进行操作

EPC:
1.


RU SetUp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
follow this links : https://confluence.int.net.nokia.com/display/5GCCT/AEQA
查看HW的版本，然后找到对应的软件，查看RU下载的包中lzma的名字，需要一致


tips:
遇到创建失败的时候，需要排查具体的错误，可以查看事件，以及进行删除
heat stack-delete [stack名称]
heat stack-delete ECE_CI_Stack_789
查询最近一次stack错误  heat stack-show ECE_CI_ext_nets_789
查询目前的网络 neutron net-list | grep fasttrack-789
查看internal的vlan可以通过NCIR上admin->platform->provider networks 找对应的网段然后
ASIK上swconfig.txt 在/ffs/run  swconfig_local.txt 在/ffs/run/config/fct/


Need done today
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. robot pc install    -- new VM resources  OK
2. cable flag  -- AMIA + cpri  OK
3. guide
4. serial no
ASIK L1181907128
ABIL L1182710868
AEQA 6Q175002566


other operation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. ADD ssh-key from oam vm to ASIK ,so can ssh toor4nsn@192.168.2.60
2. scf file just need modify the backaul ip ,dont try to modify fronthaul , and fronthaul dont use the 192.168.1.x as ip
3.


How to setup a 5G cloud env (include CU + DU + RU + EPC/LTE and control pc)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
As present ,we use thoes HW/SW for this guide ,if your HW is not this kind of type , may need help.
AMIA : 203
DU : ASIK X42 , ABIL X21
RU : AEQA X11
AirFrame : AirFrameVirtualized-4.3002.6-CHN-ART01.qcow2
AirScale : AirScaleASIK-4.3002.6-CHN-ART01.zip / 4.32111.69/161 as middle fsmpsl_version


Prepare works
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. Have the lab access permission.
2. make sure the AMIA / CU / DU / RU location in Lab.
Example:
AMIA: https://srvjira.int.net.nokia.com/browse/LSDSUP-415626
CU:
3.


Apply for HW/SW
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. One Lab apply for ASIK + ABIL + AEQA
2. send email to


Setup CU stack (flavor/ext_net/vm_stack)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. Enter the controller pc use the dashboard account(in your apply info or email) to see if your resources is enough .
You need 20 cpu-cores and 100G ram as least.
2. Download the package as links: https://hzisoj70.china.nsn-net.net/artifactory/PCI-model-5g-package-local/5G18A_R4_B4_NSA_4.1044.1/, need AirFrameVirtualized + Addons + AirScaleASIK.
3. Enter the controller pc using shell ,copy Addons.xx.txz to /home/[username]/
source ~/[username]  (the one you use for shell to enter terminal, verification)
tar -xJvf Addons-4.1012.*.txz
tar zxvf HeatTemplates-AirFrame-config.tgz
cd NCIR/5G18A/
4. create external network using command heat stack-create -f 5gbts_ext_net.yaml -e 5gbts_ext_net.env ECE_CI_ext_nets_789
vi 5gbts_ext_net.env
change valeu of parameter number_of_networks to 2 and vnf_name to fasttrack-789
as default, oam part need delete , and fronthaul part should replace to 192.168.2.x (no match 192.168.1.x or 192.168.3.x is ok), backaul should replace to a very different ip like 172.18.10.x/165.165.166.x
5. when it done ,check the backaul and fronthaul id by neutron net-list | grep fasttrack-789, the firet value is id.
6. create flavor stack using command heat stack-create -f 5gbts_flavor.yaml ECE_CI_ext_flavor_789
vi 5gbts_flavor.yaml
change value of parameters default to anyone not like 5gbts , such as 5gbts_789
6. setup your base image use AirFrame.xx.qcow2 as recommand(such as we use AirFrameVirtualized-4.1044.1-R4-B4-NSA.qcow2)
openstack image create --disk-format=qcow2 --container-format=bare --file=AirFrameVirtualized-4.2113.404.qcow2 ft4.2113.404
7. create heat stack using command heat stack-create -f 5gbts_heat.yaml -e 5gbts_heat.env  ECE_CI_Stack_789
chang image / vnf_name / flavors (vnf_name + _oam/cpcn and so on) / oam_network / oam_ip / backhaul / fronthaul
7.1 if  there is real eNB , backhaul net id need change to oam net id (also need another two 大网ip for cpif and upue backhaul ip)
x. Dont forget to do workaround: https://confluence.int.net.nokia.com/pages/viewpage.action?spaceKey=CV&title=WA+for+Branch+master_cloudbts_asik_abil_l1r3+WA (CV44)


Setup DU (install ASIK / ABIL)
1. pull out the ABIL , make sure only your ASIK is in AMIA
2. because the ASIK is X type , so you can not use the neweast version ,need some middle version ,like 22111.63 -> 32111.69 -> 161
3. use the neweast yaft from svn :  https://svne1.access.nsn.com/isource/svnroot/BTS_T_YAFT/trunk/
4. run command as below , use the relative path in yaft fold
python yaft.py -R -i 192.168.255.1 -z AirScaleASIK-4.1044.1.zip
if success ，ssh to asik to see version in /ffs/fs2 and power off/on
5. plugin in ABIL ,wait for installing ,may take 15 mins , you will see ip 22 in asik by arp -a
6. Replace and rename Loner itb file by loner_Loner_2018_w13_01.itb  to /ffs/run/boardcfg/2/C/mcu-aspa/boot/5GL1SW.itb (other version has bin file)
7. crasign it
8. power off/on ,plugin out ABIL and yaft ASIK to 69
9. power off and plugin in ABIL and power on, waiting for install
10. power off/on ,plugin out ABIL and yaft ASIK to 161
11. power off and plugin in ABIL and power on, waiting for install
12. power off/on ,plugin out ABIL and yaft ASIK to AirScaleASIK-4.3002.6-CHN-ART01
13. power off and plugin in ABIL and power on, waiting for install
14. check l1sw : opkg list | grep l1sw  and  fpga : fpga_bitstream_version


Add CU to DU key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
at OAM:  cat /var/opt/nokia/lib/internalsshkeys/robot/id_rsa.pub
at Du : echo "host key" >> /user/toor4nsn/.ssh/authorized_keys
you can login in with:  ssh toor4nsn@192.168.2.60   from OAM VM
add CU cplane coredump flag : ssh cpif-0.local   && sudo bash && vi /rpram/swconfig.txt(may not exists)


workaround
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
delete rlan0 part and  change 'disableFrmSwAutoload' to true
/mnt/services/mzoam/config/4.xx/bims/pbim_repo.json  (when scf load ,this file will create)
/opt/nokia/SS_MzOam/cloud-siteoam/siteoam/profiles/5G18A/5g_pid11.json
/opt/nokia/SS_MzOam/cloud-siteoam/siteoam/profiles/5G18A/rap/5g_pid11.json


Setup RU
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
hwid.txt , install sw, patch flag  ASIK set ip / replace rlan0 to ethlmp0
开xml日志flag ： rad -pw 0x2c 1



Cloud 2 Environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
CU command:
heat stack-delete ECE_OAM_Sprint_TL1_Stack
heat stack-delete ECE_OAM_TL1_Stack
openstack image create --disk-format=qcow2 --container-format=bare --file=AirFrameVirtualized-4.311.297-R4-B4-NSA.qcow2 mu4.311.297
openstack image create --disk-format=qcow2 --container-format=bare --file=AirFrameVirtualized-4.316.113-R4-B4-NSA.qcow2 ae4.316.113
#heat stack-create -f 5gbts_ext_net.yaml -e 5gbts_ext_net.env ECE_OAM_TL1_ext_nets
heat stack-create -f 5gbts_flavor.yaml ECE_OAM_TL1_flavor
heat stack-create -f 5gbts_heat.yaml -e 5gbts_heat.env OAM_5G19_Stack26414
python yaft.py -R -i 192.168.255.1 -z AirScaleASIK-4.311.297-R4-B4-NSA.zip
python yaft.py -R --ignore_slaves -i 192.168.255.1 -z AirScale-0.135.472.zip --ask_for_password oZPS0POrRieRtu

heat stack-create -f 5gbts_flavor.yaml flavor19b
heat stack-delete OAM_5G19_Stack26414
heat stack-create -f 5gbts_heat.yaml -e 5gbts_heat.env OAM_5G19_Stack26414
openstack image create --public --disk-format=qcow2 --container-format=bare --file=AirFrameVirtualized-0.115.580.qcow2 5G19_0.115.580

DU step:
0. 对于A系列的板子，需要进行备份，记录fs1 && fs2
1. 新版本的asik未升级前，需要先检测一下abil，是否正常
2. 分开升级各部分
3. 升级asik后通过  export PYTHONPATH=/opt/hwmt/python; python /opt/hwmt/python/hwapi/AdetS.py -s  查看asik的状态 ，ready再继续
4. abil采用默认的loner即可，且一次升级到位，不用中间包 , 目前上海采用35_04的loner(暂不用，仅记录)
5. 查看loner版本:  5G18A-ReleaseR4-0.2.0-r4.19

Other：
1. 申请cu、lte、epc资源
2. 申请lte、epc的虚拟机，并绑定backhaul port
3. 申请ASIK上eif1口的vlan和交换机绑定关系
4. 申请gps
5. 申请链接ru的cpri线
6. 申请链接robot pc / RU / DU 之间的VLAN (cu资源应该包含3个internal vlan)


troubleshooting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. ASIK
      * M01 type can not
      * X type need some middler version to install ABIL
      * replace loner
2. ABIL
3. CU
    * modify the ext_net , fronthaul dont use the 192.168.1.x or 192.168.3.x ,backhaul use a different ip like 172.18 or 165.165
    * enter other VM: use ssh cpcl-0.local/cpif-0.local
    * see logs : journalctl -ab
4. RU
  * change the cpri optical fiber A -> B
  * dont forget to set the flag
  * CloudNodeOam.tgz need change rlan0 to ethlmp0 , for dhcp to give RU ip
  * with 2 abil , slot1 use  opt2 ,slot2 use opt1
5. controller pc: NCIR 17A ，so the image has some problem, need apply for other cloud resources not on WB15 and WB19
6. LTE : use LTEV5.03
set ENB1_X2_ADDR="165.165.166.20"  // LTE emu ip
set ENB2_X2_ADDR="165.165.166.3"  // backaul X2-CPIF ip
7. EPC :
8. other: asik + switch  if the speed is not match ,may cause port down.



DDR4
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
draw DDR4 time domain signal
1) install python 3.7 https://www.python.org/ftp/python/3.7.0/python-3.7.0.exe
2) install python lib matplotlib:  in Scripts directory  pip install --trusted-host pypi.dynamic.nsn-net.net --index-url http://pypi.dynamic.nsn-net.net/5g-testing-framework/tools matplotlib
3) python convert_snapshot_to_iq_and_plot_python2.py res.bin


目前
1. log check
2. script modify

before on-air log check
~~~~~~~~~~~~~~~~~~~~~~~
查询ASIK_master_Startup_DEFAULT
1. Sctp Setup Task Done!
查询CPIF
2. F1 Setup Task Done!
3. F1APCommonEnvelope , F1SetupRequest
查询cpcl
4. CP-Cell sending CuConfigurationUpdate to CP-IF F1AP
查询CPIF
5. X2 link is available
查询CPRT ，789均需要返回OK，同时4C2 返回4个subcell 0-3 ， 8C2返回8个 0-7
6. gnb cu configuration update from CU
7. L1Ul_SubcellSetupReq  , L1Dl_SubcellSetupReq
8. L1Ul_SubcellSetupResp , L1Dl_SubcellSetupResp
9. L2Lo_CellSetupResp , L2Ps_CellSetupResp
查询NOEOAM
10. cprt_cm_SAntennaCarrierActivationResp
查询CPRT
11. GnbCuConfigurationUpdateAck to CU
CloudNodeOam
12. AAHF载波激活log check ASIK_master_startup_NODEOAM.log
Activate carriers
Sending carrier activation to radio
carrierActivate:SOAP reply OK


BTS startup and cell setup logs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
查询racoam.log
1. 0x6001 CPCM_CONFIGURATION_REQ_MSG
3. 0x6003 CPCM_NETWORK_PLAN_REQ_MSG
5. 同3， 不过额外收到一条 CPCM_PLAN_ACTIVATION_COMPLETE_IND  消息
8. 0x6006 CPCM_TRSW_CONFIGURATION_REQ_MSG
10.0x6008 CPCM_POOL_CONFIGURATION_REQ_MSG (L2_NRT,L3_NB,L3_CL,L3_UE,L3_IF,)
12.0x601a CPCM_DEPLOYMENT_COMPLETE_IND
查询cpif
13.CpIfDeploymentCompleteInd
查询CPRT (INF/NODEOAM/mescom:-->表示nodeoam发出的mescom消息，对应的接受者看receiver，可以通过去profile中查询cpid)
14.0x7200 CPRT_CM_CONFIGURATION_REQ_MSG
查询nodeoam
16.0xa01 CPRI_CONFIGURE_LINKS_REQ  (针对MUMIMO则会出现两次消息)
18.0xa08 CPRI_SUBSCRIBE_REQ (link_id 0/1/2/3)
20.0xa05 CPRI_SET_OUTPUT_REQ
22.CPRI_STATE_INDICATION   收到消息
查询CPRT
25.cprt::cm::SNetworkPlanReq
26.cprt::cm::SNetworkPlanResp
30.cprt::cm::SPoolConfigurationReq
31.cprt::cm::SPoolConfigurationResp
查询nodeoam
32.0x2bdb modifyParameterReq
38.0x7206 CPRT_CM_CELL_MAPPING_REQ_MSG
40.cprt_cm_SCellConfigUpdateReq
查询CPRT
42.Sctp Setup Task Done!
47.F1 Setup Task Done!
查询CPIF
48.CpNbF1LinkInfoUpdateConfirm
55.EndcX2SetupRequest / EndcX2SetupResponse
59. X2 link is available ?
60.CpIfSendToDu
SP: 检查FTMUMIMOL1AddressDlDataProc，出现过ULdata address exchange问题
查询CPRT
61.Received gnb cu configuration update from CU
查询nodeoam
64.0xa0d CPRI_DELAY_CONFIG_REQ
66.0xa0f CPRI_GET_LINK_PARAM_REQ
76.CPRI_DELAY_CONFIG_RESP
查询CPRT
78.L1Dl_SubcellSetupReq
79.L1Ul_SubcellSetupReq
82.L2Ps_CellSetupReq
83.L2Lo_CellSetupReq
88.L2Ps_CellSetupResp
89.L2Lo_CellSetupResp
90.cprt::cm::SAntennaCarrierActivationReq
94.itf::l2::ps::cell::SystemInfoConfigurationReq
96.common::f1ap::GnbCuConfigurationUpdateAck to CU

查看L1Runtime
65.IW_CPRI_DELAY_CONFIG_RESP
67.send_cpri_resp sent IW_CPRI_CONFIGURE_LINKS_RESP to
80.process_setup_req iw_dl_cell_setup_req completed for subcell_id:
86.process_address_req iw_dl_data_address_req subcell_id:  (Q:为何subcellid 是0/2/4/6)
查询nodeoam
100.Sending carrier activation to radio
101.carrierActivate:SOAP reply OK

on-air后排查
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. re-configuraiton ,当nodeoam到rru的心跳检测超时，则会发起
2. on air状态实际要看载波激活，即nodeoam发送Sending carrier activation to radio


新 DDR4

scp snapshot.py 192.168.253.22:
ssh 192.168.253.26 "
rm -rf /usr/lib/python/snapshot.py;
rm -rf /usr/lib/libsnapshot.so.3*;
mv snapshot.py /usr/lib/python/;
mv libsnapshot.so.4 /usr/lib/;
chmod +x /usr/lib/python/snapshot.py;"

1. 解析leka
2. 抓on air后wireshark eth0 && rlan0
3. 分析eth0的pcap ，按理应该有
PDSCHsendreq -l2ps, PDSCHpayload -l2lo


只读系统重新挂载，需要删除文件
~~~~~~~~~~~~~~~~~~~~~~~~~~~
mount -o rw,remount /ffs/fs1
mount -o rw,remount /ffs/fs2


抓gnb的包
-i 为对应的出入口 host则为需要过滤的ip
sudo tcpdump -i enp0s25 host 10.108.240.197 -w dump.pcap

wireshark
~~~~~~~~~~~~~~~~~~~~~~~~~~~
RRC 4/5 为CU-DU
RRC14/15 CU-L2
RRC 6/7 DU-L1


100M
1个PRB有12个子载波
子载波间隔分30K或者15K等
按照100M计算 30K则有273个PRB
1070 DDDSU UUDDD
1116 DDDDSUUDDD
1208 DDDSU

5GRAU
L2LO,L2PS,L1DL,L1UL

5GRAC
L2Hi,TRSW

疑问
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
vtk用途，和NE3S以及nbs的关系
CU侧开8080，robot侧开8088
netact
NoMA
tcpdump -i enp0s25 host 10.108.240.197 -w cu_side.pcap
tcpdump -i host 10.108.154.54 -w du_side.pcap
netstat -ntulp |grep 8080
tar -xJvf Addons-4.2113.404.txz && tar zxvf HeatTemplates-AirFrame-config.tgz

检查设备
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
traceroute target -T -P 8080
telnet target 8080
关闭防火墙 sudo ufw disable


cp3+ac
ac->testplan

独立完成单个feature测试

1. 如何找feature

2. 定schedule
目前基本所有任务都在一个FB内完成，所以对于太大的，需要进行拆分
3. 等待CP3完成后准备testplan
testplan模板进行填写case



FB4
1187-c   1905 open
1427-A-a 1905 plan
716      1905 plan     719-9/1/14/12   testplan/manul && TA for 9/1
