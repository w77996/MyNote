
1.查看当前进程

	grep 是搜索
	例如： ps -ef | grep java
	表示查看所有进程里CMD是java的进程信息
	ps -aux | grep java
	-aux 显示所有状态
	netstat -tunlp|grep [端口号]
	
2.结束进程

	例如： kill -9 [PID]
	-9表示强迫进程立即停止

3.tomcat日志实时查看

	至tomcat目录下logs
	tail -f catalina.out