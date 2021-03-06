##知识点
```
一般来说，URL重写是支持会话的非常健壮的方法。在不能确定浏览器是否支持Cookie的情况下应该使用这种方法。
然而，使用URL重写应该注意下面几点：
1.如果使用URL重写，应该在应用程序的所有页面中，对所有的URL编码，包括所有的超链接和表单的action属性值。
2.应用程序的所有的页面都应该是动态的。因为不同的用户具有不同的会话ID，因此在静态HTML页面中无法在URL上附加会话ID。
3.所有静态的HTML页面必须通过Servlet运行，在它将页面发送给客户时会重写URL。
```
##URL重写实现会话跟踪
```
URL重写实现会话跟踪


为了防止用户禁用cookie，可以使用URL重写技术来实现会话跟踪！
url重写原理：当服务器程序调用request.getSession();代码时，其会先看request.getCookies()方法中有没有名为JSESSIONID的cookie带过来，如果没有，就看URL有没有被重写(即附带JSESSIONID)，如果有，则从服务器中找key为JSESSIONID的session对象，如果都没有，则创建一个新的session。如果用户禁用了cookie，则只能通过URL重写方式实现会话跟踪！
一、在Servlet中实现URL重写:


   客户端在访问本Servlet后，会返回主页面:
import java.io.IOException;  
import java.io.PrintWriter;  
import javax.servlet.ServletException;  
import javax.servlet.http.HttpServlet;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
  
public class EncodeURL extends HttpServlet {  
  
    public void doGet(HttpServletRequest request, HttpServletResponse response)  
            throws ServletException, IOException {  
        response.setCharacterEncoding("utf-8");  
        response.setContentType("text/html;charset=utf-8");  
        PrintWriter out = response.getWriter();  
        request.getSession();  //创建session  
        //调用response的encodeURL方法，将自动将JSESSION追加到url后面，如：url;jsessionid=BD111FFC653497E81B702A29B3AC6FE4  
        String buyurl = response.encodeURL("/CookieAndSession/servlet/buy");  
        String payurl = response.encodeURL("/CookieAndSession/servlet/pay");  
        out.print("<a href='"+buyurl+"'>购买</a><br/>");  
        out.print("<a href='"+payurl+"'>结账</a><br/>");  
          
    }  
  
}  
http://localhost:8080/CookieAndSession/servlet/encodeurl（此事后cookie被禁用了）




这样即可实现回跟踪。
  注意:
  1. 但是如果用户禁用cookie，则关闭了浏览器后，重新开启浏览器，则回话失效，无法实现回话跟踪；如果是用户没有禁用cookie，则可以通过设置装载JSESSIONID的cookie的失效时间来控制浏览器关闭后session仍未失效。


   2.如果用户没有禁用cookie，而且又使用URL重写，则：用户在第一次访问EncodeURLServlet时，由于不知道用户是否禁用了cookie，所以response.encodeURL()方法内部会将JSESSIONID重写在url上，但是一旦第二次访问时，由于用户是带着cookie来的，所以response.encodeURL()不会将JSESSIONID重写在url上。
```

