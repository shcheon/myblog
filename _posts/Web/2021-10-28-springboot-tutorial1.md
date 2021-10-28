---
title: "[SpringBoot] Getting Started Guides ) Building a RESTful Web Service"
date : 2021-10-28
last_modified_at: 2021-10-28
categories:
  - Web
tags:
  - SpringBoot
---

- SpringBoot로 RESTful 웹 서비스를 만드는 방법을 배워보자

# What You Will Build

- GET Request 를 처리하는 서비스 만들기

  ```
  http://localhost:8080/greeting
  ```

  GET Response 만들기  

  ```json
  {"id":1,"content":"Hello, World!"} 
  ```

- name parameter를 받을 수 있도록 greeting 서비스를 커스터 마이징하기

  ```
  http://localhost:8080/greeting?name=User
  ```

- name의 default value는 "World"로 출력할 것

  ```json
  {"id":1,"content":"Hello, User!"}
  ```

# What You Need

- 15분의 시간
- 텍스트 에디터 또는 IDE
- JDK 1.8 이상
- Gradle 4+ or Maven 3.2+
- Spring Tool Suite 또는 IntelliJ IDEA

# Create a Resource Representation Class

- Greeting GET 요청에 대한 응답 JSON 은 아래처럼 될 것이고, ID는 고유 값으로 증가할 것

  ```json
  {
      "id": 1,
      "content": "Hello, World!"
  }
  ```

- Greeting 응답을 위한 Representation Class는 아래와 같다

  ```java
  package com.example.restservice;
  
  public class Greeting {
      private final long id;
      private final String content;
  
      public Greeting(long id, String content) {
          this.id = id;
          this.content = content;
      }
  
      public long getId() {
          return id;
      }
  
      public String getContent() {
          return content;
      }
  }
  
  ```

# Create a Resource Controller

- HTTP Request는 Controller에 의해 처리됨

- 이 예제에서는 RestController Annotation으로 처리

- Greeting 처리용 GET HTTP Request 처리용 Controller Class는 아래와 같다

  ```java
  package com.example.restservice;
  
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.RequestParam;
  import org.springframework.web.bind.annotation.RestController;
  
  import java.util.concurrent.atomic.AtomicLong;
  
  @RestController
  public class GreetingController {
      private static final String template = "Hello, %s!";
      private final AtomicLong counter = new AtomicLong();
  
      @GetMapping("/greeting")
      public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
          return new Greeting(counter.incrementAndGet(), String.format(template, name));
      }
  }
  ```

- 클래스 뜯어 살펴보기

  - @GetMapping은 "/greeting" HTTP GET 요청을 greeting() 메소드와 대응시킴

  - @RequestParam은 파라메터로 전달되는 name의 값을 greeting()의 매개변수 name에 대응시킴, name 값이 전달되지 않았을 경우, defaultValue값으로 대체됨

  - 전통적인 MVC Controller와 RESTFul web service controller의 차이는 HTTP 응답 Body가 생성되는 방식이 다름.

  - MVC Controller의 경우는 HTML과 같은 view에 의존적인 ResponseBody를 만들어야 하지만, RESTful 웹서비스 컨트롤러는 JSON형태로 응답이 생성됨

  - 객체가 JSON으로 변환될 수 있는 이유는 SpringBoot 내부적으로 Jackson을 이용하기 때문임
 
  - @RestController는 모든 Method는 view대신 도메인 객체를 반환하는 클래스임을 명시하는 것

  - @RestController는 @Controller와 @ResponseBody가 합쳐진 축양형 Annotation임

- SpringBoot Main클래스에 달린 @SpringBootApplication은 아래 3개의 Annotatation 기능을 포함하고 있음

  - @Configuration : Spring의 Application Context에게 Bean을 정의하는 소스 임을 알리는 꼬리표

  - @EnableAutoConfiguration : Classpath 안에 정의된 Bean을 Spring Boot에 추가하는 작업을 시작하라는 의미

  - @ComponentScan : 개발자가 구현한 Component class를 찾으라는 의미

- SpringAppliocation.run() : SpringBoot application을 실행시키는 메소드

- SpringBoot 어플리케이션은 Web.xml 파일없이 순수 100% Java로 작성함

# Build an executable JAR

- Gradle or Maven 을 이용하여 Jar 생성



# Test the Service

1. SpringBoot 실행 

2.  웹 브라우저에서 http://localhost:8090/greeting 으로 GET Request를 전송하면 아래처럼 결과가 나옴

   ```json
   {"id":1,"content":"Hello, World!"}
   ```

3. Name을 지정하여 http://localhost:8090/greeting?nameUser GET Request  전송하면 아래처럼 결과가 나옴

   ```json
   {"id":2,"content":"Hello, User!"}
   ```

   
