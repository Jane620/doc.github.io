

chapter0
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
本书中任何一部分章节，都需要通过ut进行覆盖，并且需要满足代码行覆盖率>80%
pip install pytest-cover
pip install pytest


Chapter 1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
poker question
1. collections.namedtuple 用法, 可以当成一个小型的数据库
  namedtuple('desc',['arg1','arg2']) , 其中desc只是为了描述后面参数的一个命名而已, 类似namespace而已
2. list 可以直接通过 + 完成
3. str.split() 默认通过空格进行分割
4. genarator方式获取两个参数时，不需要额外的","之类进行分隔
   Card(rank,suit) for rank in ranks for suit in suits
5. 定义在class内的参数，都可以直接通过self. 方式引用
6. list[index]，其中index的获取实现通过内置方法 __getitem__ 实现
7. 随机获取则可以采用内置函数方法直接获得 random.choice
8. 只要实现了__getitem__方法，即可使得对象可迭代，反向迭代可以用reversed()
