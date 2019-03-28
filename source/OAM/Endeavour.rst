ENdeavour SCT testing tool
=========================================


Install
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

参考：`how to setup endeavour env.<https://confluence.int.net.nokia.com/display/Endeavour/How+to+setup+testing+environment+to+own+PC>`_

1. 下载 siteoam/racoam/nodeoam/oamagentjs/endeavour-server 的代码(需要注意版本)，传入mint虚拟机中
2. 进入每个目录下安装npm依赖包，npm install
3. 之后安装PDE,PDE是一个用于endeavour的消息编码/解码工具
4. 设置虚拟机通讯endeavour的端口，输出和输入
5. 启动endeavour-server服务， NODE_ENV=5gMutConfig node src/main.js
6. 下载Email.zip和Endeavour testcase ,将Email解压到case目录下
7. 启动Email.exe，设置sack和profile路径为testcase下的resource以及resource/profile/SBTS_MT
8. 引入sack和profile配置，选中对应的sacks，点击load
9. 配置endeavour-server连接配置，新建一个，然后填写对应的地址端口等
10. 通过Email启动endeavour
11. 引入testcas，选择某一个，点击运行
12. 正常情况会同虚拟机的server进行交互，产生对应的日志
