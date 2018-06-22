Scrapy Framework
========================
介绍
~~~~~~~~~~~~~~~~~~~~~~~
Scrapy是一个为了爬取网站数据，提取结构性数据而编写的应用框架。 可以应用在包括数据挖掘，信息处理或存储历史数据等一系列的程序中。


**框架介绍**

实际Scrapy框架的主要构成以及数据流向

.. figure:: /_static/python/scrapy/architecture.png
  :width: 20.0cm

详细介绍过程
::

  1. 引擎从自定义爬虫中获取初始化请求（也叫种子URL）
  2. 引擎把该请求放入调度器中，同时引擎向调度器获取一个待下载的请求（这两部是异步执行的）
  3. 调度器返回给引擎一个待下载的请求
  4. 引擎发送请求给下载器，中间会经过一系列下载器中间件
  5. 这个请求通过下载器下载完成后，生成一个响应对象，返回给引擎，这中间会再次经过一系列下载器中间件
  6. 引擎接收到下载返回的响应对象后，然后发送给爬虫，执行自定义爬虫逻辑，中间会经过一系列爬虫中间件
  7. 爬虫执行对应的回调方法，处理这个响应，完成用户逻辑后，会生成结果对象或新的请求对象给引擎，再次经过一系列爬虫中间件
  8. 引擎把爬虫返回的结果对象交由结果处理器处理，把新的请求对象通过引擎再交给调度器
  9. 从1开始重复执行，直到调度器中没有新的请求处理

核心组件
::

  - Scrapy Engine：核心引擎，负责控制和调度各个组件，保证数据流转
  - Scheduler：负责管理任务、过滤任务、输出任务的调度器，存储、去重任务都在此控制
  - Downloader：下载器，负责在网络上下载网页数据，输入待下载URL，输出下载结果
  - Item Pipeline：负责输出结构化数据，可自定义输出位置
  - Spiders：用户自己编写的爬虫脚本，可自定义抓取意图
  - Downloader middlewares：介于引擎和下载器之间，可以在网页在下载前、后进行逻辑处理；例如设置代理或者切换IP
  - Spider middlewares：介于引擎和爬虫之间，可以在调用爬虫输入下载结果和输出请求/数据时进行逻辑处理

创建DEMO
~~~~~~~~~~~~~~~~~~~~~~
命令行创建Scrapy框架目录

::

  >>>scrapy startproject scrapy_name

.. figure:: /_static/python/scrapy/startproject.png
  :width: 20.0cm


目录介绍
~~~~~~~~~~~~~~~~~~~~~~~
**目录结构**

.. figure:: /_static/python/scrapy/construction.png
  :width: 20.0cm

* **spider**      存放实际爬虫代码
* **items**       存放最终输出内容的结构
* **middlewares** 下载中间件，主用用于代理，切换ip等
* **pipelines**   下载方式，可以定义输出json或者存入数据库
* **settings**    全局配置参数

Code
::

  import scrapy

  class QuotesSpider(scrapy.Spider):
    name = 'quotes'

    def start_requests(self):
        # 种子url
        urls = [
            'http://quotes.toscrape.com/page/1/',
            'http://quotes.toscrape.com/page/2/'
        ]
        for url in urls:
            yield scrapy.Request(url=url,callback=self.parse)

    def parse(self, response):
        # 解析response
        page = response.url.split('/')[-2]
        filename = 'quotes-%s.html' % page
        with open(filename,'wb') as f:
            f.write(response.body)
        self.log('Saved file %s' %filename)
