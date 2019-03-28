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
3.
