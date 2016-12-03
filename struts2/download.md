# Struts2实现文件下载。
> Action配置  

  ```xml
  <action name="download_*" class="com.bestsnail.download.Download" method="{1}">
            <result name="list">/WEB-INF/page/list.jsp</result>
            <result name="downloadResult" type="stream">
                <!--运行下载的文件类型：指定为所有的二进制文件类型-->
                <param name="contentType">application/octet-stream</param>
                <!--对应的是Action中属性：返回留的属性-->
                <param name="inputName">attrInputStream</param>
                <!--<param name="inputName">attrInputStream</param>-->
                <!--下载头中包括：浏览器显示的文件名-->
                <param name="contentDisposition">attachment;filename=${downFileName}</param>
                <!--缓冲位置大小-->
                <param name="bufferSize">2048</param>
            </result>
  ```

##### 要使用Struts2实现文件下载，简单的分为三部分。
1、实现action中的访问方法  
```xml
<result name="downloadResult" type="stream">
  <!-- 对应的java实现 -->
```
```java
  public String downResule(){

        return "downloadResult";
    }
```
2、实现文件流的方法
```xml
<!--运行下载的文件类型：指定为所有的二进制文件类型-->
<param name="contentType">application/octet-stream</param>
<!-- 获取文件类型可以在tomcat-> web.xml 中查找 -->
```   
```java
public InputStream getAttrInputStream(){
      InputStream inputStream = null;
      try{
        //这里的fileName是通过请求时同时传递上来的参数可以通过对应的参数获方法获取
          File f = new File(path+"/"+fileName);
          inputStream = new FileInputStream(f);
      }catch (Exception e){
          new RuntimeException(e);
      }
      return inputStream;
  }
```
3、设置头内容让下载的时候能够得到文件对应正确的名字。
```xml
<!--下载头中包括：浏览器显示的文件名-->
<param name="contentDisposition">attachment;filename=${downFileName}</param>
```
```java
// 4. 下载显示的文件名（浏览器显示的文件名）
  public String getDownFileName() {
      // 需要进行中文编码
      try {
          fileName = URLEncoder.encode(fileName, "UTF-8");
      } catch (UnsupportedEncodingException e) {
          throw new RuntimeException(e);
      }
      return fileName;
  }
```
4、设置缓冲区的大小
```xml
<!--缓冲位置大小-->
<param name="bufferSize">2048</param>
```
