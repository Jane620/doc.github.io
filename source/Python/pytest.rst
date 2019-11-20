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



高级用法
~~~~~~~~~~~~~~~~~~~~
函数参数前置*，则可以使得后续的参数必须以键值对方式传入
即
def fun1(*args, arg1,arg2,arg3):
  pass

fun1(1,2,3) 不能直接这样使用, 需要func1(arg1=1,arg2=2,arg3=3)


PDB
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
调试工具，主要在assert等出错的地方进行调试检查参数变量值
pytest --pdb xxx.py
