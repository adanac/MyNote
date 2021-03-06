 南京招聘会玩或想玩dubbo的程序员，联系我 50625185 地址：江苏软件园29栋西201 1. Dubbo是什么？

Dubbo是一个分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，以及SOA服务治理方案。简单的说， dubbo就是个服务框架，如果没有分布式的需求，其实是不需要用的，只有在分布式的时候，才有dubbo这样的分布式服务框架的需求，并且本质上是个服务调用的东东， 说白了就是个远程服务调用的分布式框架（告别 Web Service模式中的WSdl，以服务者与消费者的方式在dubbo上注册 ）
其核心部分包含:
1. 远程通讯: 提供对多种基于长连接的NIO框架抽象封装，包括多种线程模型，序列化，以及“请求-响应”模式的信息交换方式。
2. 集群容错: 提供基于接口方法的透明远程过程调用，包括多协议支持，以及软负载均衡，失败容错，地址路由，动态配置等集群支持。
3. 自动发现: 基于注册中心目录服务，使服务消费方能动态的查找服务提供方，使地址透明，使服务提供方可以平滑增加或减少机器。 2. Dubbo能做什么？

1.透明化的远程方法调用，就像调用本地方法一样调用远程方法，只需简单配置，没有任何API侵入。      
2.软负载均衡及容错机制，可在内网替代F5等硬件负载均衡器，降低成本，减少单点。
3. 服务自动注册与发现，不再需要写死服务提供方地址，注册中心基于接口名查询服务提供者的IP地址，并且能够平滑添加或删除服务提供者。

Dubbo采用全Spring配置方式，透明化接入应用，对应用没有任何API侵入，只需用Spring加载Dubbo的配置即可，Dubbo基于Spring的Schema扩展进行加载。


之前使用Web Service，我想测试接口可以通过模拟消息的方式通过soapui或LR进行功能测试或性能测试。但现在使用Dubbo，接口之间不能直接交互，我尝试通过模拟消费者地址测试，结果不堪入目，再而使用jmeter通过junit进行测试，但还是需要往dubbo上去注册，如果再不给提供源代码的前提下，这个测试用例不好写啊....

  3. dubbo的架构
dubbo 架构图如下所示：
 



 

  节点角色说明：

       Provider: 暴露服务的服务提供方。

       Consumer: 调用远程服务的服务消费方。

       Registry: 服务注册与发现的注册中心。

       Monitor: 统计服务的调用次调和调用时间的监控中心。

       Container: 服务运行容器。

这点我觉得非常好，角色分明，可以根据每个节点角色的状态来确定该服务是否正常。 调用关系说明：

0 服务容器负责启动，加载，运行服务提供者。 1. 服务提供者在启动时，向注册中心注册自己提供的服务。 2. 服务消费者在启动时，向注册中心订阅自己所需的服务。 3. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。 4. 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。 5. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

dubbo的容错性显而易见，性能方面还没有还得及测，我们系统某页面需要掉5次接口，本来想建议做个缓存，但业务关系不能采纳，还需要研究下dubbo的性能调优问题... 4. dubbo使用方法。

Dubbo采用全Spring配置方式，透明化接入应用，对应用没有任何API侵入，只需用Spring加载Dubbo的配置即可，Dubbo基于Spring的Schema扩展进行加载。 如果不想使用Spring配置，而希望通过API的方式进行调用（不推荐）

下面我们就来看看spring配置方式的写法: 服务提供者：

1. 下载zookeeper注册中心，下载地址： http://www.apache.org/dyn/closer.cgi/zookeeper/   下载后解压即可，进入D:\apach-zookeeper-3.4.5\bin，

双击zkServer.cmd启动注册中心服务。 2. 定义服务接口: (该接口需单独打包，在服务提供方和消费方共享)

 

  下面这个例子不错，写的很详细可以做个model.
[html]

 
package com.unj.dubbotest.provider;  
  
import java.util.List;  
  
public interface DemoService {  
  
    String sayHello(String name);  
  
    public List getUsers();  
  
}  


在服务提供方实现接口：(对服务消费方隐藏实现)

 
[html]  

 
package com.unj.dubbotest.provider;  
  
import java.util.ArrayList;  
import java.util.LinkedList;  
import java.util.List;  
  
  
public class DemoServiceImpl implements DemoService{  
      
     public String sayHello(String name) {  
            return "Hello " + name;  
     }  
     public List getUsers() {  
         List  list  =  new  ArrayList();  
         User  u1  =  new  User();  
         u1.setName("jack");  
         u1.setAge(20);  
         u1.setSex("男");  
           
         User  u2  =  new  User();  
         u2.setName("tom");  
         u2.setAge(21);  
         u2.setSex("女");  
           
         User  u3  =  new  User();  
         u3.setName("rose");  
         u3.setAge(19);  
         u3.setSex("女");  
           
         list.add(u1);  
         list.add(u2);  
         list.add(u3);  
         return list;  
     }  
}  

 
用Spring配置声明暴露服务：
[html]  

 
<? xml   version = "1.0"   encoding = "UTF-8" ?>   
< beans   xmlns = "http://www.springframework.org/schema/beans"   
     xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"   
     xmlns:dubbo = "http://code.alibabatech.com/schema/dubbo"   
     xsi:schemaLocation ="http://www.springframework.org/schema/beans  
        http://www.springframework.org/schema/beans/spring-beans.xsd  
        http://code.alibabatech.com/schema/dubbo  
        http://code.alibabatech.com/schema/dubbo/dubbo.xsd  
        " >   
   
     <!-- 具体的实现bean -->   
     < bean   id = "demoService"   class = "com.unj.dubbotest.provider.DemoServiceImpl"   />   
      
     <!-- 提供方应用信息，用于计算依赖关系 -->   
     < dubbo:application   name = "xixi_provider"    />   
   
    <!-- 使用multicast广播注册中心暴露服务地址   
     < dubbo:registry   address = "multicast://224.5.6.7:1234"   /> -- >   
    
     <!-- 使用zookeeper注册中心暴露服务地址 -->   
     < dubbo:registry   address = "zookeeper://127.0.0.1:2181"   />    
    
     <!-- 用dubbo协议在20880端口暴露服务 -->   
     < dubbo:protocol   name = "dubbo"   port = "20880"   />   
   
     <!-- 声明需要暴露的服务接口 -->   
     < dubbo:service   interface = "com.unj.dubbotest.provider.DemoService"   ref = "demoService"   />   
      
</ beans >   
加载Spring配置，启动服务：
[html]  

 
package com.unj.dubbotest.provider;  
  
import org.springframework.context.support.ClassPathXmlApplicationContext;  
  
public class Provider {  
   
    public static void main(String[] args) throws Exception {  
        ClassPathXmlApplicationContext  context  =  new  ClassPathXmlApplicationContext(new String[] {"applicationContext.xml"});  
        context.start();  
   
        System.in.read(); // 为保证服务一直开着，利用输入流的阻塞来模拟  
    }  
   
}  
服务消费者：

 applicationContext-dubbo.xml 中注册自己需要调用的接口，我刚开始测试的时候需要的接口很多，所以把这个文件写的满满的，后来熟悉了把接口按业务类型分开，写了N多个  applicationContext-dubbo-***.xml 简练多了 》。 

 
1.通过Spring配置引用远程服务：
[html]

 
<? xml   version = "1.0"   encoding = "UTF-8" ?>   
< beans   xmlns = "http://www.springframework.org/schema/beans"   
     xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"   xmlns:dubbo = "http://code.alibabatech.com/schema/dubbo"   
     xsi:schemaLocation ="http://www.springframework.org/schema/beans  
        http://www.springframework.org/schema/beans/spring-beans.xsd  
        http://code.alibabatech.com/schema/dubbo  
        http://code.alibabatech.com/schema/dubbo/dubbo.xsd  
        " >   
  
     <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->   
     < dubbo:application   name = "hehe_consumer"   />   
  
     <!-- 使用zookeeper注册中心暴露服务地址 -->   
     <!-- <dubbo:registry address="multicast://224.5.6.7:1234" /> -->   
     < dubbo:registry   address = "zookeeper://127.0.0.1:2181"   />   
  
     <!-- 生成远程服务代理，可以像使用本地bean一样使用demoService -->   
     < dubbo:reference   id = "demoService"   
         interface = "com.unj.dubbotest.provider.DemoService"   />   
  
</ beans >   

2.加载Spring配置，并调用远程服务：
[html]  

 
package com.alibaba.dubbo.demo.pp;  
  
import java.util.List;  
  
import org.springframework.context.support.ClassPathXmlApplicationContext;  
  
import com.unj.dubbotest.provider.DemoService;  
  
public class Consumer {  
  
    public static void main(String[] args) throws Exception {  
        ClassPathXmlApplicationContext  context  =  new  ClassPathXmlApplicationContext(  
                new String[] { "applicationContext.xml" });  
        context.start();  
  
        DemoService  demoService  = (DemoService) context.getBean("demoService"); //  
        String  hello  =  demoService .sayHello("tom"); // ִ  
        System.out.println(hello); //   
  
        //   
        List  list  =  demoService .getUsers();  
        if (list != null && list.size()  >  0) {  
            for (int  i  =  0 ; i  <   list.size (); i++) {  
                System.out.println(list.get(i));  
            }  
        }  
        // System.out.println(demoService.hehe());  
        System.in.read();  
    }  
  
}  

调用结果为：

 




dubbo管理页面：

这个管理页面还需要部署一个环境的，一开始我还以为是dubbo自带的，找了半天没有找到....

 




 

 

 

 

应用页面：



 

 

 

 

提供者页面:



 

 

 

 

消费者页面：



 

 

 

 

服务页面：



测试是否成功，我觉得只要看看状态是否正常，就ok了 ....

 

案例代码下载： http://download.csdn.net/detail/yiyu1/7116319

 
 

 

 

 

 