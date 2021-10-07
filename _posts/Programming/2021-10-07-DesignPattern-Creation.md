---
title: 디자인 패턴(생성패턴)
last_modified_at: 2021-10-07
categories: 
  - Programming
tags:
  - 디자인패턴
---

# 디자인 패턴 

- 프로그래머가 어플리케이션이나 시스템을 디자인 할 때, 공통된 문제들을 해결하는데 쓰이는 형식화 된 가장 좋은 관행
- Programming Language 별로 디자인 패턴을 구현하는 소스코드 구조가 다를 수 있음



## 생성패턴 (Creational Pattern)

인스턴스를 만드는 절차를 추상화하는 패턴

### 싱글턴 (Singleton)

- 프로그램 내에서 오직 하나의 인스턴스가 생성됨을 보장하는 패턴

- 프로그램 어디에서든 인스턴스를 접근 할 수 있음
  
```java
public class Singleton {
	private static Singleton singleton = null;

	// 외부에서 직접 생성하지 못하도록 private 선언
	private Singleton() {
	}

	// 오직 1개의 객체만 생성
	public static Singleton getInstance() {
		if (singleton == null) {
			singleton = new Singleton();
		}

		return singleton;
	}
}
```

```java
@Test
public void TestSingleton()
{
    for( int i = 0; i < 5; i++ ){
        System.out.println(Singleton.getInstance().toString());
    }
}

<Console>    
SingleObj@98c449
SingleObj@98c449
SingleObj@98c449
SingleObj@98c449
SingleObj@98c449
```

### 프로토타입 (Prototype)

- 이미 존재하는 객체를 이용하여, 새로운 객체를 생성하는 패턴

- 객체 생성 시간이 많이 소요될 때 주로 사용 (외부 시스템으로부터 데이터를 수신하여 객체를 생성하는 경우)

- 주로 java의 clone 메소드를 통해 구현

```java
import java.util.ArrayList;
import java.util.List;
/* Prototype */
public class Prototype implements Cloneable {

	private List<String> datalist;

	public Prototype() {
		datalist = new ArrayList<String>();
	}

	public Prototype(List<String> data) {
		this.datalist = data;
	}
	public List<String> getDataList() {
		return this.datalist;
	}

	public boolean loadData() {
		boolean res = true;
		// data loading하는 경우는 DB 또는 Http통신 등 다양한 방법으로 이루어 질 수 있음
		res &= datalist.add("data1");
		res &= datalist.add("data2");
		res &= datalist.add("data3");
		res &= datalist.add("data4");
		return res;
	}

	@Override
	protected Object clone() throws CloneNotSupportedException {
		List<String> tmp = new ArrayList<String>(); // 깊은 복사를 통해 객체 내부 데이터를 복제함
		for (String data : datalist) {
			tmp.add(data);
		}
		return new Prototype(tmp);
	}
}
```

```java
@Test
public void TestProtoType() throws CloneNotSupportedException {
    Prototype proto = new Prototype();
    proto.loadData();

    Prototype cloneProto = (Prototype)proto.clone();

    cloneProto.getDataList().remove(0);
    System.out.println("Address : "+proto+"\tOriginal Object : "+proto.getDataList());
    System.out.println("Address : "+cloneProto+"\tClone Object : "+cloneProto.getDataList());
}

<Console>    
Address : cleancode.designpattern.Prototype@3b7951	Original Object : [data1, data2, data3, data4]
Address : cleancode.designpattern.Prototype@514713	Clone Object : [data2, data3, data4]

```



### 팩토리 메소드 (Factory Methods)

- 객체 생성을 서브클래스에게 위임하는 디자인 패턴
- 클래스 생성과 비즈니스 처리 로직을 분리하여 결합도를 낮추기 위해 사용

```java
/* Creator */
public abstract class Factory {
	public Shape getShape(ShapeType type) {
		return this.create(type);
	}

	protected abstract Shape create(ShapeType type);
}

/* Creator1 */
public class FactoryShape extends Factory {

	@Override
	public Shape create(ShapeType type) {

		switch (type) {
		case Triangle:
			return new ShapeTriangle();
		case Rectangle:
			return new ShapeRectangle();
		default:
			throw new RuntimeException(type.toString() + "is not existed");
		}
	}
}

/* Product */
public abstract class Shape {
	abstract public String draw();
}

/* Product1 */
public class ShapeRectangle extends Shape{
	@Override
	public String draw() {
		return "Rectangle";
	}
}
public class ShapeTriangle extends Shape {
	@Override
	public String draw() {
		return "Triangle";
	}
}
public enum ShapeType {
	Triangle, Rectangle
}

```

```java
@Test
public void TestFactoryMethod(){
    Factory factory = new FactoryShape();
    Shape shapeObj = factory.getShape(ShapeType.Rectangle);	
    Shape shapeObj2 = factory.getShape(ShapeType.Triangle);
    System.out.println(shapeObj.draw());
    System.out.println(shapeObj2.draw());
}

<Console>
Rectangle
Triangle 
```

### 빌더  (Builder)

- 다양한 상태의 객체 생성이 필요할 때 사용하는 객체 생성 패턴

- 객체 생성과 표현부를 분리할 수 있음

```java
/* Product */
public class Pizza {
	private String dough = "";
	private String sauce = "";
	private String topping = "";

	public void setDough(String dough) {
		this.dough = dough;
	}

	public void setSauce(String sauce) {
		this.sauce = sauce;
	}

	public void setTopping(String topping) {
		this.topping = topping;
	}

	public String getDough() {
		return this.dough;
	}

	public String getSauce() {
		return this.sauce;
	}

	public String getTopping() {
		return this.topping;
	}
}


/* Builder */
public abstract class PizzaBuilder {
	protected Pizza pizza;
	
	public Pizza getPizza(){
		return pizza;
	}
	
	public void createNewPizzaProduct(){
		pizza = new Pizza();
	}
	
	public abstract void buildDough();
	
	public abstract void buildSauce();
	
	public abstract void buildTopping();
}

/* ConcreteBuilder */
public class HawaiianPizzaBuilder extends PizzaBuilder {
	@Override
	public void buildDough() {
		pizza.setDough("cross");
	}

	@Override
	public void buildSauce() {
		pizza.setSauce("mild");
	}

	@Override
	public void buildTopping() {
		pizza.setTopping("ham,pineApple");
	}
}
public class SpicyPizzaBuilder extends PizzaBuilder {
	@Override
	public void buildDough() {
		pizza.setDough("pan baked");
	}

	@Override
	public void buildSauce() {
		pizza.setSauce("hot");
	}

	@Override
	public void buildTopping() {
		pizza.setTopping("pepperoni,salami");
	}
}

/* Director */
public class Cook {
	private PizzaBuilder pizzaBuilder;

	public void setPizzaBuilder(PizzaBuilder pizzaBuilder) {
		this.pizzaBuilder = pizzaBuilder;
	}

	public Pizza getPizza() {
		return pizzaBuilder.pizza;
	}

	public void constructPizza() {
		pizzaBuilder.createNewPizzaProduct();
		pizzaBuilder.buildDough();
		pizzaBuilder.buildSauce();
		pizzaBuilder.buildTopping();
	}
}
```

```java
@Test
public void TestBuilder() {
    Cook cook = new Cook();
    PizzaBuilder hawaiianPizzaBuilder = new HawaiianPizzaBuilder();
    PizzaBuilder spicyPizzaBuilder = new SpicyPizzaBuilder();

    cook.setPizzaBuilder(hawaiianPizzaBuilder);
    cook.constructPizza();

    Pizza pizza = cook.getPizza();
    System.out.println(pizza.toString() + "," + pizza.getDough() + "," + pizza.getSauce() + "," + pizza.getTopping());

    cook.setPizzaBuilder(spicyPizzaBuilder);
    cook.constructPizza();

    pizza = cook.getPizza();
    System.out.println(pizza.toString() + "," + pizza.getDough() + ","+ pizza.getSauce() + "," + pizza.getTopping());

}

Console
cleancode.designpattern.builder.Pizza@1663380,cross,mild,ham,pineApple
cleancode.designpattern.builder.Pizza@137e0d2,pan baked,hot,pepperoni,salami
```



### 추상 팩토리 (Abstract Factory)

- 서로 관련된 객체들을 동시에 생성할 때 이를 추상화 된 인터페이스로 객체생성 방식을 제공하는 패턴 
- 팩토리 메소드로 객체를 생성할 경우, 클래스간 결합도가 증가하는 문제가 발생하여 이를 해결하고자 사용

```java
/* AbstractFactory */
public abstract class GUIFactory {
	public abstract Button createButton();
	public abstract Scroll createScroll();
}

/* Factory1 */
public class GUILinuxFactory extends GUIFactory {
	@Override
	public Button createButton() {
		return new LinuxButton();
	}

	@Override
	public Scroll createScroll() {
		return new VerticalScroll();
	}

}

/* Factory2 */ 
public class GUIWinFactory extends GUIFactory {

	@Override
	public Button createButton() {
		return new WinButton();
	}

	@Override
	public Scroll createScroll() {
		return new HorizontalScroll();
	}

}

/* Product1 */
public abstract class Button {
	public abstract void paint();
}
public class LinuxButton extends Button{
	@Override
	public void paint() {
		System.out.println("Draw Linux Button");
	}
}
public class WinButton extends Button{
	@Override
	public void paint() {
		System.out.println("Draw Win Button");		
	}
}

/* Product2 */
public abstract class Scroll {
	public abstract void paint();
}
public class VerticalScroll extends Scroll{
	@Override
	public void paint() {
		System.out.println("VerticalScroll");
	}
}
public class WinButton extends Button{
	@Override
	public void paint() {
		System.out.println("Draw Win Button");		
	}
}
```

```java
@Test
public void TestAbstractFactory() throws Exception
{
    GUIFactory factory = new GUIWinFactory();
    Button button = factory.createButton();
    Scroll scroll = factory.createScroll();
    button.paint();		
    scroll.paint();

    factory = new GUILinuxFactory();
    button = factory.createButton();
    scroll = factory.createScroll();
    button.paint();		
    scroll.paint();
}

<Console>
Draw Win Button
Draw HorizontalScroll
Draw Linux Button
Draw VerticalScroll
```


