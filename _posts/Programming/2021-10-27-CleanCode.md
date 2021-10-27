---
title: Clean Code 작성원칙
last_modified_at: 2021-10-27
categories: 
  - Programming
tags:
  - Clean Code
  - SOLID원칙
---

# Clean Code란?

- 모든 팀원이 이해하기 쉽도록 작성된 코드

- 의존적(Dependency)이지 않고 단순하고, 테스트 코드로 검증된 코드

# Clean Code 작성 원칙

## Naming

- 변수, 함수, 클래스 선언 시 의미 있는 이름을 사용할 것

``` java
AS-IS
class testClass{   
    static public int getVal(int a, int b)
    {
        return a + b;
    }
}
```

````java
TO-BE
class Calculator{   
    static public int add(int operA, int operB)
    {
        return operA + operB;
    }
}
````



## Style

- 행길이 및 들여쓰기 유지하고, 빈행으로 분리할 것

- 사용하는 변수와 종속관계의 함수는 최대한 가까이 배치할 것

```java
AS-IS
class Calculator{   
    static public int add(int operA, int operB) { return operA + operB; }
}
```

```java
TO-BE
class Calculator{   
    static public int add(int operA, int operB)
    {
        return operA + operB;
    }
}
```

## 주석

- 주석으로 나쁜 코드를 보완하지 말고, 코드로 의도를 표현할 것

- 좋은주석 : 저작권, 사용권, 구체적인 정보와 의도를 설명하는 주석, 결과를 경고하는 주석, TODO, 공개 API에 대한 javadocs 등

- 나쁜 주석 : 의무감으로 작성하는 주석, 코드 내용을 반복하는 주석, 주석 처리된 코드

```java
AS-IS
class Shop{   
    private double sales;
    public void setSales(double sales){ 
        this.sales = sales;
    }
    // sales는 할인율, 상품 가격조회 시, 할인된 가격을 제시
    // 할인율을 소수점 둘째 자리에서 반올림
    public double getPrice(Item item){
        double price = item.price;
        double res = Math.round(price * (1-sales));
        return res*100.0/100.0;
    }    
}
```

```java
TO-BE
class Shop{   
    private double allDiscount;
    public void setAllDiscount(double allDiscount){ 
        this.allDiscount = allDiscount;
    }
    public double getPriceWithDefaultDisCount(Item item){
        return Math.round(item.price * (1-this.allDiscount))*100.0/100.0;
    }
}
```

## Dead Code

- 사용하지 않는 코드는 삭제할 것

  ``` java
  class Shop{   
      private double allDiscount;
      public void setAllDiscount(double allDiscount){ 
          this.allDiscount = allDiscount;
      }
      public double getPriceWithDefaultDisCount(Item item){
          return Math.round(item.price * (1-this.allDiscount))*100.0/100.0;
      }
      /* Class 수정 완료
  	// sales는 인율, 상품 가격조회 시, 할인된 가격을 제시
      // 할인율을 소수점 둘째 자리에서 반올림
      public double getPrice(Item item){
          double price = item.price;
          double res = Math.round(price * (1-sales));
          return res*100.0/100.0;
      }        
      */
  }
  ```

  

## Method

- 함수에서 인수의 개수는 적을수록 좋다

```java
AS-IS
public Student updateStudent(id,age,classNum,avgGrade,name,address){
    // Student 객체 수정
}
```

```java
TO-BE
public Student updateStudent(Student student){
    // Student 객체 수정
}
```

- 최대한 단순하게 처리할 것

```java
AS-IS
public boolean isAdult(int age){
	if(age > 0){
        if(age >= 18)
            return true;
    }
    return false;
}
```

```java
TO-BE
public boolean isAdult(int age){
    return age >= 18;
}
```

- 단일 역할을 가지는 코드를 작성할 것

```java
AS-IS
class Person{
    public void doSomething(String action){
		    if("sleep".equals(action)) {
            //do Sleep..
        } else if("work".equals(action)) {
            //do work
        } else if("study".equals(action)) {
            //do study
        } else if("play".equals(action)) {
            //do play
        } else if("speak".equals(action)) {
            //do speak
        } ...            
    }
}   
```



```java
TO-BE
    
class Person{
    public void Sleep(){ ... }
    public void Work(){ ... }
    public void Study(){ ... }
    public void Play(){ ... }
    public void WriteReport(){ ... }
    public void speak(){ ... }
    ...
}
```



# Class

- 높은 응집도(Cohesion)를 유지하라

- 높은 응집도란? 클래스 내에 선언한 변수를 많은 메소드에서 사용할수록 응집도가 높음

  ```java
  public class Stack{
      private int topOfStack = 0;
      List<Integer> elements = new LinkedList<Integer>();
      
      public int size(){
          return topOfStack;
      }
      public void push(int element){
          topOfStack++;
          elements.add(element);
      }
      public int pop() throws PoppedWhenEmpty{
          if(topOfStack == 0)
              throw new PoppedWhenEmpty();
          int element = elements.get(--topOfStack);
          elements.remove(topOfStack);
          return element;
      }
         
  }   
  ```



# Class Design (S.O.L.I.D)

- Class 설계 5대 원칙을 고려하여 코드를 작성, SOLID 5대 원칙이라고 부름.

- Single Responsibility Principle(SRP) : 모든 클래스는 하나의 책임만 가지며, 하나의 액터에 대해서만 책임져야 한다.

  * 액터 : 시스템이 동일한 방식으로 변경되기를 원하는 집단
  * 책임 : 기능 또는 변경되는 부분
  * 예시 ) 음식점에서 일하는 종업원을 Class로 디자인하였을 때 SRP를 위배한 경우

- Extract Class 또는 신규 Class를 생성하여 SRP 원칙을 준수함

  ``` java
  public class Employeee
  {
      public 포스기Class 계산기객체;
      public 수저Class 수저객체;
      public 포크Class 포크객체;
      public 음식Class 음식객체;
      public 손님Class 손님객체;
      public 장바구니class 장바구니객체;
      public 부엌class 부엌객체;
      
      public void 서빙하기(){
          ... // Employee 클래스가 너무 많은 기능을 수행함.
              // 기능 변경 시, Employee 클래스의 변경사유가 너무 광범위 해짐. 책임이 많음
      }
      public void 주문받기(){
          ... 
      }
      public void 테이블세팅하기(){
          ...
      }
      public void 테이블치우기(){
          ...
      }
  
      public void 계산하기(){
          ...
      }
      public void 설거지하기(){
  		...
      }
      public void 요리하기(){
  		...
      }
      public void 장보기(){
  		...
      }
  }    
  ```

  ``` java
  public class 요리사
  {
      public 요리Interface 요리하기(요리Interface 요리){
          ... // 기능 추가 및 수정 시, 담당 class만 수정하면 됨.
      }
      public void 장보기(){
  		...
      }
  }
  public class 요리사보조원{
      public void 설거지하기(그릇Class 그릇){
          ...
      }
  }
  public class 홀서빙종웝원{
      public void 서빙하기(){
          ...
      }
      public void 주문받기(){
          ...
      }
      public void 테이블세팅하기(테이블Class 테이블, 포크Class 포크객체, 수저Class 수저객체){
          테이블.세팅(포크객체).세팅(수저객체);
      }
      public void 테이블치우기(테이블Class 테이블){
          테이블.치우기();
      }
  }
  public class 계산원{
      public int 계산하기(포스기Class 포스기객체, int 테이블번호, int 지불금액){
  		if(포스기객체.주문금액확인(테이블번호) > 지불금액)
              throw new Exception("주문한 금액보다 지불금액이 부족합니다");
  		return 포스기객체.계산하기(테이블번호, 지불금액).잔액(테이블번호);
      }
  }
  ```

  

- Open-Closed Principle(OCP) : 클래스는 확장에는 열려있고, 변경에는 닫혀 있어야 한다

  - 확장에 열려있다 => 새로운 동작 추가 또는 기존 동작을 변경가능함
  - 변경에는 닫혀있다 =>  코드 수정 불가능 ex) API 사용

- DownCasting, if-else구문을 자주 사용한다면 OCP원칙을 위배된 Class라고 볼 수 있음

- Dependency Injectection, 추상화, 다형성을 활용하여 OCP를 준수함

  ```java
  public class Chef{
      public void Cook라면(){ // 요리할 수 있는 종류가 추가될 때마다, Chef 클래스는 수정이 발생한다.
          System.out.println("라면 요리");        
      }
      public void Cook제육볶음(){
          System.out.println("제육볶음 요리");        
      }
      public void Cook불고기(){
          System.out.println("불고기 요리");
      }
  }
  ```

  ```java
  public class Chef{
      public void cook(Food food)
      {
          System.out.println(food.Name + " 요리") // 요리할 수 있는 메뉴가 추가되어도, Chef클래스는 변경사항이 없음.
      }       
  }
  ```

- Liskov Substitution Princible (LSP) : 자식 클래스는 자신의 부모 클래스에서 가능한 행위는 수행 할 수 있어야 한다

  - LSP 위반 => 명시된 명세에서 벗어난 값 리턴, 예외 발생, 기능 수행
  - 사각형 - 정사각형 상속관계에서 면적 결과 계산 오류문제 -> LSP 위반
  - LSP 위반 시 해결 방법 : 상속관계 제거 or 부모 클래스에 구현된 Method를 자식클래스로 이동

  ```java
  public class Rectangle
  {
      private int height;
      private int width;
      
      public int getHeight()
      {
      	return this.height;    
      }
      public int getWidth()
      {
          return this.width;
      }
      public int area()
      {
      	return this.getWidth() * this.getHeight();    
      }        
      public void setWidth(int width)
      {
      	this.width = width;    
      }
      public void setHeight(int height)
      {
          this.height = height;
      }
  }
  
  public class Square extends Rectangle
  {
      @Override
      public void setWidth(int width)
      {
          this.width = width;
          this.height= width;
      }
      @Override
      public void setHeight(int height)
      {
          this.width = height;
          this.height = height;
      }
  }
  
  
  class Test {
      static boolean checkAreaSize(Rectangle r) {
          r.setWidth(5);
          r.setHeight(4);
  
          if(r.area() != 20 ){
              return false; // Square 객체 생성 시, 면적의 결과가 의도와 다르게 출력되어 False가 됨 (LSP 위배)
          }
  
          return true;
      }
      public static void main(String[] args){
          Test.checkAreaSize(new Rectangle()); // true
          Test.checkAreaSize(new Square()); // false
      }
  }
  
  ```



- Interface Segregation Principle (ISP) : 인터페이스의 단일 원칙, 클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 한다. 인터페이스는 구체적이고 작은 단위들로 분리시켜 클라이언트들이 꼭 필요한 메서드들만 이용할 수 있게 한다.

```java
public interface Bird
{
    void sing();
    void eat();
    void fly();
}

public class Penguin implements Bird
{
    @Override
    public void sing()
    {
        ...
    }
	@Override
    public void eat()
    {
        ...
    }
    @Override
    public void fly()
    {
        ...// 펭귄은 날지 못함. 클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 함.
           // 인터페이스를 작은 단위로 나뉘어야 할 필요가 있음 
    }
}
```



```java
public interface Bird
{
    void sing();
    void eat();
}
public interface FlyableBird implements Bird
{
    void fly();
}

public class Penguin implements Bird
{
    @Override
    public void sing()
    {
        ... // Interface를 작은단위로 나눔으로써, 자신이 사용하는 메소드만 신경쓸 수 있게 됨
    }
	@Override
    public void eat()
    {
        ...
    }
}
public class Eagle implements FlyableBird
{
    @Override
    public void sing()
    {
        ...
    }
	@Override
    public void eat()
    {
        ...
    }
    @Override
    public void fly()
    {
        ...
    }
}
```

- Dependency Inversion Principle(DIP) : 추상화된 것은 구체적인 것에 의존하면 안된다.(자주 변경되는 구체적인 것에 의존하지 말고 추상화된 것을 참조하도록 구현)

  - ex) 전통적인 계층 형태의 상속이 아닌, 상위 하위 모듈이 추상적인 모듈에 의존하도록 만듬

```java
public class Adder{
    public double add(double addend, double augend)
    {
        return addend + augned;
    }
}

public class Calculator
{
    private Adder adder;
    public Calculator(){
        this.adder = new Adder(); // Adder 구현체 변경 시, 클라이언트 코드도 변경되야 함.
	}
    public double add(double addend, double augend)
    {
        return this.adder(addend, augend); // Calculator 모듈이 Adder모듈에 의존하게 됨.
    }
    public static void main(String[] args)
    {
        Calculator calc = new Calculator();
        System.out.println(calc.add(1,2));
    }
}

```

```java
public interface IAdder
{
    public double add(double addend, double augend);
}
public class Adder implements IAdder{
    @Override
    public double add(double addend, double augend)
    {
        return addend + augned;
    }
}

public class Calculator
{
    private IAdder adder;
    public Calculator(){
        this.adder = new Adder(); // Adder의 구현체만 변경하면 기존 Adder 코드는 변경하지 않아도 됨
	}
    public double add(double addend, double augend)
    {
        return this.adder(addend, augend);
    }
    public static void main(String[] args)
    {
        Calculator calc = new Calculator();
        System.out.println(calc.add(1,2));
    }
}
```

