# Linux Centos系统设置tomcat开机自启

> 对于设置程序开机自启，最本质的就是把程序设置成系统服务，然后在系统启动的时候启动该服务。

## 一、主要会用的工具  chkconfig

> chkconfig主要包括5个原始功能：  
> 	1. 为系统管理增加新的服务  
> 	2.为系统管理移除服务  
> 	3.列出单签服务的启动信息  
> 	4.改变服务的启动信息  
> 	5.检查特殊服务的启动状态。
> 	当单独运行chkconfig命令而不加任何参数时，他将显示服务的使用信息。

#### 必要参数
```
--add 开启制指定的服务程序   
--del  关闭指定的服务程序  
--list 列出chkconfig所知道的所有的服务
```

## 二、设置tomcat开机启动
1.切换到tomcat目录下  
```	
cd /tomcat/bin
	```
2.使用vim打开tomcat的启动脚本添加chkconfig能够识别的相关内容，保存退出。添加启动tomcat需要的所有环境变量
 ```
vim startup.sh
    #chkconfig: 2345 80 90    
    #description:tomcat auto start    
    #processname: tomcat
    <!--‘#’ 不要省略，这个不是注释-->
	```
3.在`catalina.sh` 文件中添加启动tomcat需要的所有环境变量
```
vim catalina.sh 
    <!--在文件开始处添加java 的环境变量-->   
    export JAVA_HOME=/mnt/jdk1.8.0_121   
    export JRE_HOME=/mnt/jdk1.8.0_121/jre
    <!--在 export QIBM_MULTI_THREADED=Y 之后添加tomcat环境变量（大约169行左右）-->
    export CATALINA_BASE=/home/test/tomcat
    export CATALINA_HOME=/home/test/tomcat
    <!--保存退出-->
	```
4.添加连接 
```
ln -s /tomcat/bin/startup.sh /etc/rc.d/init.d/tomacat7
<!--通过ln -s 将startup.sh 文件连接到init.d目录下-->
	```
5.切换到 `/etc/rc.d/init.d` 目录下 通过 `ll` 命令查看tomcat7 是否是可执行文件  
```
<!--可执行文件执行命令-->
chmod +x tomcat7
	```
6.最后添加开机启动服务
```
chkconfig --add tomcat7
```
7.通过 `chkconfig --list` 查看服务是否添加成功
        
 
        
