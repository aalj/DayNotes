访问使用的注解

| 注解 | 解释| 参数说明 | 使用说明 |   
| ------- | ----- | --------------- | ------------ |   
| @Controller| 访问控制器| x  | @Controller表示的是MVC中的C层（控制层），用于表示当前类的是一个访问控制器 |
| @RequestMapping   | 表示访问的地址 | value=“/url”   ,method=RequestMethod.GET | 设置访问的地址 |
| RequestMethod.XXX | 访问的方法，六大访访问方法，(GET,POST,DELETE) | X  | 请求方法 |
| @RequestParam     | 通过注解获取客户端的请求参数  | value=“请求参数的的name”,DefaultValue="当默认参数没有的时候的默认值" | 在访问的方法参数中使用，在使用的参数前面添加此注解，在通过该url访问大的时候能够把请求参数赋值到该参数中 |
| @PathVariable     |    |       |         |

| 注解             | 解释   | 参数说明 | 使用说明 |   
| -------------- | ---- | ---- | ---- |   
| @Configuration |      |      |      |   
| @EnableWebMvc  |      |      |      |   
| @ComponentScan |      |      |      |   
