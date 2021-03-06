##Cookie实现会话跟踪
```
服务器默认创建一个Cookie回传给用户:（Cookie cookie = new Cookie("JSESSIONID",session.getId());response.addCookie(cookie);）此cookie的默认生命周期为关闭浏览器cookie即销毁，所以当浏览器关闭后，使用cookie实现的回话跟踪应用(如购物)将会失效。

解决浏览器关闭，装载sessionid的cookie 失效的问题: 可以通过创建一个名为"JSESSIONID"的cookie，然后设置cookie的生命周期(cookie.setMaxAge())来设置cookie的失效时间，如下：


import java.io.IOException;  
import java.io.PrintWriter;  
import javax.servlet.ServletException;  
import javax.servlet.http.Cookie;  
import javax.servlet.http.HttpServlet;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import javax.servlet.http.HttpSession;  
  
public class BuyServlet extends HttpServlet {  
  
    public void doGet(HttpServletRequest request, HttpServletResponse response)  
            throws ServletException, IOException {  
         response.setCharacterEncoding("utf-8");  
         response.setContentType("text/html;charset=utf-8");  
         PrintWriter out = response.getWriter();  
         HttpSession session = request.getSession();  
        /** 
         * 设置session的空闲失效时间有两种方式： 
         *  1.在web.xml中设置: 
         *     <session-config> 
                   <session-timeout>2</session-timeout> 
              </session-config>  
            2.设置session的maxInactiveInterval属性，（session的 默认空闲失效时间为30分钟，即30分钟内不 
                              访问服务器，session将自动销毁）如下:session.setMaxInactiveInterval(2*60); 
                            也可以通过 session.invalidate();销毁session 
         **/  
         session.setAttribute("name", "联想笔记本");  
         /** 
          *  session的原理是通过向浏览器发送存放sessionId的cookie来实现session的，而存放sessionId的cookie的默认 
          *  生命为浏览器关闭cookie消亡，所以当用户关闭浏览器后，其购物车商品会消失，为避免这种情况，应设置存放 
          *  sessionId的cookie的生命周期 
          **/  
         Cookie cookie = new Cookie("JSESSIONID", session.getId());  
         cookie.setMaxAge(30*60);//设置放置sessionId的cookie的生命周期为30分钟  
         cookie.setPath("/CookieAndSession");  
         response.addCookie(cookie);  
           
    }  
}  
如果用户禁用了cookie，则只能通过url重写的方式来实现回话跟踪。但是url重写的方式实现回话跟踪的不能解决浏览器关闭，无法获取原来的sessionid的问题，即不能解决浏览器关闭，回话失效的问题。关于使用url重写实现回话跟踪，见:URL重写实现会话跟踪


一般大型的电子商务网站，一般是使用cookie实现回话跟踪的，因为如果使用session来实现回话跟踪的话，对服务器的压力很大，导致服务器性能下降！
```