## Jenkins 介绍

> 简介		

	Jenkins 是一个开源项目，提供了一种易于使用的持续集成系统，使开发者从繁杂的集成中解脱出来，
	专注于更为重要的业务逻辑实现上。同时 Jenkins能实施监控集成中存在的错误，提供详细的日志文件
	和提醒功能，还能用图表的形式形象地展示项目构建的趋势和稳定性

> 优点

	1.采用shell自定义脚本,控制集成部署环境更加方便灵活

	2.精简war包中的lib包,常驻tomcat里，减少war包传输时间

	3.Jenkins 用户权限管理，不让淘气鬼乱动

	4.构建失败发邮件通知相关人员解决

	5.自动按天备份war包,Jenkins配置备份以及版本控制化

> 功能

	1、项目的"自动化"构建，编译、打包、分发部署。

	2、监控外部调用执行的工作。

## 下载安装

	wget  https://pkg.jenkins.io/redhat-stable/jenkins-2.138.3-1.1.noarch.rpm
	rpm -ivh jenkins-2.138.3-1.1.noarch.rpm
	service jenkins start

	1. 访问 localhost:8080
	2. 初次访问需要输入默认密码，文件路径为  /var/lib/jenkins/secret.key
	3. 确定后配置 admin 密码，以后登陆就用这个密码

## 页面介绍
	




[jenkins 参数配置说明](https://blog.csdn.net/taishanduba/article/details/61423121)
