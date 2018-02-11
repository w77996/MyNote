1.修改springboot server port
	
	java -jar xxx.jar --server.port=8888
	相当于在application.properties中设定server.port=8888

2.屏蔽命令行改参数

	SpringApplication.setAddCommandLineProperties(false)

3.springboot多环境

	下面，以不同环境配置不同的服务端口为例，进行样例实验。

	针对各环境新建不同的配置文件
	application-dev.properties、application-test.properties、application-prod.properties
	在这三个文件均都设置不同的server.port属性
	如：dev环境设置为1111，test环境设置为2222，prod环境设置为3333
	application.properties中设置spring.profiles.active=dev，就是说默认以dev环境设置
	测试不同配置的加载
	1.执行java -jar xxx.jar，可以观察到服务端口被设置为1111，也就是默认的开发环境（dev）
	2.执行java -jar xxx.jar --spring.profiles.active=test，可以观察到服务端口被设置为2222，也就是测试环境的配置（test）
	3.执行java -jar xxx.jar --spring.profiles.active=prod，可以观察到服务端口被设置为3333，也就是生产环境的配置（prod）

	按照上面的实验，可以如下总结多环境的配置思路：
	
	application.properties中配置通用内容，并设置spring.profiles.active=dev，以开发环境为默认配置
	application-{profile}.properties中配置各个环境不同的内容
	通过命令行方式去激活不同环境的配置

4.创建SpringBoot项目

	mvn archetype:generate -DgroupId=springboot -DartifactId=springboot-helloworld -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false