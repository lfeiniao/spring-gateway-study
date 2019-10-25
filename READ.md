# Spring Gateway 学习笔记
### spring Cloud Gateway 环境，
1.先创建一个空白的springboot 项目
2.加入Spring Gateway 依赖
```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```
3.创建springboot 启动类。
4.配置网关，有两种方式
- 1.在配置文件yml 中配置- 
- 2.通过@Bean 自定义RouterLocator 



```yaml
server:
  port: 8080
spring:
  cloud:
    gateway:
      routes:
      - id: neo_route  #路由的ID，保持唯一
        uri: http://www.ityouknow.com  #目标服务器地址
        predicates:    #断言，路由条件
        - Path=/spring-cloud
```
```java
@Bean
	public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
		return builder.routes()
				.route("path_route", r -> r.path("/about")  // path_route , 路由id ,path 路径匹配
						.uri("http://ityouknow.com")) // uri 目标服务器地址
				.build();
	}
```
### spring cloud Gateway 的路由断言（规则）
通过时间：
```yaml
spring:
  cloud:
    gateway:
      routes:
       - id: time_route
        uri: http://ityouknow.com
        predicates:
         - After=2018-01-20T06:06:06+08:00[Asia/Shanghai]  # 通过时间规则
		 #-Before=2018-01-20T06:06:06+08:00[Asia/Shanghai]
		 # - Between=2018-01-20T06:06:06+08:00[Asia/Shanghai], 2019-01-20T06:06:06+08:00[Asia/Shanghai]
		 #- Cookie=ityouknow , liuyang  # 通过cookie匹配，=名称，值
		 #- Header=x-Request-Id, \d+ # 通过header 进行匹配
		 #- Host=**。ityouknow.com #通过Host 来进行路由
		 #- Method=GET #通过请求方式匹配
		 #- Path=/foo #通过请求路径匹配
		 #- Query=smile, \d+ # 通过请求参数，前面是参数名称，后面是正则表单式
		 #- RemoteAddr=192.168.1.1/24, #通过IP地址匹配
		 #以上的路由规则可以配合使用，同时满足才会跳转
```