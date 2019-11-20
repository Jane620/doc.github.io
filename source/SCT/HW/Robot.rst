EIT && HW-SCT TESTCASE
===============================================


Scope
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Verify ASIK(0x10)is ready
2. Verify ABIL(0x12)is ready
3. Verify ABIL service startup
4. Verify OAM lxc is up


 Tips
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
其中安装NbsLibrary的时候错误



TESTCASE run
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sudo pybot -L trace -v G_MP_HOST:10.108.154.69 -t "Verify ABIL service startup" -d results suites


TESTCASE Flow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ET SetUp
TESTCASE
ET TearDown


Verify ABIL service startup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  *** Settings ***
  Suite Setup       Connect To ASIK
  Suite Teardown    Close All Connections

  *** Variables ***
  # 检查服务状态 ， 并且只输出结果的4行内容
  ${S_ABIL_L1SW_CMD}    ssh fsp-1-1c systemctl status l1sw | awk 'NR<5{print}'
  ${S_ABIL_L1SW_NRT_CMD}    ssh fsp-1-1c systemctl status l1sw-nrt | awk 'NR<5{print}'
  ${S_ABIL_AUTODETECTION_CMD}    ssh fsp-1-1c systemctl status autodetection | awk 'NR<5{print}'

  *** Test Cases ***
  Verify ABIL Service Status
    [Tags]    level-EIT    status-done
    # 不再需要进入abil中
    # Connect To ABIL Loner
    Verify ABIL L1SW Status
    Verify ABIL L1SW NRT Status
    Veriry ABIL Autodetection Status
    Disconnect To ABIL Loner

  Verify ABIL L1SW Status
    Common SSH Cmd And Verify Rst In FSP    ${S_ABIL_L1SW_CMD}    ${S_ABIL_L1SW_EXPECTED_RST}

  Common SSH Cmd And Verify Rst In FSP
    [Arguments]    ${ssh_cmd}    ${expected_result}
    # 替换TAF的用法
    #${result}    TAF_SSH.Execute    ${ssh_cmd}    fsp
    ${result}    Execute Command    ${ssh_cmd}
    Should Match Regexp    ${result}    ${expected_result}


业务解释：

1. 几个服务 l1sw / l1sw-nrt /  autodetection 的启动命令或者方式  -- 硬件自动即启动服务
2. 检查服务是否启动的标记只有这个 loaded / active



单独执行本地的环境
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
MR的description 中添加一句话,否则进去测试池中通跑
OFFLINE_ENV 5gRobot110


针对NE3S的注册
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
分为非TLS，和TLS的两种模式
检查配置oamTls ,默认是forced ，则走的是HTTPS，即8443端口
oamTls=off,则走HTTP，端口8080


RobotFramework关键字作用域
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
当用到一个同名关键字时，Robot Framework 将要判断哪个关键字在它的作用域中拥有最高的优先级。一个关键的作用域取决于它是如何创建的，有如下几种方式:
 1、当前文件中创建的关键字。这类关键字最常用而且在所有同名关键字中拥有最高的优先级。
 2、一个源文件(resource)创建的关键字且直接或间接地被其它源文件引用。这类关键字拥有第二高的优先级。
 3、扩展测试库的关键字。当其它地方没有同名关键字时，这这些关键字将被使用。但是，如果在标准库中 有同名的关键字的话将会有警告提示出现。
 4、标准库中的关键字。这类关键字拥有最低的优先级。
