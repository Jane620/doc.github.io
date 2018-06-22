MockTest Demo
======================

介绍
~~~~~~~~~~~~~~~~~~~~~


PythonDemo
~~~~~~~~~~~~~~~~~~~~
mock类目前已经整合入unittest中

::

  # 被测类和方法
  classc Count:

    def add(self):
      pass


  from unittest import mock
  import unittest
  class TestCount(unittest.TestCase):
    def test_count():
      count = Count()
      # 通过mock出add方法的返回值
      count.add = mock.Mock(return_value = 10)
      result = count.add(5,5)
      self.assertEquals(result, 10)


  if __name__ == '__main__':
      unittest.main()

Java Mockito
~~~~~~~~~~~~~~~~~~~~~
不支持对静态函数，构造函数，私有函数，final函数以及系统函数进行模拟

::

  pom.xml增加依赖
  <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>2.15.0</version>
  </dependency>
