


切换环境
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. rm trunk目录
2. 从jenkins中找对应最新版本的 code以及binary
http://lteomci.inside.nsn.com:10101/trunk/view/00_TRUNK/view/11_COMMON+trunk/job/OAM+trunk.FSM4.WMP.ADMIN.PIT/
3. 进入k3环境处理
  1. cd /**/**/C_Test
  2. source k3setenv.env
  3. prepareToolsAndPlugins.sh CLEAN
  4. multibin getsut --validator-url http://lteomci.inside.nsn.com:10101/trunk/job/OAM+trunk.COMMON.COMMON.VALIDATOR/xxxx/
  5. k3env reload && prepareToolsAndPlugins.sh
  6. setupAdminEnv
  7. chmod -R 777 /var/fpwork/jwang/trunk/*
