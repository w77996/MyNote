1.maven 安装本地jar包

	<dependency>  
	    <groupId>org.springframework</groupId>  
	    <artifactId>spring-context-support</artifactId>  
	    <version>3.1.0.RELEASE</version>  
	</dependency> 
	
	mvn install:install-file   
	-Dfile=jar包的位置   
	-DgroupId=上面的groupId   
	-DartifactId=上面的artifactId   
	-Dversion=上面的version   
	-Dpackaging=jar  
	
	mvn install:install-file   
	-Dfile=jar包的位置   
	-DgroupId=上面的groupId   
	-DartifactId=上面的artifactId   
	-Dversion=上面的version   
	-Dpackaging=jar  