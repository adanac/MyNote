





注意：本文中的tomcat8所在目录为 D:\tools\apache-tomcat-8.0.32 目录下 
1、官网下载solr-5.5.0.zip解压到D:\tools\solr-5.5.0目录 
解压之后的solr-5.5.0文件夹包含了几乎所有你需要的东西。

2、复制solr-5.5.0/server/solr-webapp/webapp到tomcat下的webapps目录下，改名为solr。

3、将solr-5.5.0/server/lib/ext/目录下的所有jar包复制到tomcat/webapps/solr/WEB-INF/lib/下。

4、将solr-5.5.0/server/solr目录复制到D:\tools\apache-tomcat-8.0.32\bin目录下，这个就是传说中的solr/home(存放检索数据)。

5、设置 solr/home：编辑D:\tools\apache-tomcat-8.0.32\webapps\solr\WEB-INF\web.xml文件，以下部分是注释的，打开。 solr在启动的时候会去这个根目录下加载配置信息。


    <env-entry>
       <env-entry-name>solr/home</env-entry-name>
       <env-entry-value>D:\\tools\\apache-tomcat-8.0.32\\bin\\solr</env-entry-value>
       <env-entry-type>java.lang.String</env-entry-type>
    </env-entry>
6、将solr-5.5.0/server/resouce下的log4j.properties文件复制到tomcat/weapps/solr/WEB-INF/classes目录下，如果没有则新建。



7、将solr-5.5.0/dist目录下的solr-dataimporthandler-5.5.0.jar和solr-dataimporthandler-extras-5.5.0.jar复制到tomcat/webapps/solr/WEB-INF/lib/下，这个是为了以后导入数据库表数据。

到这里，solr5的环境就搭建好了，重启tomcat服务，访问 http://localhost/solr/index.html 可以看到solr控制台。

下一步就是添加core了 

8、在tomcat/solrhome/目录下创建core1(自定义), 在其目录下创建data文件夹，并将tomcat7/solrhome/configsets/basic_configs/目录下的conf文件夹复制到core1下。然后在solr控制台点击Add Core




name：给core随便起个名字

instanceDir：core的安装目录，这里就是之前在 tomcat/solrhome/目录下创建的core1文件夹

dataDir： 指定用于存放lucene索引和log日志文件的目录路径，该路径是相对于core根目录(在单core模式下，就直接是相对于solr_home了)，默认值是当前core目录下的data。

config：用于指定solrconfig.xml配置文件的文件名，启动时会去 core1/config目录下去查找。

schema： 即用来配置你的schema.xml配置文件的文件名的，schema.xml配置文件应该存放在当前core目录下的conf目录下。但是下载的solr里没有这个文件，所以我也不管了。

属性都填上，然后点击Add Core，就创建完成了。





未完待续...



