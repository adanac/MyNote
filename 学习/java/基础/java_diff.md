

## 为什么Java中的密码优先使用 char[] 而不是String？

```

（1574个赞）

在Swing中，密码字段有一个getPassword()（返回 char数组）方法而不是通常的getText()（返回String）方法。同样的，我遇到过一个建议，不要使用 String 来处理密码。

为什么String涉及到密码时，它就成了一个安全威胁？感觉使用char数组不太方便。

解决方案

String是不可变的。这意味着一旦创建了字符串，如果另一个进程可以进行内存转储，在GC发生前，（除了反射）没有方法可以清除字符串数据。

使用数组操作完之后，可以显式地清除数据：可以给数组赋任何值，密码也不会存在系统中，甚至垃圾回收之前也是如此。

所以，是的，这是一个安全问题 – 但是即使使用了char数组，仅仅缩小了了攻击者有机会获得密码的窗口，它值针对制定的攻击类型。

```
##HashMap 和 Hashtable的不同是什么？
```


非多线程应用中使用哪个更有效率？

解决方案

Java 中 HashMap 和 HashTable 有几个不同点：
Hashtable  是同步的，然而  HashMap 不是。 这使得HashMap更适合非多线程应用，因为非同步对象通常执行效率优于同步对象。
Hashtable 不允许 null 值和键。HashMap允许有一个 null 键和人一个 NULL 值。
HashMap的一个子类是 LinkedHashMap 。所以，如果想预知迭代顺序（默认的插入顺序），只需将HashMap转换成一个LinkedHashMap。用Hashtable就不会这么简单。

因为同步对你来说不是个问题，我推荐使用HashMap。如果同步成为问题，你可能还要看看 ConcurrentHashMap 。

来源：  http://www.codeceo.com/article/stackoverflow-10-java-problem.html
```
##request的getAttribute与getParameter的区别
```
public Pager<GoodDto> listGoods(HttpServletRequest request, HttpServletResponse response) {
        log.debug("listGoods----------------------------");
        // 获取session中的登陆人信息
        HttpSession session = request.getSession();
        String suppId = session.getAttribute(MmcConstants.SESSION_SUPP_ID).toString();
        String goodName = request.getParameter("goodName");// 商品名称或货号;  

```
##modelMap的put与addAttribute的区别
```
public void findPayDetail(HttpServletRequest request, ModelMap model) {
        try {
            // 参数封装
            String id = request.getParameter("id");
            log.info("findPayDetail====>id:" + id);
            // 根据id查询支付详情信息
            PaySerAppAlipDto paySerAppAlipDto = paymentService.findPaySerAppAlipById(Integer.parseInt(id));
            if (null == paySerAppAlipDto || "".equals(paySerAppAlipDto)) {
                paySerAppAlipDto = new PaySerAppAlipDto();
            }
            model.put("alip", paySerAppAlipDto);
            model.put("appr", paySerAppAlipDto.getApprList());
            model.addAttribute("alip", paySerAppAlipDto);
            model.addAttribute("appr", paySerAppAlipDto.getApprList());
            log.info("findPayDetail====>paySerAppWxDto:" + JSONObject.fromObject(paySerAppAlipDto).toString());
        } catch (Exception e) {
            log.error("查询支付详情失败:", e.toString());
        }     }  

```