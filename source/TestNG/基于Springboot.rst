基于Springboot框架
===============================

介绍
~~~~~~~~~~~~~~~~~~~~

SpringBoot目前原生支持的为Junit4，但是针对TestNG也提供了继承方法AbstractTestNGSpringContextTests


Demo
~~~~~~~~~~~~~~~~~~~~~

::

  /**
  * 需要继承AbstractTestNGSpringContextTests
  * 同时指定配置文件ActiveProfiles
  */
  @SpringBootTest(classes = Application.class)
  @ActiveProfiles("test")
  public class MethodTestNGTest extends AbstractTestNGSpringContextTests{
    @Autowired
    private MethodService methodService;

    @Test(groups = "xxx")
    public void method1Test(){
        String result = methodService.method();
        Assert.assertEquals("xx",result);
    }
  }
