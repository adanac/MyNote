
Solr里面的core就像 数据库 里面的一个表，用来管理索引和相关配置。

 

一、使用示例core

下载的solr完整包里面solr-4.7.0\example\multicore这个文件夹下面有2个示例core；分别是core0和core1；如下图：






随便拷贝个到 $SOLR_HOME$，$SOLR_HOME$在什么地方配置的呢，就是你solr的web服务里面的web.xml里面配置，如下面配置，其中：D:\workspace\lucene\solr_home就是我的$SOLR_HOME$。


[html] view plain copy
print ?
< env-entry >   
      < env-entry-name > solr/home </ env-entry-name >   
      < env-entry-value > D:\workspace\lucene\solr_home </ env-entry-value >   
      < env-entry-type > java.lang.String </ env-entry-type >   
  </ env-entry >   

<env-entry>
     <env-entry-name>solr/home</env-entry-name>
     <env-entry-value>D:\workspace\lucene\solr_home</env-entry-value>
     <env-entry-type>java.lang.String</env-entry-type>
 </env-entry>





这里我们拷贝core0到我们的$SOLR_HOME$。



现在我们看看core0下面都有什么，这时候里面就一个conf文件夹，什么都没有。在core0/conf里面也就2个xml文件，分别是schema.xml、solrconfig.xml；

schema.xml定义了core0的field类型和名称，field就像数据库的字段，field的类型就像数据库的字段类型，field的名称就像数据库的字段名称；

solrconfig.xml描述了core0管理配置，比如指定索引文件的存储位置、日志文件的存储位置、使用什么管理器等。

启动solr服务，进入solr的管理界面，选中core Admin栏，如下图：

我们点击Add Core按钮，在弹出的界面中把name和instanceDir的值改为core0，也就是我们上面拷贝到$SOLR_HOME$的那个core0文件夹的名称，点击那个蓝色的Add Core按钮。





我们成功在solr服务器新加了一个core。



现在我们再回到$SOLR_HOME$/core0,发现下面多了一个data文件夹和一个core.properties文件。这2个东西就是我们在solr的管理页面操作的时候solr自己给我们创建的。我们为什么在管理页面能够看到core0，solr服务如何知道$SOLR_HOME$下面有个core0，其实还是core.properties在发挥作用。其实我们可以通过手动写core.properties来完成新建core。

core.properties
[html] view plain copy
print ?
#Written by CorePropertiesLocator  
#Sat Mar 15 15:49:01 CST 2014  
name = core0   
config = solrconfig .xml  
schema = schema .xml  
dataDir = data   

#Written by CorePropertiesLocator
#Sat Mar 15 15:49:01 CST 2014
name=core0
config=solrconfig.xml
schema=schema.xml
dataDir=data



二、手动新建core

在$SOLR_HOME$新建clj_core文件夹，然后在clj_core文件夹下面再建立一个conf文件夹，我们把示例core0里面的conf下面的2个xml文件拷贝到新建的clj_core/conf文件夹下面；我们在clj_core下面新建一个core.properties文件配置如下：


[html] view plain copy
print ?
name = core1   
config = solrconfig .xml  
schema = schema .xml  
dataDir = data   

name=core1
config=solrconfig.xml
schema=schema.xml
dataDir=data





重启solr服务，我们便在solr的管理页面看到我们新建的core1了。注意一点我们的core的名称和core的文件夹可以不一样，但是最好定义为一样的，方便管理。像上面我们手动建立的core1，其实我们的core文件夹名称是clj_core，这样的设计对维护很不友好，最好把文件夹名称改为core1或者把core的名称改为clj_core.

