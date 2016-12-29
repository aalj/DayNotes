ClassPathXmlApplicationContext 读取Spring对应的xml文件然后进行解析bean文件
InternalResourceViewResolver  Spring的试图解析器


	<url-pattern>/</url-pattern>  路径匹配，只能这样写，才能匹配任意路径

	  
	关于SpringMVC中如何使用重定向  固定格式   return “redirect:/url”；这是重定向到相应的地址下面
	  
	关于SpringMVC使用 response  request  的使用  
	  
	关于在SpringMVC中如何把数据传送到jsp页面中  使用model可以把数据传递到jsp页面
	  
	关于获取请求参数  使用注解在方法参数对对应的参数使用注解 @RequestParam(name = "name", defaultValue = "guest")   
															 name表示请求参数的方法名  defaultValue表示当没有该参数的时候的默认值
	
	
	对于获取用户上传的参数，可以通过HttpServletRequest和注解@RequestParam(value="",defaultValue="")来获取
	
	
	自己的理解，通过获取请求地址上面数据，然后能够做某些处理
		@RequestMapping("/getURL/{a}/{b}")//请求的url，
		
		@PathVariable(value = "a")String A,//获取参数对应url a 字段，同时把值赋值给A
		@PathVariable(value = "b")String B,//获取参数对应url b 字段，同时把值赋值给B
	
	类似接口显示发送简单的字符串内容，
		使用@RequestBody来实现，也可食用HttpRequest的getWriter方法来实现
	
	
  
  