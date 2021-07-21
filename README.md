![](https://github.com/Lylio/image-repo/blob/master/logos/spring-cloud-config.png?raw=true)
![](https://github.com/Lylio/image-repo/blob/master/logos/server.png?raw=true)
# Configaro
### Description
A Spring Cloud Config Server.

### Docker Launch
1. `docker build -t configaro .`
2. `docker run -e GIT_USERNAME={username} -e GIT_PASSWORD={password} -p 8888:8888 configaro:latest`
3. Verify the config has been pulled at http://localhost:8888/application/default
4. A different config file can be pulled with http://localhost:8888/{application name}/default
   e.g http://localhost:8888/chatty-services/default

### Maven Launch
1. `mvn clean install`
2. `cd target/`
3. `java -jar configaro-0.0.1-SNAPSHOT.jar --git.username={username} --git.password={password}`
4. A different config file can be pulled with http://localhost:8888/{application name}/default
   e.g http://localhost:8888/chatty-services/default
