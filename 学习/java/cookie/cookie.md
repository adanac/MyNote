##浏览器的cookie保存位置
```
C:\Users\allen\AppData\Roaming\Microsoft\Windows\Cookies

```
##常用属性
```
expires属性
指定了coolie的生存期，默认情况下coolie是暂时存在的，他们存储的值只在浏览器会话期间存在，当用户推出浏览器后这些值也会丢失，如果想让cookie存在一段时间，就要为expires属性设置为未来的一个过期日期。现在已经被max-age属性所取代，max-age用秒来设置cookie的生存期。
path属性
它指定与cookie关联在一起的网页。在默认的情况下cookie会与创建它的网页，该网页处于同一目录下的网页以及与这个网页所在目录下的子目录下的网页关联。
domain属性
domain属性可以使多个web服务器共享cookie。domain属性的默认值是创建cookie的网页所在服务器的主机名。不能将一个cookie的域设置成服务器所在的域之外的域。
例如让位于order.example.com的服务器能够读取catalog.example.com设置的cookie值。如果catalog.example.com的页面创建的cookie把自己的path属性设置为“/”，把domain属性设置成“.example.com”，那么所有位于catalog.example.com的网页和所有位于orlders.example.com的网页，以及位于example.com域的其他服务器上的网页都可以访问这个coolie。
secure属性
它是一个布尔值，指定在网络上如何传输cookie，默认是不安全的，通过一个普通的http连接传输
```