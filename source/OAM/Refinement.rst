Refinement
================================


Background info
~~~~~~~~~~~~~~~~~~~~~
SCF+profile,scf定义过小，导致实际commiting到射频报错

Deal design
siteOAM 新增一个validation，用于校验profile的带宽，其中不在profile中新增flag
UI段需要对应的提示

profile根据硬件配置的带宽，检查scf提交的时候不满足则，前端block


1.3 OAM

background info
系统重启检查原因，是否要进行上报


设置关键词到pssw
oam@cu send requst ，pssw response

检测条件并将关键词暂存pssw
启动后获取应答
解析应答，进行上报
racoam

只针对classical
PSSW检测所有container的状态，由racoam订阅
当检测到异常(?)，之后向pssw设置关键词，即新增一个addition字段
发送重启(其他人处理)
多个container重启也只上报一次
racoam向pssw请求上报的消息，根据应答来决定是否上报，1.伪重启 2. 应用重启且为关键词才上报
