docker 部署启动
将项目 my-site 拷贝服务器中，进入项目路径下进行打包测试.

#打包
mvn package
#启动
java -jar target/my-site-1.0.0.RELEASE.jar
看到 Spring Boot 的启动日志后表明环境配置没有问题，接下来我们使用 DockerFile 构建镜像。

mvn package docker:build
第一次构建可能有点慢，当看到以下内容的时候表明构建成功：

[INFO] Building image springboot/my-site
Step 1/4 : FROM openjdk:8-jdk-alpine
 ---> 224765a6bdbe
Step 2/4 : VOLUME /tmp
 ---> Using cache
 ---> bef8b6ae93aa
Step 3/4 : ADD my-site-1.0.0.RELEASE.jar app.jar
 ---> fe7f4dff781d
Step 4/4 : ENTRYPOINT java -Djava.security.egd=file:/dev/./urandom -jar /app.jar
 ---> Running in 7def475e609d
 ---> 66653a573692
Removing intermediate container 7def475e609d
ProgressMessage{id=null, status=null, stream=null, error=null, progress=null, progressDetail=null}
Successfully built 66653a573692
Successfully tagged springboot/my-site:latest
[INFO] Built springboot/my-site
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 25.119 s
[INFO] Finished at: 2018-05-03T19:16:05+08:00
[INFO] Final Memory: 48M/578M
[INFO] ------------------------------------------------------------------------
然后我们使用docker images命令查看镜像：

hendonghuadeMacBook-Pro% docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
springboot/my-site   latest              66653a573692        47 seconds ago      142MB
<none>               <none>              1bec22fbcef1        6 days ago          133MB
openjdk              8-jdk-alpine        224765a6bdbe        3 months ago        102MB

springboot/my-site 就是我们构件好的镜像，下一步就是运行该镜像

docker run -p 8080:8080 -t springboot/my-site
启动完成之后我们使用docker ps查看正在运行的镜像：

chendonghuadeMacBook-Pro% docker ps
CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                    NAMES
0617b1dc12f1        springboot/my-site   "java -Djava.secur..."   46 seconds ago      Up 44 seconds       0.0.0.0:8080->1111/tcp   festive_pike
启动成功之后访问： http://localhost:8080 访问首页 http://localhost:8080/admin 访问后台 默认账密：admin 123456
