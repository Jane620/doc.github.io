
程序员的数学
page: 137


0的故事
~~~~~~~~~~~~~~~~~~~~~
1. 指数法则
10的2次=10*10 = 100, 10的3次= 10*10*10 =1000, 按照普通理解 10的0次 = 10*0 = 0?， 事实上10的0次=1
正确理解指数，应该按照这个来
10的3次 = 1000
10的2次 = 100
10的1次 = ?
可以看出按照指数的减少(每次-1), 都是上一个指数的1/10, 因此10的0次应该是10的1次的1/10， 即可10*（1/10） =1
同理看出， 10的-1次 = ？， 应该是1*（1/10） = 1/10
2. 0的作用
在10进制中 :占位
指数中： 标准化， 否则就要单独写个1来表示指数的0次幂

目前时钟的60源自古巴比伦60进制算法


逻辑True/false
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
文式图 / 卡诺图
没有遗漏，则具备完整性
没有重复，则具备排他性

异或运算 : 仅当两个值不同时为true
python的异或为:
if  f_flag ^ t_flag: print('111')
else: print('222');


余数
~~~~~~~~~~~~~~~~~~~~~~~~~~~
假设今天是星期日，100天后是星期几
100/7=14余2，因此是星期二
那么求10的100次方式星期几？
按照0/10/100/1000/10000等找规律，按照0的个数来确定规律后，按照100个0/规律0 = 余数0则为星期几

欧拉公式中奇偶校验, 解决哥尼斯堡的桥问题
1. 只有起点终点为奇点
2. 其余点为偶点
3. 经过的每一个偶点，必定存在两条或倍数的边

证明公式
1. 证明满足f(1) 满足
2. 证明若f(k)成立，f(k+1)也成立 , 常为假设f(k)成立，则在基础上增加k+1


数学中的置换
3个扑克牌的排列：
  第1张的取法有3种
  第2张的取法只有2种（不存在重复的）
  第1张的取法仅1种
  即 3+2+1 = 6 等同 3X2X1 = 3！
同理可得 5张扑克牌为 5！

同上，5张扑克牌中取3张的排列有多少种
  第1张的取法有5张
  第2张的取法有4张
  第1张的取法有3张
即 5X4X3 = 60 （此处并非5+4+3，因为3,2，1的时候只是凑巧乘法加法两者一样，事实上依旧是按照乘法来的）
推导 n张中取k张的排列
(n-0)*(n-1)*...*(n-(k-1)) = n*(n-1)*...*(n-k+1)
P = n! / (n-k)!
