##`<permission>、<uses-permission>、<permission-tree />、<permission-group />`区别



最常用的当属<
uses
-
permission
>，当我们需要获取某个权限的时候就必须在我们的
manifest
文件中声明，此<
uses
-
permission
>与<
application
>同级，具体权限列表请看此处

通常情况下我们不需要为自己的应用程序声明某个权限，除非你提供了供其他应用程序调用的代码或者数据。这个时候你才需要使用<
permission
>
 
这个标签。很显然这个标签可以让我们声明自己的权限。比如：`
<
permission android
:
name
=
"com.teleca.project.MY_SECURITY"
 
.
 
.
 
.
 
/>`

那么在
activity
中就可以声明该自定义权限了，如：

```

<
application 
.
 
.
 
.>

<
activity android
:
name
=
"XXX"
 
.
 
.
 
.
 
>

android
:
permission
=
"com.teleca.project.MY_SECURITY"
>
 
</
activity
>

</
application
>

```

当然自己声明的
permission
也不能随意的使用，还是需要使用<
uses
-
permission
>来声明你需要该权限

<
permission
-
group
>
 
就是声明一个标签，该标签代表了一组
permissions
，而<
permission
-
tree
>是为一组
permissions
声明了一个
namespace
。
##APK在AndroidManifest.xml常用权限
android . permission . ACCESS_CHECKIN_PROPERTIES


//允许读写访问”properties”表在checkin数据库中，改值可以修改上传

android
.
permission
.
ACCESS_COARSE_LOCATION 

//允许一个程序访问CellID或WiFi热点来获取粗略的位置

android
.
permission
.
ACCESS_FINE_LOCATION 

//允许一个程序访问精良位置(如GPS)

android
.
permission
.
ACCESS_LOCATION_EXTRA_COMMANDS 

//允许应用程序访问额外的位置提供命令 

android
.
permission
.
ACCESS_MOCK_LOCATION 

//允许程序创建模拟位置提供用于测试 

android
.
permission
.
ACCESS_NETWORK_STATE 

//允许程序访问有关GSM网络信息

android
.
permission
.
ACCESS_SURFACE_FLINGER 

//允许程序使用SurfaceFlinger底层特性

android
.
permission
.
ACCESS_WIFI_STATE 

//允许程序访问Wi-Fi网络状态信息

android
.
permission
.
ADD_SYSTEM_SERVICE 

//允许程序发布系统级服务 

android
.
permission
.
BATTERY_STATS 

//允许程序更新手机电池统计信息 

android
.
permission
.
BLUETOOTH 

//允许程序连接到已配对的蓝牙设备 

android
.
permission
.
BLUETOOTH_ADMIN 

//允许程序发现和配对蓝牙设备 

android
.
permission
.
BRICK 

//请求能够禁用设备(非常危险

android
.
permission
.
BROADCAST_PACKAGE_REMOVED 

//允许程序广播一个提示消息在一个应用程序包已经移除后

android
.
permission
.
BROADCAST_STICKY 

//允许一个程序广播常用intents 

android
.
permission
.
CALL_PHONE 

//允许一个程序初始化一个电话拨号不需通过拨号用户界面需要用户确认

android
.
permission
.
CALL_PRIVILEGED 

//允许一个程序拨打任何号码，包含紧急号码无需通过拨号用户界面需要用户确认

android
.
permission
.
CAMERA 

//请求访问使用照相设备 

android
.
permission
.
CHANGE_COMPONENT_ENABLED_STATE 

//允许一个程序是否改变一个组件或其他的启用或禁用

android
.
permission
.
CHANGE_CONFIGURATION 

//允许一个程序修改当前设置，如本地化 



android
.
permission
.
CHANGE_NETWORK_STATE

//允许程序改变网络连接状态 

android
.
permission
.
CHANGE_WIFI_STATE 

//允许程序改变Wi-Fi连接状态

android
.
permission
.
CLEAR_APP_CACHE 

//允许一个程序清楚缓存从所有安装的程序在设备中

android
.
permission
.
CLEAR_APP_USER_DATA 

//允许一个程序清除用户设置 

android
.
permission
.
CONTROL_LOCATION_UPDATES 

//允许启用禁止位置更新提示从无线模块 

android
.
permission
.
DELETE_CACHE_FILES 

//允许程序删除缓存文件 

android
.
permission
.
DELETE_PACKAGES 

//允许一个程序删除包 

android
.
permission
.
DEVICE_POWER 

//允许访问底层电源管理 

android
.
permission
.
DIAGNOSTIC 

//允许程序RW诊断资源

android
.
permission
.
DISABLE_KEYGUARD 

//允许程序禁用键盘锁 

android
.
permission
.
DUMP 

//允许程序返回状态抓取信息从系统服务 

android
.
permission
.
EXPAND_STATUS_BAR 

//允许一个程序扩展收缩在状态栏,android开发网提示应该是一个类似Windows Mobile中的托盘程序

android
.
permission
.
FACTORY_TEST 

//作为一个工厂测试程序，运行在root用户

android
.
permission
.
FLASHLIGHT 

//访问闪光灯,android开发网提示HTC Dream不包含闪光灯

android
.
permission
.
FORCE_BACK 

//允许程序强行一个后退操作是否在顶层activities

android
.
permission
.
FOTA_UPDATE 

//暂时不了解这是做什么使用的，android开发网分析可能是一个预留权限.

android
.
permission
.
GET_ACCOUNTS 

//访问一个帐户列表在Accounts Service中

android
.
permission
.
GET_PACKAGE_SIZE 

//允许一个程序获取任何package占用空间容量

android
.
permission
.
GET_TASKS 

//允许一个程序获取信息有关当前或最近运行的任务，一个缩略的任务状态，是否活动等等

android
.
permission
.
HARDWARE_TEST 

//允许访问硬件 

android
.
permission
.
INJECT_EVENTS 

//允许一个程序截获用户事件如按键、触摸、轨迹球等等到一个时间流，android开发网提醒算是hook技术吧

android
.
permission
.
INSTALL_PACKAGES 

//允许一个程序安装packages 

android
.
permission
.
INTERNAL_SYSTEM_WINDOW 

//允许打开窗口使用系统用户界面 

android
.
permission
.
INTERNET 

//允许程序打开网络套接字 

android
.
permission
.
MANAGE_APP_TOKENS 

//允许程序管理(创建、催后、 z- order默认向z轴推移)程序引用在窗口管理器中

android
.
permission
.
MASTER_CLEAR 

//目前还没有明确的解释，android开发网分析可能是清除一切数据，类似硬格机

android
.
permission
.
MODIFY_AUDIO_SETTINGS 

//允许程序修改全局音频设置 

android
.
permission
.
MODIFY_PHONE_STATE 

//允许修改话机状态，如电源，人机接口等 

android
.
permission
.
MOUNT_UNMOUNT_FILESYSTEMS 

//允许挂载和反挂载文件系统可移动存储 

android
.
permission
.
PERSISTENT_ACTIVITY 

//允许一个程序设置他的activities显示

android
.
permission
.
PROCESS_OUTGOING_CALLS 

//允许程序监视、修改有关播出电话 

android
.
permission
.
READ_CALENDAR 

//允许程序读取用户日历数据 

android
.
permission
.
READ_CONTACTS 

//允许程序读取用户联系人数据 

android
.
permission
.
READ_FRAME_BUFFER 

//允许程序屏幕波或和更多常规的访问帧缓冲数据 

android
.
permission
.
READ_INPUT_STATE 

//允许程序返回当前按键状态 

android
.
permission
.
READ_LOGS 

//允许程序读取底层系统日志文件 

android
.
permission
.
READ_OWNER_DATA 

//允许程序读取所有者数据 

android
.
permission
.
READ_SMS 

//允许程序读取短信息 

android
.
permission
.
READ_SYNC_SETTINGS 

//允许程序读取同步设置 

android
.
permission
.
READ_SYNC_STATS 

//允许程序读取同步状态 

android
.
permission
.
REBOOT 

//请求能够重新启动设备 

android
.
permission
.
RECEIVE_BOOT_COMPLETED 

//允许一个程序接收到 

android
.
permission
.
RECEIVE_MMS 

//允许一个程序监控将收到MMS彩信,记录或处理

android
.
permission
.
RECEIVE_SMS 

//允许程序监控一个将收到短信息，记录或处理 

android
.
permission
.
RECEIVE_WAP_PUSH 

//允许程序监控将收到WAP PUSH信息

android
.
permission
.
RECORD_AUDIO 

//允许程序录制音频 

android
.
permission
.
REORDER_TASKS 

//允许程序改变Z轴排列任务

android
.
permission
.
RESTART_PACKAGES 

//允许程序重新启动其他程序 

android
.
permission
.
SEND_SMS 

//允许程序发送SMS短信

android
.
permission
.
SET_ACTIVITY_WATCHER 

//允许程序监控或控制activities已经启动全局系统中

android
.
permission
.
SET_ALWAYS_FINISH 

//允许程序控制是否活动间接完成在处于后台时 

android
.
permission
.
SET_ANIMATION_SCALE 

//修改全局信息比例 

android
.
permission
.
SET_DEBUG_APP 

//配置一个程序用于调试 

android
.
permission
.
SET_ORIENTATION 

//允许底层访问设置屏幕方向和实际旋转 

android
.
permission
.
SET_PREFERRED_APPLICATIONS 

//允许一个程序修改列表参数PackageManager.addPackageToPreferred()和PackageManager.removePackageFromPreferred()方法

android
.
permission
.
SET_PROCESS_FOREGROUND 

//允许程序当前运行程序强行到前台 

android
.
permission
.
SET_PROCESS_LIMIT 

//允许设置最大的运行进程数量 

android
.
permission
.
SET_TIME_ZONE 

//允许程序设置时间区域 

android
.
permission
.
SET_WALLPAPER 

//允许程序设置壁纸 

android
.
permission
.
SET_WALLPAPER_HINTS 

//允许程序设置壁纸hits 

android
.
permission
.
SIGNAL_PERSISTENT_PROCESSES 

//允许程序请求发送信号到所有显示的进程中 

android
.
permission
.
STATUS_BAR 

//允许程序打开、关闭或禁用状态栏及图标Allows an application to open, close, or disable the status bar and its icons.

android
.
permission
.
SUBSCRIBED_FEEDS_READ 

//允许一个程序访问订阅RSS Feed内容提供

android
.
permission
.
SUBSCRIBED_FEEDS_WRITE 

//系统暂时保留改设置,android开发网认为未来版本会加入该功能。

android
.
permission
.
SYSTEM_ALERT_WINDOW 

//允许一个程序打开窗口使用 TYPE_SYSTEM_ALERT，显示在其他所有程序的顶层(Allows an application to open windows using the type TYPE_SYSTEM_ALERT, shown on top of all other applications. )

android
.
permission
.
VIBRATE 

//允许访问振动设备 

android
.
permission
.
WAKE_LOCK 

//允许使用PowerManager的 WakeLocks保持进程在休眠时从屏幕消失

android
.
permission
.
WRITE_APN_SETTINGS 

//允许程序写入APN设置

android
.
permission
.
WRITE_CALENDAR 

//允许一个程序写入但不读取用户日历数据 

android
.
permission
.
WRITE_CONTACTS 

//允许程序写入但不读取用户联系人数据 

android
.
permission
.
WRITE_GSERVICES 

//允许程序修改Google服务地图

android
.
permission
.
WRITE_OWNER_DATA 

//允许一个程序写入但不读取所有者数据 

android
.
permission
.
WRITE_SETTINGS 

//允许程序读取或写入系统设置 

android
.
permission
.
WRITE_SMS 

//允许程序写短信 

android
.
permission
.
WRITE_SYNC_SETTINGS 

//允许程序写入同步设置
##关于配置文件属性



activity_main
.
xml

<
LinearLayout
 xmlns
:
android
=
"http://schemas.android.com/apk/res/android"

    android
:
layout_width
=
"match_parent"

    android
:
layout_height
=
"match_parent"

    android
:
orientation
=
"vertical"

    android
:
id
=
"@+id/content_area"
>


--
android
:
orientation

Android
布局
LinearLayout
注意设置属性
android
:
orientation
属性，否则有的组件可能无法显示。

该属性不设置时默认为
horizontal
，此时第一个控件的宽度若设置成
"fill_parent"
,后面添加的组件将都无法看到。

因此使用该布局的时候要注意设置
android
:
orientation
=
"vertical"


android
:
layout_gravity 
(
 
是本元素相对于父元素的重力方向
 
)

android
:
gravity 
（是本元素所有子元素的重力方向）

android
:
orientation 
（线性布局以列或行来显示内部子元素）

android
:
layout_weight 
（线性布局内子元素对未占用空间【水平或垂直】分配权重值，其值越小，权重越大。

    
前提是子元素
 
设置了
 android
:
layout_width 
=
 
"fill_parent"
 
属性（水平方向）

                  
或
 android
:
layout_height 
=
 
"fill_parent"
 
属性（垂直方向）


如果某个子元素的
 android
:
layout_width 
=
 
"wrap_content"

           
或
 android
:
layout_height 
=
"
 wrap_content
”
 
，

则
 android
:
layout_weight 
的设置值
 
对该方向上空间的分配刚好相反。
##Layout_weight属性的作用：它是用来分配属于空间的一个属性，你可以设置他的权重

## android应用中去掉标题栏的方法
- 在清单文件（manifest.xml）里面实现
```
<application android:icon= "@drawable/icon"    
     android:label= "@string/app_name"    
        android:theme= "@android:style/Theme.NoTitleBar" >  
```
-  在style.xml文件里定义
```
<?xml version="1.0" encoding="UTF-8" ?>  
<resources> 
    <style name="notitle">  
        <item name="android:windowNoTitle">true</item>  
    </style>   
</resources>
```
然后面manifest.xml中引用就可以了，

```
<application android:icon="@drawable/icon"   
        android:label="@string/app_name"   
        android:theme="@style/notitle"> 
```


##Android 中TextView 添加超链接
1、 使用android:autoLink="all" 只需在TextView中加入这个属性，而在TextView里面写的文字中包含网址、电话、email的会自动加入连接地址。 


```
<TextView xmlns:android="http://schemas.android.com/apk/res/android" 
android:id="@+id/text1" 
android:layout_width="match_parent" 
android:layout_height="match_parent"
android:autoLink="all" 
android:text="@string/link_text_auto"
/> 
```
2、 使用<string name=””><a href=””></a></string>标签，建立超链接：

```
<string name="link_text_manual"><b>text2:</b> This is some other 
text, with a <a href="http://www.google.com">link</a> specified 
via an <a> tag.  Use a \"tel:\" URL 
to <a href="tel:4155551212">dial a phone number</a>. 
</string> 
别忘了 
TextView t2 = (TextView) findViewById(R.id.text2); 
t2.setMovementMethod(LinkMovementMethod.getInstance()); 
```
3、 在java文件中使用HTML语言：
```
TextView t3 = (TextView) findViewById(R.id.text3); 
t3.setText(Html.fromHtml("<b>text3:</b>Text with a " 
+ "<a href=\"http://www.google.com\">link</a> " 
+ "created in the Java source code using HTML.")); 
t3.setMovementMethod(LinkMovementMethod.getInstance()); 
```
4、 字符串截取方法 
```
SpannableString ss = new SpannableString("text4: Click here to dial the phone."); 
ss.setSpan(new StyleSpan(Typeface.BOLD), 0, 6, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE); 
ss.setSpan(
new URLSpan("tel:4155551212"),13, 17, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE); 
TextView t4 = (TextView) findViewById(R.id.text4); 
t4.setText(ss); 
t4.setMovementMethod(LinkMovementMethod.getInstance()); 
```
5、 Android中我们为了实现文本的滚动可以在ScrollView中嵌入一个TextView，其实TextView自己也可以实现多行滚动的，毕竟ScrollView必须只能有一个直接的子类布局。只要在layout中简单设置几个属性就可以轻松实现 
```
  <TextView  
    android:id="@+id/tvCWJ"  
    android:layout_width="fill_parent"  
    android:layout_height="wrap_content"  
    android:scrollbars="vertical"   <!--垂直滚动条 --> 
    android:singleLine="false"       <!--实现多行 --> 
    android:maxLines="15"            <!--最多不超过15行 --> 
    android:textColor="#FF0000" 
  />  
   当然我们为了让TextView动起来，还需要用到TextView的setMovementMethod方法设置一个滚动实例，代码如下 
TextView tvAndroid123 = (TextView)findViewById(R.id.tvCWJ);   
tvAndroid123.setMovementMethod(ScrollingMovementMethod.getInstance()); 
ad_link = (TextView) findViewById(R.id.ad_link);   
ad_link.setText(Html.fromHtml("<a href="\"mce_href="\"""+mURL.getLink()+"\">"+Html.fromHtml(mURL.getLabel()+"</a>")));   
ad_link.setMovementMethod(LinkMovementMethod.getInstance());
```
##android textView 滚动条
```

Xml代码 
<TextView 
    Android:id="@+id/text_view" 
    android:layout_width="fill_parent" 
    android:layout_height="wrap_content" 
    android:singleLine="false" 
    android:maxLines="10" 
    android:scrollbars="vertical" 
    /> 
还需要在代码了设置TextView可以滚动。 
Java代码 

TextView textView = (TextView)findViewById(R.id.text_view);  
textView.setMovementMethod(ScrollingMovementMethod.getInstance());
```