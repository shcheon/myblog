---
title: "[SpringBoot] Demo 어플리케이션 프로젝트 설정 및 실행"
date : 2021-10-28
last_modified_at: 2021-10-28
categories:
  - Web
tags:
  - SpringBoot
---

# SpringBoot DemoApplication 실행

- start.spring.io 사이트에서 demo project를 다운받는다
- 아래와 같은 RestController를 추가한 후 실행, 참고로 RestController를 사용하기 위해 "spring-boot-starter-web" dependency가 추가되어야 한다
- 소스코드와 실행 결과는 아래와 같음
- Springboot실행 시, port를 8090으로 변경해 봄
- 웹 브라우저에서 URL을 localhost:8090/hello 로 입력하면, 화면 상에서 Hello World가 출력되면 HTTP GET 요청, 응답을 받은 것임



```properties
/* application.properties */
server.port=8090
```

```java
/* DemoApplication.java */

package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args)
	{
		SpringApplication.run(DemoApplication.class, args);
	}
}
```

```java
/* TestController.java */
@RestController
public class TestController {
    @GetMapping("/hello")
    public String hello(@RequestParam(value = "name", defaultValue = "World") String name) {
        return String.format("Hello  %s!",name);
    }
}
```

```java
/* 실행 결과 */
.   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.5.6)

2021-10-28 11:31:07.858  INFO 27920 --- [           main] com.example.demo.DemoApplication         : Starting DemoApplication using Java 1.8.0_241 on KORND000979 with PID 27920 (C:\Users\sh2.cheon\Desktop\demo\demo\target\classes started by sh2.cheon in C:\Users\sh2.cheon\Desktop\demo\demo)
2021-10-28 11:31:07.863  INFO 27920 --- [           main] com.example.demo.DemoApplication         : No active profile set, falling back to default profiles: default
2021-10-28 11:31:09.239  INFO 27920 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8090 (http)
2021-10-28 11:31:09.251  INFO 27920 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2021-10-28 11:31:09.251  INFO 27920 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.54]
2021-10-28 11:31:09.509  INFO 27920 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2021-10-28 11:31:09.511  INFO 27920 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1555 ms
2021-10-28 11:31:10.071  INFO 27920 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8090 (http) with context path ''
2021-10-28 11:31:10.084  INFO 27920 --- [           main] com.example.demo.DemoApplication         : Started DemoApplication in 2.887 seconds (JVM running for 5.008)
2021-10-28 11:31:10.152  INFO 27920 --- [nio-8090-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2021-10-28 11:31:10.152  INFO 27920 --- [nio-8090-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2021-10-28 11:31:10.153  INFO 27920 --- [nio-8090-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 1 ms

Process finished with exit code -1

```

