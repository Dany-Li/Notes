# SpringMVC

标签（空格分隔）： JstlView

---（一）在 SpringMVC 中使用 Jstl 标签

##### 1. 先加入两个包
        * jstl.jar
        * standard.jar
        
##### 2. 编写资源文件（这里用国际化来做例子）
``` 
//i18n_en_US.properties
    i18n.username=Username
    i18n.password=Password
    
//i18n_zh_CN.properties
    i18n.username=\u7528\u6237\u540D
    i18n.password=\u5BC6\u7801
    
//i18n.properties
    i18n.username=Username
    i18n.password=Password
```

##### 3. 编写 jsp 页面（只是为了测试一下）
```
<!--导入标签资源文件-->
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>

//页面代码
<fmt:message key="i18n.username"></fmt:message>
<fmt:message key="i18n.password"></fmt:message>
```

##### 4. 配置国际化资源文件
```
<!-- 配置国际化资源文件 -->
<bean id="messageSource"
	class="org.springframework.context.support.ResourceBundl             eMessageSource">
	<property name="basename" value="i18n"></property>	
</bean>
```




--- （二）配置直接转发的页面

```
<!-- 配置直接转发的页面 -->
<!-- 可以直接相应转发的页面, 而无需再经过 Handler 的方法.  -->
<!-- 如果只配置这个的话，会使别的方法失效，所以得配置下面的标签 -->
<mvc:view-controller path="/success" view-name="success"/>
	
<!-- 在实际开发中通常都需配置 mvc:annotation-driven 标签 -->
<mvc:annotation-driven></mvc:annotation-driven>
```
       
       
--- （三）自定义视图
```
<!-- 配置视图解析器: 如何把 handler 方法返回值解析为实际的物理视图，这个的优先级默认的 order 是最大值，不用指定，越是常用的优先级越低 -->
<bean class="org.springframework.web.servlet.view.InternalResour    ceViewResolver">
		<property name="prefix" value="/WEB-INF/views/"></property>
		<property name="suffix" value=".jsp"></property>
</bean>
	
<!-- 配置视图  BeanNameViewResolver 解析器: 使用视图的名字来解析视图 -->
<!-- 通过 order 属性来定义视图解析器的优先级, order 值越小优先级越高 -->
<bean class="org.springframework.web.servlet.view.BeanNameViewRe    solver">
		<property name="order" value="100"></property>
</bean>
```

自定义视图时要实现 view 的接口
```
@Component
public class HelloView implements View{

	@Override
	public String getContentType() {
		return "text/html";
	}

	@Override
	public void render(Map<String, ?> model, HttpServletRequest         request,
			HttpServletResponse response) throws Exception {
		response.getWriter().print("hello view, time: " + new Date());
	}

}
```

--- （四）重定向

* 可以使用转发，当然也可以使用重定向
    * 重定向：
    `return "redirect:/index.jsp";`
    * 转发：
    `return "forward:/index.jsp";`



   






