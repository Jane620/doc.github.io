PIT
========================================


介绍
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Plane Integration Tests - in this level we test all BTS SOM binaries/compnents + ADMIN binary as a part of e2e scenarios.


分析Startup_LWG_FbbaFbbc2FrgsAhdbRel3Wmp case
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Base Line
1. import 系统配置/环境配置，启动通用库/测试流程库
2. 创建配置 FSM/FSP[0]/FSP[2]/radios[0]
3.


Q&A
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
FSM : 对应SMOD，即system module，5g为asik
FSM.fsp[0]/[2] 对应BBMOD，5g为abil
omit :  The omit keyword shall only be used for optional fields
optLink - optIFnumber- cpriPort关系

为啥 setOptLink 时abil是用的optIF=8？
QSFP口id从0-7，因为ecpri走sfp，则为8
另外optlink从1开始，所以ecpri为9

info model before SOAM Must be attached to BM root object



K3 Env Setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. apply EE LinSEE access privilege
2. ...
3. if you logout from container , you should cd / source / reload ,then you can run cases again


TT-CN3
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Has two componets defined
MTC:  mtc is the component organizing the simulation , main test componet  主测模块
System: system component is an interface to the real world , real world指 BTSOAM(binary包)、CCS(hwapi)、bash
SUT: System Under Test , will be started by mtc
verdict: test result , pass/fail/inconc/none/error

Basic knowledge:
1. filter logs by grep ‘|tcfi|’ to see whether is this case fail or not
2.


Demo:
module MyTest{
  import from MyMtc all;
  import from MySystem all;

  testcase test() run on MyMtc system MySystem{
    //to do something
  }

  control {
    execute(test());
  }
}


Date type
=========================
var integer valueOfNum := 0;
var float valueOfFloat := 10.0;
var boolean valueOfBool := true/false;
var verdicttype valueOfResult := pass/fail/inconc/none/error;
var bitstring valueOfBit := '01101'B;
var hexstring valueOfBit := 'AB01'H;
var octetstring valueOfOct := 'FF96'O;
var charstring valueOfString := "a" / "b";

type charstring MyList("a","b","c");
type integer MyIntegerRanger(0..255);
type integer MyByte length(8);
