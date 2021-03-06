##Java 回调机制(CallBack) 趣解
```

最近学习java，接触到了回调机制(CallBack)。初识时感觉比较混乱，而且在网上搜索到的相关的讲解，要么一言带过，要么说的比较单纯的像是给CallBack做了一个定义。当然了，我在理解了回调之后，再去看网上的各种讲解，确实没什么问题。但是，对于初学的我来说，缺了一个循序渐进的过程。此处，将我对回调机制的个人理解，按照由浅到深的顺序描述一下，如有不妥之处，望不吝赐教！

开始之前，先想象一个场景：幼稚园的小朋友刚刚学习了10以内的加法。 第1章. 故事的缘起

幼师在黑板上写一个式子 “1 + 1 = ”，由小明同学来填空。

由于已经学习了10以内的加法，小明同学可以完全靠自己来计算这个题目，模拟该过程的代码如下：
public class Student
 {
     private String name = null;

     public Student(String name)
     {
         // TODO Auto-generated constructor stub
         this.name = name;
     }

     public void setName(String name)
     {
         this.name = name;
     }

     private int calcADD(int a, int b)
     {
         return a + b;
     }

     public void fillBlank(int a, int b)
     {
         int result = calcADD(a, b);
         System.out.println(name + "心算:" + a + " + " + b + " = " + result);
     }
 }
小明同学在填空(fillBalnk)的时候，直接心算(clacADD)了一下，得出结果是2，并将结果写在空格里。测试代码如下：


public class Test
  {
     public static void main(String[] args)
     {
        int a = 1;
         int b = 1;
         Student s = new Student("小明");
         s.fillBlank(a, b);
     }
 }
运行结果如下：

小明心算:1 + 1 = 2

该过程完全由Student类的实例对象单独完成，并未涉及回调机制。 第2章. 幼师的找茬

课间，幼师突发奇想在黑板上写了“168 + 291 = ”让小明完成，然后回办公室了。

花擦！为什么所有老师都跟小明过不去啊？明明超纲了好不好！这时候小明同学明显不能再像上面那样靠心算来完成了，正在懵逼的时候，班上的小红同学递过来一个只能计算加法的计算器（奸商啊）！！！！而小明同学恰好知道怎么用计算器，于是通过计算器计算得到结果并完成了填空。

计算器的代码为：
public class Calculator
 {
     public int add(int a, int b)
     {
         return a + b;
     }
 }
修改Student类，添加使用计算器的方法：


public class Student
 {
     private String name = null;

     public Student(String name)
     {
         // TODO Auto-generated constructor stub
         this.name = name;
     }

     public void setName(String name)
     {
         this.name = name;
     }

     @SuppressWarnings("unused")
     private int calcADD(int a, int b)
     {
         return a + b;
     }

     private int useCalculator(int a, int b)
     {
         return new Calculator().add(a, b);
     }

     public void fillBlank(int a, int b)
     {
         int result = useCalculator(a, b);
         System.out.println(name + "使用计算器:" + a + " + " + b + " = " + result);
     }
 }
测试代码如下：


public class Test
 {
     public static void main(String[] args)
     {
         int a = 168;
         int b = 291;
         Student s = new Student("小明");
         s.fillBlank(a, b);
     }
 }
运行结果如下：

小明使用计算器:168 + 291 = 459

该过程中仍未涉及到回调机制，但是部分小明的部分工作已经实现了转移，由计算器来协助实现。 3. 幼师回来了

发现小明完成了3位数的加法，老师觉得小明很聪明，是个可塑之才。于是又在黑板上写下了“26549 + 16487 = ”，让小明上课之前完成填空，然后又回办公室了。

小明看着教室外面撒欢儿的小伙伴，不禁悲从中来。再不出去玩，这个课间就要废了啊！！！！ 看着小红再一次递上来的计算器，小明心生一计：让小红代劳。

小明告诉小红题目是“26549 + 16487 = ”，然后指出填写结果的具体位置，然后就出去快乐的玩耍了。

这里，不把小红单独实现出来，而是把这个只能算加法的计算器和小红看成一个整体，一个会算结果还会填空的超级计算器。这个超级计算器需要传的参数是两个加数和要填空的位置，而这些内容需要小明提前告知，也就是小明要把自己的一部分方法暴漏给小红，最简单的方法就是把自己的引用和两个加数一块告诉小红。

因此，超级计算器的add方法应该包含两个操作数和小明自身的引用，代码如下：
public class SuperCalculator
 {
     public void add(int a, int b, Student  xiaoming)
     {
         int result = a + b;
         xiaoming.fillBlank(a, b, result);
     }
 }
小明这边现在已经不需要心算，也不需要使用计算器了，因此只需要有一个方法可以向小红寻求帮助就行了，代码如下：


public class Student
 {
     private String name = null;

      public Student(String name)
     {
         // TODO Auto-generated constructor stub
         this.name = name;
     }

     public void setName(String name)
     {
         this.name = name;
     }

     public void callHelp (int a, int b)
     {
         new SuperCalculator().add(a, b, this);
     }

     public void fillBlank(int a, int b, int result)
     {
         System.out.println(name + "求助小红计算:" + a + " + " + b + " = " + result);
     }
 }
测试代码如下：


public class Test
  {
      public static void main(String[] args)
      {
          int a = 26549;
          int b = 16487;
          Student s = new Student("小明");
          s.callHelp(a, b);
      }
  }
运行结果为：

小明求助小红计算:26549 + 16487 = 43036

执行流程为：小明通过自身的callHelp方法调用了小红（new SuperCalculator()）的add方法，在调用的时候将自身的引用（this）当做参数一并传入，小红在使用计算器得出结果之后，回调了小明的fillBlank方法，将结果填在了黑板上的空格里。

灯灯灯！到这里，回调功能就正式登场了，小明的fillBlank方法就是我们常说的回调函数。

通过这种方式，可以很明显的看出，对于完成老师的填空题这个任务上，小明已经不需要等待到加法做完且结果填写在黑板上才能去跟小伙伴们撒欢了，填空这个工作由超级计算器小红来做了。回调的优势已经开始体现了。 第4章. 门口的婆婆

幼稚园的门口有一个头发花白的老婆婆，每天风雨无阻在那里摆着地摊卖一些快过期的垃圾食品。由于年纪大了，脑子有些糊涂，经常算不清楚自己挣了多少钱。有一天，她无意间听到了小明跟小伙伴们吹嘘自己如何在小红的帮助下与幼师斗智斗勇。于是，婆婆决定找到小红牌超级计算器来做自己的小帮手，并提供一包卫龙辣条作为报酬。小红经不住诱惑，答应了。

回看一下上一章的代码，我们发现小红牌超级计算器的add方法需要的参数是两个整型变量和一个Student对象，但是老婆婆她不是学生，是个小商贩啊，这里肯定要做修改。这种情况下，我们很自然的会想到继承和多态。如果让小明这个学生和老婆婆这个小商贩从一个父类进行继承，那么我们只需要给小红牌超级计算器传入一个父类的引用就可以啦。

不过，实际使用中，考虑到java的单继承，以及不希望把自身太多东西暴漏给别人，这里使用从接口继承的方式配合内部类来做。

换句话说，小红希望以后继续向班里的小朋友们提供计算服务，同时还能向老婆婆提供算账服务，甚至以后能够拓展其他人的业务，于是她向所有的顾客约定了一个办法，用于统一的处理，也就是自己需要的操作数和做完计算之后应该怎么做。这个统一的方法，小红做成了一个接口，提供给了大家，代码如下：
public interface doJob
 {
     public void fillBlank(int a, int b, int result);
 }

因为灵感来自帮小明填空，因此小红保留了初心，把所有业务都当做填空（fillBlank）来做。

同时，小红修改了自己的计算器，使其可以同时处理不同的实现了doJob接口的人，代码如下：
public class SuperCalculator
 {
     public void add(int a, int b, doJob  customer)
     {
         int result = a + b;
         customer.fillBlank(a, b, result);
     }
 }
小明和老婆婆拿到这个接口之后，只要实现了这个接口，就相当于按照统一的模式告诉小红得到结果之后的处理办法，按照之前说的使用内部类来做，代码如下：

小明的：
public class Student
  {
     private String name = null;

     public Student(String name)
      {
          // TODO Auto-generated constructor stub
          this.name = name;
      }

     public void setName(String name)
     {
         this.name = name;
     }

     public class doHomeWork implements doJob
     {

         @Override
         public void fillBlank(int a, int b, int result)
         {
             // TODO Auto-generated method stub
             System.out.println(name + "求助小红计算:" + a + " + " + b + " = " + result);
         }

     }

     public void callHelp (int a, int b)
     {
         new SuperCalculator().add(a, b, new doHomeWork());
     }
 }
老婆婆的：


public class Seller
{
    private String name = null;

    public Seller(String name)
    {
        // TODO Auto-generated constructor stub
        this.name = name;
    }

    public void setName(String name)
    {
        this.name = name;
    }

    public class doHomeWork implements doJob
    {

        @Override
        public void fillBlank(int a, int b, int result)
        {
            // TODO Auto-generated method stub
            System.out.println(name + "求助小红算账:" + a + " + " + b + " = " + result + "元");
        }

    }

    public void callHelp (int a, int b)
    {
        new SuperCalculator().add(a, b, new doHomeWork());
    }
}
测试程序如下：


public class Test
{
    public static void main(String[] args)
    {
        int a = 56;
        int b = 31;
        int c = 26497;
        int d = 11256;
        Student s1 = new Student("小明");
        Seller s2 = new Seller("老婆婆");

        s1.callHelp(a, b);
        s2.callHelp(c, d);
    }
}
运行结果如下：

小明求助小红计算:56 + 31 = 87
老婆婆求助小红算账:26497 + 11256 = 37753元 最后的话

可以很明显的看到，小红已经把这件事情当做一个事业来做了，看她给接口命的名字doJob就知道了。

有人也许会问，为什么老婆婆摆摊能挣那么多钱？ 你的关注点有问题好吗！！这里聊的是回调机制啊！！

我只知道，后来小红的业务不断扩大，终于在幼稚园毕业之前，用挣到的钱买了人生的第一套房子。

```



##Spring中Quartz的配置

```
Quartz是一个强大的企业级任务调度框架，Spring中继承并简化了Quartz，下面就看看在Spring中怎样配置Quartz：
首先我们来写一个被调度的类：
public class QuartzJob {
public void work() {
System.out.println("Quartz的任务调度！！！");
}
}
Spring的配置文件quartz-config.xml：
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" "http://www.springframework.org/dtd/spring-beans.dtd">
<beans>    
        <!-- 要调用的工作类 -->
        <bean id="quartzJob" class="com.kay.quartz.QuartzJob"></bean>
        <!-- 定义调用对象和调用对象的方法 -->
        <bean id="jobtask" class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
            <!-- 调用的类 -->
            <property name="targetObject">
                <ref bean="quartzJob"/>
            </property>
            <!-- 调用类中的方法 -->
            <property name="targetMethod">
                <value>work</value>
            </property>
        </bean>
        <!-- 定义触发时间 -->
        <bean id="doTime" class="org.springframework.scheduling.quartz.CronTriggerBean">
            <property name="jobDetail">
                <ref bean="jobtask"/>
            </property>
            <!-- cron表达式 -->
            <property name="cronExpression">
                <value>10,15,20,25,30,35,40,45,50,55 * * * * ?</value>
            </property>
        </bean>
        <!-- 总管理类 如果将lazy-init='false'那么容器启动就会执行调度程序  -->
        <bean id="startQuertz" lazy-init="false" autowire="no" class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
            <property name="triggers">
                <list>
                    <ref bean="doTime"/>
                    <!-- 如果有多个触发时间，都可以在此处定义。
                    <ref bean="doTime2" /> -->
                </list>
            </property>
        </bean>
    
</beans>
定义触发时间还可以用SimpleTriggerBean
<bean id="doTime2" class="org.springframework.scheduling.quartz.SimpleTriggerBean">
  <property name="jobDetail"><ref bean="jobtask1"></ref></property>
  <!-- 服务器启动后延迟2s后再执行定时任务-->
  <property name="startDelay" value="2000"></property>
  <!-- 定时任务每隔3s执行一次-->
  <property name="repeatInterval" value="3000"></property>
 </bean>
测试程序：
我们需要把log4j的配置文件放入src目录下，启动main类就可以了。
public static void main(String[] args) {
System.out.println("Test start...");
ApplicationContext context = new ClassPathXmlApplicationContext("quartz-config.xml");
// 如果配置文件中将startQuertz bean的lazy-init设置为false 则不用实例化
// context.getBean("startQuertz");
System.out.print("Test end...");
}
```


##关于cron表达式
```
Cron 表达式包括以下 7 个字段：
秒  分  小时  月内日期  月周内日期  年(可选字段) 特殊字符
Cron 触发器利用一系列特殊字符，如下所示：
/ 字符 表示增量值。例如，在秒字段中"5/15"代表从第 5 秒开始，每 15 秒一次。
? 字符 和 L 字符只有在月内日期和周内日期字段中可用。
    ? 表示这个字段不包含具体值。所以，如果指定月内日期，可以在周内日期字段中插入“?”，表示周内日期值无关紧要。
    L 是 last 的缩写。放在月内日期字段中，表示安排在当月最后一天执行。
        在周内日期字段中，如果“L”单独存在，就等于“7”，否则代表当月内周内日期的最后一个实例。
        所以“0L”表示安排在当月的最后一个星期日执行。
    W 用在月内日期字段中，表示把执行安排在最靠近指定值的工作日。
        如“1W”放在月内日期字段中，表示把执行安排在当月的第一个工作日内。
    # 表示给定月份指定具体的工作日实例。
        如“MON#2”放在周内日期字段中，表示把任务安排在当月的第二个星期一。
    * 是通配字符，表示该字段可以接受任何可能的值。
字段     允许值     允许的特殊字符 
秒         0-59 ,     - * / 
分         0-59 ,     - * / 
小时       0-23 ,     - * / 
日期       1-31 ,     - * ? / L W C 
月份       1-12 或者 JAN-DEC ,     - * / 
星期       1-7 或者 SUN-SAT ,      - * ? / L C # 
年（可选） 留空 或者 1970-2099 ,    - * /


表达式意义 
    "0 0 12 * * ?" 每天中午12点触发 
    "0 15 10 ? * *" 每天上午10:15触发 
    "0 15 10 * * ?" 每天上午10:15触发 
    "0 15 10 * * ? *" 每天上午10:15触发 
    "0 15 10 * * ? 2005" 2005年的每天上午10:15触发 
    "0 * 14 * * ?" 在每天下午2点到下午2:59期间的每1分钟触发 
    "0 0/5 14 * * ?" 在每天下午2点到下午2:55期间的每5分钟触发 
    "0 0/5 14,18 * * ?" 在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发 
    "0 0-5 14 * * ?" 在每天下午2点到下午2:05期间的每1分钟触发 
    "0 10,44 14 ? 3 WED" 每年三月的星期三的下午2:10和2:44触发 
    "0 15 10 ? * MON-FRI" 周一至周五的上午10:15触发 
    "0 15 10 15 * ?" 每月15日上午10:15触发 
    "0 15 10 L * ?" 每月最后一日的上午10:15触发 
    "0 15 10 ? * 6L" 每月的最后一个星期五上午10:15触发 
    "0 15 10 ? * 6L 2002-2005" 2002年至2005年的每月的最后一个星期五上午10:15触发 
    "0 15 10 ? * 6#3" 每月的第三个星期五上午10:15触发 


每天早上6点   0 6 * * *
每两个小时    0 */2 * * * 
晚上11点到早上8点之间每两个小时，早上八点     0 23-7/2，8 * * *
每个月的4号和每个礼拜的礼拜一到礼拜三的早上11点    0 11 4 * 1-3 
1月1日早上4点    0 4 1 1 *


2）Cron表达式范例：


                 每隔5秒执行一次：*/5 * * * * ?


                 每隔1分钟执行一次：0 */1 * * * ?


                 每天23点执行一次：0 0 23 * * ?  


                         每天11点07分的时候程序执行一次: 0 07 11 * * ? 


                 每天凌晨1点执行一次：0 0 1 * * ?


                 每月1号凌晨1点执行一次：0 0 1 1 * ?


                 每月最后一天23点执行一次：0 0 23 L * ?


                 每周星期天凌晨1点实行一次：0 0 1 ? * L


                 在26分、29分、33分执行一次：0 26,29,33 * * * ?


                 每天的0点、13点、18点、21点都执行一次：0 0 0,13,18,21 * * ?
```






##Spring WebApplicationContext的两种初始化方式
```
Spring提供了两种方式用于初始化WebApplicationContext，ServletContext监听器、自启动Servlet。其中只有Servlet2.3以上版本的Web容器才支持ServletContext监听器方式初始化WebApplicationContext。
一、监听器方式（org.springframework.web.context.ContextLoaderListener）
1.ContextLoaderListener通过ServletContext上下文参数contextConfigLocation获取Spring配置文件位置，在web.xml文件中Spring配置文件的位置，如下
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/applicationContext.xml,/WEB-INF/beanConfig.xml</param-value>
</context-param>
在这里可以指定多个文件，多个文件之间用逗号或空格分割，也可以指定带资源类型前缀的路径配置，如
   "calsspath:/applicationContext.xml"、"classpath*:applicationContext*.xml"，
对于未带资源类型前缀的路径配置默认其路径相对于Web部署的根路径。
classpath与classpath*的区别：
假设有多个jar包或类路径下有一个相同的包名如（com.test）classpath只会在第一个加载的com.test包下查找，而classpath*会在所有jar包和类路径下查找。
2.在web.xml文件中配置监听器ContextLoaderListener，如下
<listener>
 <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
<listener>
 
二、自启动Servlet方式（org.springframework.web.context.ContextLoaderServlet）
1.ContextLoaderServlet同样通过ServletContext上下文参数contextConfigLocation获取Spring配置文件的位置，配置跟一样。
2.配置自启动ContextLoaderServlet，如下
<servlet>
    <servlet-name>springContextLoaderServlet</servlet-name>
    <servlet-class>org.springframework.web.context.ContextLoaderServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
在web.xml文件中配置监听器ContextLoaderListener
有2种方式：
--方式1：
<listener>
 <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
<listener>
--方式2：
<servlet>
  <servlet-name>SpringContextServlet></servlet-name>
  <servlet-class>org.springframework.web.context.ContextLoaderServlet></servlet-class>
  <load-on-startup>1</load-on-startup>
</servlet>
Spring的Timer定时器
http://sunny.blog.51cto.com/182601/32366/
//TimerTask1.java
import java.util.TimerTask;
public class TimerTask1 extends TimerTask {
public void run() {
System.out.println("我的第一个任务程序");
}
}
//timer-config.xml
<beans> 
<!--定义定时任务类--> 
<bean id="task1" class="com.adanac.spring.timer.TimerTask1"/> 
<bean id="scheduledTimerTask1" class="org.springframework.scheduling.timer.ScheduledTimerTask"> 
    <!--这里定义定时任务的对象的位置--> 
    <property name="timerTask"> 
     <ref bean="task1"/> 
    </property> 
    <!--这里定义每2秒钟程序执行一次--> 
    <property name="period"> 
     <value>2000</value> 
    </property> 
    <!--这里定义程序启动两秒钟后开始执行-->
    <property name="delay"> 
     <value>4000</value> 
    </property> 
</bean> 
<bean id="timerFactoryBean" class="org.springframework.scheduling.timer.TimerFactoryBean"> 
    <property name="scheduledTimerTasks"> 
     <list> 
        <ref bean="scheduledTimerTask1"/> 
     </list> 
    </property> 
</bean> 
</beans> 
//beans.xml
<beans>
  <!-- 关于定时任务 timer-->
  <import resource="timer-config.xml"></import>
</beans>
```


##System.load 和 System.loadLibrary详解
```
1.它们都可以用来装载库文件，不论是JNI库文件还是非JNI库文件。在任何本地方法被调用之前必须先用这个两个方法之一把相应的JNI库文件装载。
2.System.load 参数为库文件的绝对路径，可以是任意路径。
例如你可以这样载入一个windows平台下JNI库文件：
System.load("C://Documents and Settings//TestJNI.dll");。
3.System.loadLibrary 参数为库文件名，不包含库文件的扩展名。
例如你可以这样载入一个windows平台下JNI库文件
System. loadLibrary ("TestJNI");
这里，TestJNI.dll 必须是在java.library.path这一jvm变量所指向的路径中。
可以通过如下方法来获得该变量的值：
System.getProperty("java.library.path");
默认情况下，在Windows平台下，该值包含如下位置：
1）和jre相关的一些目录
2）程序当前目录
3）Windows目录
4）系统目录（system32）
5）系统环境变量path指定目录
4.如果你要载入的库文件静态链接到其它动态链接库，例如TestJNI.dll 静态链接到dependency.dll, 那么你必须注意：
1）如果你选择
System.load("C://Documents and Settings// TestJNI.dll");
那么即使你把dependency.dll同样放在C://Documents and Settings//下，load还是会因为找不到依赖的dll而失败。因为jvm在载入TestJNI.dll会先去载入TestJNI.dll所依赖的库文件dependency.dll，而dependency.dll并不位于java.library.path所指定的目录下，所以jvm找不到dependency.dll。
你有两个方法解决这个问题：一是把C://Documents and Settings//加入到java.library.path的路径中，例如加入到系统的path中。二是先调用
System.load("C://Documents and Settings// dependency.dll"); 让jvm先载入dependency.dll，然后再调用System.load("C://Documents and Settings// TestJNI.dll");
2）如果你选择
System. loadLibrary ("TestJNI");
那么你只要把dependency.dll放在任何java.library.path包含的路径中即可，当然也包括和TestJNI.dll相同的目录。
如果提示no JNI-UAPI in java.library.path:
可以将JNI-UAPI.dll文件放置到 \jdk\jre\bin 目录下，或者放到 \jdk\bin目录下。
public static void main(String[] args) {
String property = System.getProperty("java.library.path");
System.out.println(property);
}
D:\java\jdk1.7.0_79\bin;C:\Windows\Sun\Java\bin;C:\Windows\system32;C:\Windows;D:/java/jdk1.7.0_79/bin/../jre/bin/client;D:/java/jdk1.7.0_79/bin/../jre/bin;D:/java/jdk1.7.0_79/bin/../jre/lib/i386;D:\java\apache-maven-3.3.3\bin;E:\oracle\product\10.2.0\db_1\bin;D:\java\jdk1.7.0_79\bin;C:\Program Files\AuthenTec TrueSuite\;C:\Program Files\Intel\iCLS Client\;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Program Files\Intel\WiFi\bin\;C:\Program Files\Common Files\Intel\WirelessCommon\;C:\Program Files\Intel\OpenCL SDK\2.0\bin\x86;C:\Program Files\Intel\Intel(R) Management Engine Components\DAL;C:\Program Files\Intel\Intel(R) Management Engine Components\IPT;D:\mysql-5.0\bin;C:\Program Files\Intel\WiFi\bin\;C:\Program Files\Common Files\Intel\WirelessCommon\;D:\java\eclipse-jee-kepler-R-win32\eclipse;;.
```




##path和classpath的区别
```
path 路径，是java编译时需要调用的程序（如java，javac等）所在的地方 。
classpath 类的路径，在编译运行java程序时，如果有调用到其他类的时候，在classpath中寻找需要的类。
```


##利用ScheduledThreadPoolExecutor定时执行任务
```
ScheduledThreadPoolExecutor是ThreadPoolExecutor的子类；
ThreadPoolExecutor，它可另行安排在给定的延迟后运行命令，或者定期执行命令。需要多个辅助线程时，或者要求 ThreadPoolExecutor 具有额外的灵活性或功能时，此类要优于 Timer。
平时我们在执行一个定时任务时，会采用Time,和TimeTask来组合处理；
但是Timer和TimerTask存在一些缺陷：
1：Timer只创建了一个线程。当你的任务执行的时间超过设置的延时时间将会产生一些问题。
2：Timer创建的线程没有处理异常，因此一旦抛出非受检异常，该线程会立即终止。
JDK 5.0以后推荐使用ScheduledThreadPoolExecutor。该类属于Executor Framework，它除了能处理异常外，还可以创建多个线程解决上面的问题。
package org.allen.utils.io.thread;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.ScheduledThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
public class ScheduledThreadPoolExecutorDemo {
private static ScheduledThreadPoolExecutor executor = null;
private static int index;
public static void main(String[] args) {
// 构造一个ScheduledThreadPoolExecutor对象，并且设置它的容量为5个
executor = new ScheduledThreadPoolExecutor(5);
MyTask task = new MyTask();
// 隔2秒后开始执行任务，并且在上一次任务开始后隔一秒再执行一次；
executor.scheduleWithFixedDelay(task, 2, 1, TimeUnit.SECONDS);
// 隔6秒后执行一次，但只会执行一次。
executor.schedule(task, 3, TimeUnit.SECONDS);
}
private static String getTimes() {
SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss E");
Date date = new Date();
date.setTime(System.currentTimeMillis());
return format.format(date);
}
private static class MyTask implements Runnable {
public void run() {
index++;
System.out.println("2= " + getTimes() + " " + index);
if (index >= 10) {
executor.shutdown();
if (executor.isShutdown()) {
System.out.println("停止了？？？？");
}
}
}
}
}
```


##使用 java.net.URLConnection 处理 HTTP 请求
```
你需要自行处理像 NullPointerException、IOExceptions 、RuntimeExceptions、ArrayIndexOutOfBoundsException 这些异常。


准备开始：

首先，我们至少需要了解URL和字符集 charset，至于参数是可选的，主要取决于具体功能
String url = "http://example.com";
String charset = "UTF-8";
String param1 = "value1";
String param2 = "value2";
// ...
String query = String.format("param1=%s&param2=%s", 
     URLEncoder.encode(param1, charset), 
     URLEncoder.encode(param2, charset));


query参数必须是 name=value 格式的，且通过 & 符号连接起来。通常还需要将 query 参数用指定的字符集 charset 进行编码，这里使用 URLEncoder#encode() 。

代码中的 String#format() 只是为了方便而已，当字符串连接符 “+” 出现两次以上时我比较喜欢使用这种方式，个人喜好而已。

发起一个HTTP GET请求，可以携带查询参数（可选）：

这个任务比较轻松，使用默认的方法就可以了
URLConnection connection = new URL(url + "?" + query).openConnection();
connection.setRequestProperty("Accept-Charset", charset);
InputStream response = connection.getInputStream();
// ...

使用 “？” 符号将query 字符串连接到URL上。 Accept-Charset 头信息可能暗示服务器其中的参数是什么编码。如果没有任何query字符串需要发送，那么可以直接忽略 Accept-Charset 头信息。如果不需要设置任何头信息，那么你完全可以使用 URL＃的OpenStream（）快捷方法。
InputStream response = new URL(url).openStream();
// ...

无论哪种方式，如果另一端是一个 HttpServlet ，那么它的 doGet（）方法将被调用，其中的参数可以通过 HttpServletRequest＃getParameter（）获取。

发起带query 参数 的HTTP POST 请求：

设置 URLConnection#setDoOutput() 为 true ，隐性设置请求方法为 POST。 标准的 HTTP POST 是一种 application/x-www-form-urlencoded 类型的网络表单，传递的参数都会被写入请求信息主体中。
URLConnection connection = new URL(url).openConnection();
connection.setDoOutput(true); // Triggers POST.
connection.setRequestProperty("Accept-Charset", charset);
connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded;charset=" + charset);
OutputStream output = null;
try {
     output = connection.getOutputStream();
     output.write(query.getBytes(charset));
} finally {
     if (output != null) try { output.close(); } catch (IOException logOrIgnore) {}
}
InputStream response = connection.getInputStream();
// ...

注意：无论何时，如果希望以编程方式提交 HTML 格式的内容，别忘了获取query 参数中 元素下的 name=value 值对。 当然还有 下的name=value 值对。（因为这些参数通常会在服务器端用来判断按钮是否被按下，哪一个被按下等等）

你也可以将链接方式从 URLConnection 更改成 HttpURLConnection ，然后调用其中的 HttpURLConnection＃setRequestMethod（）方法。但是，如果你想使用连接作为输出，那么你仍然需要设置 URLConnection的＃setDoOutput（）为true。
HttpURLConnection httpConnection = (HttpURLConnection) new URL(url).openConnection();
httpConnection.setRequestMethod("POST");
// ...

无论哪种方式，如果另一端是一个 HttpServlet ，那么 doPost（）方法将会被调用，传递的参数可以通过HttpServletRequest#getParameter() 获取。

发起 HTTP 请求

您可以通过 URLConnection#connect() 显式的发起HTTP请求，但是如果你希望获取 HTTP 响应需求信息，那么HTTP请求就会自动发起，例如 HTTP响应的响应体使用 URLConnection#getInputStream() 等方法时。上面的例子都是如此，所以connect（）调用其实是多余的。

采集HTTP响应信息


HTTP 响应状态 
这里你需要 HttpURLConnection 。如果有必要可以第一个调用。
int status = httpConnection.getResponseCode();



HTTP 响应头信息
for (Entry<String, List<String>> header : connection.getHeaderFields().entrySet()) {
    System.out.println(header.getKey() + "=" + header.getValue());
}



HTTP 响应编码 
当 Content-type 中包含字符集参数时，响应主体可能是基于文本格式的 ，那么我们希望使用服务端指定的字符集处理消息主体。
String contentType = connection.getHeaderField("Content-Type");
String charset = null;
for (String param : contentType.replace(" ", "").split(";")) {
    if (param.startsWith("charset=")) {
        charset = param.split("=", 2)[1];
        break;
    }
}

if (charset != null) {
    BufferedReader reader = null;
    try {
        reader = new BufferedReader(new InputStreamReader(response, charset));
        for (String line; (line = reader.readLine()) != null;) {
            // ... System.out.println(line) ?
        }
    } finally {
        if (reader != null) try { reader.close(); } catch (IOException logOrIgnore) {}
    }
} else {
    // It's likely binary content, use InputStream/OutputStream.
}


会话维持

服务器端的会话通常是由 cookie 支持的。有些网页形式要求您登录 或者 可以跟踪会话。基本上你需要从所有登录或者第一次GET请求的响应中获取 Set-Cookie 头信息 ，然后传递给后续的请求。
// Gather all cookies on the first request.
URLConnection connection = new URL(url).openConnection();
List<String> cookies = connection.getHeaderFields().get("Set-Cookie");
// ...

// Then use the same cookies on all subsequent requests.
connection = new URL(url).openConnection();
for (String cookie : cookies) {
    connection.addRequestProperty("Cookie", cookie.split(";", 2)[0]);
}
// ...

split（“;”，2）[0] 用来去除与服务器端无关的cookie 属性，如 expires, path 等，另外，你也可以使用cookie.substring（0，cookie.indexOf（';'））代替 split（）。

内建的另一种方法是使用CookieHandler API。在发送所有的 HTTP 请求之前，您需要准备一个 CookieManager ，其包含 ACCEPT_ALL 的CookiePolicy 。
// First set the default cookie manager.
CookieHandler.setDefault(new CookieManager(null, CookiePolicy.ACCEPT_ALL));

// All the following subsequent URLConnections will use the same cookie manager.
URLConnection connection = new URL(url).openConnection();
// ...

connection = new URL(url).openConnection();
// ...

connection = new URL(url).openConnection();
// ...

请注意，众所周知的，以上的方法并不是所有情况下都有效。如果您尝试失败，那么最好是手动采集和设置cookie头信息，就像我们在第一个代码片段中所看到的那样。

流模式

默认情况下，在请求发送之前 ，无论您是否已经通过 connection.setRequestProperty(“Content-Length”, contentLength); 设置了内容的固定长度，HttpURLConnection 都会缓冲整个请求主体。因此当你同时发送大数据量 POST请求的时候（例如上传文件），就可能会导致 OutOfMemoryExceptions 异常。为了避免这种情况，你应该设置 HttpURLConnection setFixedLengthStreamingMode（）。
httpConnection.setFixedLengthStreamingMode(contentLength);


但是，如果内容长度是事先不知道的，那么你可以利用分块流模式，通过设置 HttpURLConnection setChunkedStreamingMode（），更改 HTTP Transfer-Encoding 头信息，强制将请求主体以区块的形式发送出去。下面的例子按照每个区块1KB发送。
httpConnection.setChunkedStreamingMode(1024);


User-Agent

它的工作原理与真正的网页浏览器一样，当请求返回了意外的响应时才会产生。基于 User-Agent 请求头信息被服务器端阻塞的机会是很大的。 URLConnection 在默认情况下，将其设置为 Java/1.6.0_19 ，最后一部分很明显是 JRE的版本号。 您可以重写此方法，代码如下：
connection.setRequestProperty("User-Agent", "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.9.2.3) Gecko/20100401"); // Do as if you're using Firefox 3.6.3.


错误处理

如果HTTP 响应代码为4nn（客户端错误）或者 5nn（服务器错误），那么你可能想读取HttpURLConnection＃getErrorStream（） ，看看服务器是否发送了什么有用的错误信息。
InputStream error = ((HttpURLConnection) connection).getErrorStream();


如果 HTTP 响应代码为-1，那么肯定是连接和响应处理出现了错误。HttpURLConnection 的连接状态保持存在bug。您可以通过设置 http.keepAlive 的系统属性为false 来关闭连接。可以在应用程序开始时做以下操作：
System.setProperty("http.keepAlive", "false");


上传文件

上传文件的POST 通常使用 multipart / form-data 编码（主要是二进制和字符数据）。更详细地介绍都在 RFC2388 中。
    String param = "value";
File textFile = new File("/path/to/file.txt");
File binaryFile = new File("/path/to/file.bin");
String boundary = Long.toHexString(System.currentTimeMillis()); // Just generate some unique random value.String CRLF = "\r\n"; // Line separator required by multipart/form-data.

URLConnection connection = new URL(url).openConnection();
connection.setDoOutput(true);
connection.setRequestProperty("Content-Type", "multipart/form-data; boundary=" + boundary);
PrintWriter writer = null;
try {
    OutputStream output = connection.getOutputStream();
    writer = new PrintWriter(new OutputStreamWriter(output, charset), true); // true = autoFlush, important!// Send normal param.
    writer.append("--" + boundary).append(CRLF);
    writer.append("Content-Disposition: form-data; name=\"param\"").append(CRLF);
    writer.append("Content-Type: text/plain; charset=" + charset).append(CRLF);
    writer.append(CRLF);
    writer.append(param).append(CRLF).flush();

    // Send text file.
    writer.append("--" + boundary).append(CRLF);
    writer.append("Content-Disposition: form-data; name=\"textFile\"; filename=\"" + textFile.getName() + "\"").append(CRLF);
    writer.append("Content-Type: text/plain; charset=" + charset).append(CRLF);
    writer.append(CRLF).flush();
    BufferedReader reader = null;
    try {
        reader = new BufferedReader(new InputStreamReader(new FileInputStream(textFile), charset));
        for (String line; (line = reader.readLine()) != null;) {
            writer.append(line).append(CRLF);
        }
    } finally {
        if (reader != null) try { reader.close(); } catch (IOException logOrIgnore) {}
    }
    writer.flush();

    // Send binary file.
    writer.append("--" + boundary).append(CRLF);
    writer.append("Content-Disposition: form-data; name=\"binaryFile\"; filename=\"" + binaryFile.getName() + "\"").append(CRLF);
    writer.append("Content-Type: " + URLConnection.guessContentTypeFromName(binaryFile.getName()).append(CRLF);
    writer.append("Content-Transfer-Encoding: binary").append(CRLF);
    writer.append(CRLF).flush();
    InputStream input = null;
    try {
        input = new FileInputStream(binaryFile);
        byte[] buffer = new byte[1024];
        for (int length = 0; (length = input.read(buffer)) > 0;) {
            output.write(buffer, 0, length);
        }
        output.flush(); // Important! Output cannot be closed. Close of writer will close output as well.
    } finally {
        if (input != null) try { input.close(); } catch (IOException logOrIgnore) {}
    }
    writer.append(CRLF).flush(); // CRLF is important! It indicates end of binary boundary.// End of multipart/form-data.
    writer.append("--" + boundary + "--").append(CRLF);
} finally {
    if (writer != null) writer.close();
}


如果另一端是 HttpServlet ，那么 doPost（）方法将被调用，parts 可通过 HttpServletRequest＃getPart（）获取（请注意，不是 getParameter（）或其他方法）。 getPart（）是一个相对较新的方法，在 Servlet 3.0 中有所介绍（GlassFish 3，Tomcat 7 等等）。在Servlet3.0 以前，你最好的选择就是使用Apache Commons FileUpload 解析 multipart / form-data 请求。

Apache HttpComponents HttpClient 
这个HTTP库更方便

解析与提取 HTML

如果你想对HTML进行解析和提取其中的内容，最好使用HTML解析器，比如 Jsoup （Jsoup.org）

```






ResourceBundle详解

```

ResourceBundle是 Java 开发中非常实用的一个类，主要用来处理应用程序多语言这样的国际化问题。

如果你的应用程序如果有国际化的需求，可以考虑使用ResourceBundle, 你要做的就是给出满足特定格式的Properties 文件，例如

resource.propreties

resource_zh_CN.properties

resource_ja_JP.properties.

然后应用程序使用ResourceBundle.getBundle(“resource”, locale) 就可以自动的搜索的相应Locale的Properties 文件。

虽然看起来很方便，但使用起来需要注意两个问题： 1 Properties 文件的搜索次序， 2.  决定是否能找到Properties 文件的ClassLoader ， 这也是很多初学者遇到的问题

1.  搜索次序。

先来看个例子，假设你的系统只有两个Properties

(1) resource.zh_CN.properties : 中文的Properties


(2) resource.properties  : 英文的Properties

假设你Java 的default locale是zh_CN,  如果你调用 ResourceBundle.getBundle(“resource”, Locale.US) , 你觉得系统会使用哪一个文件中的内容？

很多人会觉得会使用resource.properties 中的内容， 但实际上不是这样的，当你传入一个Locale .US  给ResourceBundle的时候 ,  ResourceBundle的搜索次序是这样：
(1) resource_ en_US .properties     --- 没找到
(2) resource_ en .properties           --- 还是没找到
(3) resource_ zh_CN .properties  ---- default locale， 找到了
(4) resource_ zh _properties       
(5) resource.properties               


注意，ResourceBundle会自动的加上一个default locale 即 zh_CN 来搜索

系统没有找到xxxx__en_US.properties,  也没有找到xxx_en.properties, 而是找到了xxx_zh_CN.properties, 就会使用其中的内容, 所以你看的的是 中文 的结果。

实际上ResourceBundle 搜索结束以后，会建立一个ResourceBundle 对象的Chain, 对于上面的例子会是这样：

ResourceBundle_2  [locale= zh_CN  ,  parent  = ResourceBundle_1]

ResourceBundle_1 [locale =  empty parent  = null]

你可能要问，这个链表中问什么没有en_US,en 和zh相关的信息？   这是因为他们相关的Properties 不存在， 没有必要加入这个链表中。

如果你的应用程序访问resource文件的一个值得时候， 系统会先在ResourceBundle_2[Locale=zh_CN] 这个对象中找， 如果找到，直接返回相应的值

如果没有找到，顺着parent 即ResourceBundle_1继续寻找， 如果还没有找到，只好返回null 了， 因为没有parent 了

2. ClassLoader


这个也是经常出问题的地方， 很多时候当你准备好各种Locale 的Properties 文件， 调用ResourceBundle.getBundle(“resource”, Locale.US) 时，系统总是告诉你， 找不到resource_en_US的文件， 很是令人抓狂。

主要的原因就是ClassLoader 不对 ，有空接着写 :-)


```



