
## 项目说明

此项目为图书管理系统前台，使用vue.js,vue-resource,vue-router,iView2.0UI框架,vue-quill-editor等技术实现前台页面，后台采用的是springboot+mybatis等技术实现数据持久化以及api服务调用

## 项目启动
- 1.数据库：mysql5.6执行以下脚本,项目下脚本文件--sbm.sql
- 2.后台启动：导入项目，进入控制台，到项目所在路径，执行命令：mvn clean spring-boot:run
- 3.前台启动：导入项目，进入控制台，到项目所在路径，先执行命令：npm install安装依赖，再执行命令：npm run dev，访问地址：http://localhost:8075  进入到登录界面，打开数据库中t_reader表获取用户名和密码
- 增加了页面退出的效果，并对后台项目进行了代码重构和优化。


## 效果展示
![img.png](img.png)

## 后台项目地址

