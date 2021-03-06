##latke简介
```
https://github.com/b3log/latke
Latke （’lɑ:tkə，土豆饼）是一个简单易用的 Java Web 应用开发框架，包含 IoC 容器、事件通知、持久化、插件等组件，也包含了一些应用开发时需要的基本服务（例如缓存、定时任务、邮件、HTTP 客户端等）。

在实体模型上使用 JSON 贯穿前后端，使应用开发更加快捷。这是 Latke 不同于其他框架的地方，非常适合小型应用的快速开发，  为什么又要造一个叫 Latke 的轮子  。
来源：  https://hacpai.com/article/1466870492857
```
###latke Demo
```
https://github.com/b3log/latke-demo

org.b3log.latke.demo.hello.processor.RegisterProcessor
@RequestProcessor
public class RegisterProcessor {

    @Inject
    private UserService userService;

    @RequestProcessing(value = "/register", method = {HTTPRequestMethod.GET, HTTPRequestMethod.POST})
    public void register(final HTTPRequestContext context, final HttpServletRequest request) {
        final AbstractFreeMarkerRenderer render = new FreeMarkerRenderer();
        context.setRenderer(render);

        render.setTemplateName("register.ftl");
        final Map<String, Object> dataModel = render.getDataModel();

        final String name = request.getParameter("name");
        if (!Strings.isEmptyOrNull(name)) {
            dataModel.put("name", name);

            userService.saveUser(name, 3);
        }
    }
}


register.ftl:
<!DOCTYPE html>
<html>
    <body>
        <form action="/register" method="POST">
            Your Name: <input name="name" type="text"/><input type="submit" value="Submit"/>
        </form>
        
        <#if name??>
        Hello, ${name}!
        </#if>
    </body>
</html>
```