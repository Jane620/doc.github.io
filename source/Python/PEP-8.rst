


Nameing Style
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Function  function/my_function
Variable  x/var/my_variable
Class     Model/MyClass
Method    class_method/Method
Constant  CONSTANT/MY_CONSTANT
Module    mudule.py/my_mudule.py
package   package/mypackage


How to Choose Names
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
尽量一直使用最简洁且最具描述性的名称



私有变量的使用
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
私有变量通常采用_开头, 其中可以通过property装饰器将私有变量变为公有变量使用
好处是，property实现了setter和getter方法,并且也隐藏了实际的共有变量的来源

def __init__(self):
  self._name = "a"
  self._account = Account(
       name = self.name
       )

@property
def name(self):
  return self._name
