## phonegap前期准备

> cordova(phonegap) 两者区别：

1. node.js安装
   1. 下载相应的安装跑进行安装即可

2. 安装phonegap
   1. npm install -g phonegap
   2. npm install -g cordova

3. 生成第一个应用 
   1. 创建一个cordova项目     phonegap create projectName packageName;
   2. 创建一个android工程   cordova platforms add android （等同于phonegap run android，但是执行该句会出现无法执行下去的情况。）
   3. cordova  build   编译执行android工程（这里需要连接真机或者测试机）

4. cordova项目目录结构
   1.  phonegap create xxxx   创建的文档结构

       ![cordova项目结构](./png/cordova project.png)

   2. cordova platforms add Android 创建的Android项目结构

      ![android项目结构](./png/cordova_android.png)

   3. cordova 为js提供的js 插件 结构

      ![Codova 定义的js文件](./png/cordova_android_assets_www.png)

   4. android 的创建的资源文件内容

      ![android 的资源文件](./png/cordova_android_res.png)

5. js 插件定义     结构定义

6. js调用原生api 的 方法书写规范

7. Android路由表结构

8. Android 与js交互插件的定义书写

9. 在activity中页面的呈现

10. 在activity页面中同时存在html页面和原生Android控件

11. js传送数据给activity

12. activity给js

13. 加载网络页面同时能够与Android进行交互

    ​