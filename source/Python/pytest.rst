Pytest使用
====================================


Setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  #建议使用python3.6以上
  pip install -U pytest



Demo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. 建一个case目录，文件命名要求必须为test_xxx.py ，否则pytest无法识别
2. 建几个测试函数，包含assert
3. 进入case目录下，执行pytest ，自动扫描目录下test的case，并检查assert
