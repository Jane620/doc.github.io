PIT Base Sill
===================================


Sublime text Plugins
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
0. 确保安装package control , 如果没有，则通过ctrl+shift+p 输入install package
1. 确保外网顺畅，不行则通过设置LAN， 配置为10.144.1.100:8080

sftp
1. package control 输入sftp 进行安装
2. 安装完成后，通过选中文件夹邮件remote mapping配置sftp的地址，注意修改用户密码，域名以及路径
3. 之后修改文件，可以通过右键upload file进行同步

BracketHighlighter 高亮括号匹配等
1. package control 输入BracketHighlighter 进行安装
2. 修改配置 "high_visibility_style":"solid" 显示实线， underline-下划线



PIT 硬件对应关系
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
optlinkModuleId
1. FSM_FSP1 ： fsp[0] , 依次同理


PIT case 基础函数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setupOpticalLinkFspRadio
1. aFspConf :  BBMOD部分，其中boardId从18开始
2. aRmConf : RMOD部分

入参EOptLink_x
对于接两根线，其中一个cpri和一个obsai，这个送入的之都是EOptLink_1，在代码逻辑中会对非eCPRI的值+1

PIT 代码和binary包获取路径
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
http://lteomci.inside.nsn.com:10101/trunk/view/00_TRUNK/view/11_COMMON+trunk/job/OAM+trunk.FSM4.WMP.ADMIN.PIT/

PIT 用法
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. 修改IM中cablnk_l对象的内容
var Object cablnk_l_cpri := InfoModelFunctions.getObject(cablinks[i].distname);
cablnk_l_cpri.object.cablnk_l.cablinkType := ECablinkType_CPRILink;
InfoModelFunctions.updateObject(cablnk_l_cpri);

2. 查找对应内容的distname
var charstring sDistName := "/MRBTS-1/RAT-1/BTS_L-1/EQM_L-1/RMOD_L-3/*";
var RoObjects cablinks := InfoModelFunctions.getObjectsByTemplate(templateName , proto_objects : {cablnk_l := cablnk_l_object(
        sourceDistName := sDistName,
        stateInfo_originatingStatus := EOriginatingStatus_detected
)});



PIT Case 痛点
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SCF file的准备
1. 准备一个真实的或者是现有的配置(主要关注cover点，配置可以不同，但是必须是要覆盖到这个点)
2. 按照原先的配置目录方式，放入scf，按照规则命名SCFS_1.xml
3. 拷贝到对应的data目录下
4. 执行 setupAdminEnv
5. 对整个trunk目录进行赋权， Chmod –R 777 .
6. 对SCF进行转换为ims2用于case的执行， ims-generator -d path
7. 正常情况会通跑一个commissioning的case，只有pass才会生成ims2

减少日志的输出到log文件
1. 修改tk3的文件，""中增加 -a "u_log" , 目前无效--

对于testcase execute timeout情形的问题排查
1. 某个Expectations的xx_event 没有找到对应的对象，这个时候需要增加log，检查传入的值，去查看对应的event matched
类似: [IM::expectation::single] event matched / [IM::expectation::interleaved] event matched
2.


PIT Case修改点
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. scf新增一个单abia-2fzhj
2. 如何将optlink改成cprilink
3. cablnk_L对象 如何进行修改为cpri ： 协议和其他什么属性？
4. ru_l对象的序列号修改 --ok


PIT start to Onair state
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
initialize
1. getRel() 获取fsm.hwRelease系统版本, 来源configuration
  1.1 configuration的来源为 createDefaultFsmAndFspsConfiguration()， 根据传入的smodtype定义hwRelease :=3/4
  1.2 ASIA/ASIK/ASIB/ASIKA 均为4 ， FSIH/FSMF 为3
2. setupFs
3. prepareTargetBdFile
4. prepareFileDirectoryFile
5. addK3SwConfigFlags
  5.1 其中getFcmFsPath 为啥在configuration中看fsm.fcm.boardId =16 ,结果ccsfs下却是0x(10)11，因为进行了一次十进制转十六进制，即转成10
6. createAndConfigureComponents
  6.1 createBaseComponents()
  6.2 connectEnvironmentPorts()
  6.3 componentsPreconfiguration()
    6.3.1 MHwApiOpt.setup()
        6.3.1.1 根据mConfiguration.fsm.optLinks[i].connectedDevice 判断是否有radio直接接入FSM
    6.3.2 MNokiaCpriRadio.setup()
    6.3.3 MHwApiAutodetection.setup()
  6.4 createFspComponents()
  6.5 connectEnvironmentPortsFsp()
  6.6 componentsPreconfigurationFsp()



New add CPRI Radio flow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. create new radio configuration, including thoes info as below
  1.1 radio variant
  1.2 radio serialnum
  1.3 radio ordernum
2. setupOpticalLinkFspRadio if radio connect to FSP
  2.1 connect fsp number
  2.2 fsp port
  2.3 radio configuration
  2.4 Rmod port number
3. set fsp[port].highLimit = 80 ??
4. MHwApiSum.setup  set mBbLinkTopologyMap value
5. MHwApiCpri


pypit start on air case flow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
testcase
	run_testcase
		scenario
			test
				strategy
					DefaultStrategy
						_run_until_timeout
							处理消息的获取和expectation tree校验


MNokiaCpriRadio goesup message for TDD_FDD
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
keyporint
1. VSB_DATA_IND_MSG is the message which contains details about the radio you want to detect.
2. in HAS/PLH point of view, the radio is already detected, and it's RUMAGS turn to do some updates already after RMOD becomes detected and stateInfo.proceduralState -> ethernetEnabled
3. RMOD becomes enabled when RU_L-1 is updated to enabled by RUMAG, PLH is just subscribing to that change

keyword:
1. processSendVsbDataReqNokiaLTE2294 aReqType is:
2. buildDefaultCpriVsbDataNokiaLTE2294 mac is:  /   buildDefaultCpriVsbDataNokiaLTE2294 mBoardOptLinks is:
fault: not send CpriRadioGoesUp message
flow1 : mBoardOptLinks[linkid].connectedDevice = 0, setupoptical的时候显示的为1 ?  --closed
    1. 跟踪到HwApiCpri.setup 时，fsp[0].optlink 状态ECpriState_A, connectedDevice=1 还是正常
    2. 后续connnectDevice两个一样了？
flow2:  根据马尼拉的意思是 VSB_DATA_IND_MSG 消息送入的mac地址是一样的，导致未检测到我需要起来的radio
    1. 检查buildDefaultCpriVsbDataNokiaLTE2294中的mac地址
    2. 检查fsp的connectDevice为何变化了，rc？
    3. 重新尝试add函数，将传入的mac地址增加一位，此处的参数应该不管radio类型，因为默认每个radio的mac地址都是从216开始
    4. 此时检查ims2中发现rmod_l-1020的存在, 但是后续并没有实际连上来，并且vsb的消息也已经发送多条不同的mac,等待下一步的检查
    5. 得知mo已建立，但是RU_L的状态不对，此处由rumag进行更新 , MRA/MRI 获取对应的pc跟sn

flow3: 退回到mo未创建的情况，其中检查CPRILink.isEnabled 状态 为false，引起没有触发执行到F态
    1. 增加打印，其中在HWApiCpri.setup中检查到已经将CPRILink_1设置为true了，但是后续又变成false了
    2. 检查打印 init mLinksState 1/2/3/4 is:  和  isEnabledLink mLinksState is:
    3. 从流程图中查看，是因为消息API_CPRI_SET_OUTPUT_REQ_MSG未收到引起的, 但实际查看oam的日志发现已经发出，且在pit的消息队列中
flow4: 尝试替换为CPRI/IR的radio
    1. waitForUdpPort 需要建立33333的udp端口 用于接受RFSW send的消息
    2. 检查发现replace后的configuration存在问题，似乎connectDevice以及fsp。optlinks[1] 错误了，应该是obsai的，结果也变成cpri？
    3. 送入正确的fsp[i] 后配置正常，增加udp的广播消息，让广播重发(init过程中将重发flag置为true了) ，目前已经创建mo
flow5: 目前detection_state = Done, operational_state = disable
    1. 当cpri link state reach to F ，PLH发起API2_ETH_PORT_CTRL_REQ_MSG， 作用未知？
    2. 缺少Radio MRI主动发起, 如何构建这条消息, 似乎为establishment替代了？
    3. 目前repalce函数中必须去掉cpriradio部分才能正常运行
    4. 不添加bm.init流程则最终依旧为rmod_L-1010, 添加显示为1020，但是sut崩溃
flow6: 替换为common的jenkins包后 IR替换IR 已经ok， 但是发现RMOD还是原先那个，不过sn变了？
    1. remove obsai ok，不过其中rmod disabled没有匹配到，奇怪？
    2. insert IR的过程中 提示no ECprilink，需要详细排查
    3. 检查发现RMOD_L没有创建，并且cablnk没有删除原先的RMOD_L-1的导致，在获取cablnk的时候，只取到了RMOD_L-1010
flow7: 尝试检查radiogoesup消息以及dhcp获取地址
    1. UDPCPConnector|Server address set in
    2. initAdetActions  即为nokiacpri的discovery消息，走的udpcp
    3. 添加map connect(hwapiCpriInFsp[0]:mHwApiCpri2NokiaTddCpriPort, nokiaCpriRadio[aCpriRmodOrderNumber]:mEnvPort); 失败，More than one route possible for transmit operation (ambiguous)
    4. 检查hotInsertCpriBmScenario ，似乎这里存在问题，导致后续没有走
flow8: 改变antenna的数量， 目前FZHS只支持2tx2rx, 并且修改cell carrier的bandwidth 10 -> 20
    1. 其中block跟unblock需要一起使用，只存在一个会影响到on air 过程
    2. 单覆盖PR的已经完成，除unblock过程有问题
    3. unblock、 switch的BBC timeout 问题需要一起跟踪，testplan根据unblock的进度进行调整
    4. all Done


VSB之前的set_output
1. 先发API_CPRI_SET_OUTPUT_REQ_MSG disable to hwapi , set linkstate from F to A
2. 再发API_CPRI_CONFIGURE_LINKS_REQ_MSG to RU ,
3. 之后再会发API_CPRI_SET_OUTPUT_REQ_MSG enable to api , linkstate will change to B -> C -> D/E -> F
4. sendRadiogoesup message ,wait radio start to receive


VSB_DATA flow
1. HWApiCpri.init()
2. handleSApiCpriSetOutputReq() ,wait for receive SApiCpriSetOutputReq
3. serveSApiCpriSetOutputReq(), handle function serveCpriSyncStateTransition()
4. make sure mLinksState.isEnable = true
  4.1 HWApiCpri.setup will change this state to true according to connectDevice
5. when link state change to C , sendVsbDataIndForNokiaCpriLTE2294()
6. handler function startSendingVsbDataIndForNokiaCpriLTE2294()
7. handler function se ndSApiCpriVsbDataIndForNokiaCpriLTE2294() send  SApiCpriVsbDataInd
8. then wait for oam plh deal with this ind and  send SApiCpriSendVsbDataReq back to HWAPiCpri to deal with handleSApiCpriSendVsbDataReq

Radiogoesup message

Protocol switch
1. 触发点未知, 目前尝试增加case时间到10mins
2. BBC会更新 START_DETECTION_TASK-1的结果，当fail则需要等待5分钟才会release ownership to DEM
3. DEM take over ownership and do something？ 之后也会释放ownership
4. PLH 接手 ownership ，进行接下来的radio detection过程
