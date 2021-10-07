---
title: 디자인 패턴(구조패턴)
last_modified_at: 2021-10-07
categories: 
  - Programming
tags:
  - 디자인패턴
---

## 구조패턴 (Structural Pattern)

클래스 또는 객체를 조합하여 더 큰 구조의 S/W를 만드는 패턴

### 어댑터 (Adapter)

- 새로운 클래스를 기존 클래스 구조 변경없이 호환되도록 만드는 패턴

- Wrapper라고도 부름

- 인터페이스를 이용하여 두 개 이상의 클래스 간의 연관 관계를 맺어줄 수 있음

- 구현방식으로는 Class Adapter와 Object Adapter 두가지로 가능함

  - Class Adpater : 다중상속(클래스 상속, 인터페이스 상속)을 활용하여 구현
  - Object Apdater : 객체 주입을 통해 다른 클래스와 연관관계를 만듦
  
- Object Apdater 구현 예제

```java
/* Target */
public interface _110V {
	public void charge();
}

/* Adapter */
public class _110Vto220VAdapter implements _110V {
	public _220V TwoTwoZeroVolt = new _220V();
	@Override
	public void charge() {
		System.out.println("Convert 110V to 220V");
		TwoTwoZeroVolt.charge();
		System.out.println("Complete charge");
	}
}

/* Adaptee */
public class _220V {
	public void charge() {
		System.out.println("220V is Charging");
	}
}
```

```java
/* Client */
@Test
public void TestAdpater()
{
    _110V volt = new _110Vto220VAdapter();
    volt.charge();
}

<Console>    
Convert 110V to 220V
220V is Charging
Complete charge
```

### 브릿지(Bridge)

- 추상화를 구현으로부터 분리하여 각각 독립적으로 클래스를 변형할 수 있는 패턴
- Abstraction : 기능을 명시하는 추상클래스 또는 인터페이스, 구현 객체를 통해 구현 메소드를 호출함
- RefinedAbstraction : 기능 계층에서 새로운 부분을 확장한 클래스
- Implementor : abstraction의 기능을 명시하는 인터페이스
- ConcreteImplementor : 일반 클래스, 실제 기능 구현하는 클랙스

```java
/* Abstraction */
public interface ShapeBridge {
	public void draw();
	public void resizeByPercentage(double pct);
}

/* Refined Abstraction */
public class CircleShape implements ShapeBridge {
	private double x, y, radius;
	private DrawingAPI drawingAPI;

	public CircleShape(double x, double y, double radius, DrawingAPI drawingAPI) {
		this.x = x;
		this.y = y;
		this.radius = radius;
		this.drawingAPI = drawingAPI;
	}

	@Override
	public void draw() {
		drawingAPI.drawCircle(x, y, radius);
	}

	@Override
	public void resizeByPercentage(double pct) {
		radius *= pct;
	}
}

/* Implementor */
public interface DrawingAPI {
	public void drawCircle(double x, double y, double radius);
}

/* ConcreteImplementor 1/2 */
public class DrawingAPI1 implements DrawingAPI {
	@Override
	public void drawCircle(double x, double y, double radius) {
		System.out.printf("API1.circle at %f:%f radius %f\n", x, y, radius);
	}
}

/* ConcreteImplementor 2/2 */
public class DrawingAPI2 implements DrawingAPI {
	@Override
	public void drawCircle(double x, double y, double radius) {
		System.out.printf("API2.circle at %f:%f radius %f\n", x, y, radius);
	}
}
```

```java
@Test
public void TestBridge()
{
    ShapeBridge[] shapes = new ShapeBridge[2];

    shapes[0] = new CircleShape(1, 2, 3, new DrawingAPI1());
    shapes[1] = new CircleShape(4, 5, 6, new DrawingAPI2());

    for (ShapeBridge shape : shapes) {
        shape.resizeByPercentage(2);
        shape.draw();
    }
}

<Console>
API1.circle at 1.000000:2.000000 radius 6.000000
API2.circle at 4.000000:5.000000 radius 12.000000
```



### 컴포지트(Composite)

- 객체를 트리 구조로 구성하여 부분-전체 계층으로 표현하는 패턴
- Component : 공통 기능을 명세하는 인터페이스
- Leaf : 인터페이스의 기능을 구현하는 클래스
- Composite : Component 요소를 자식으로 가지며, 또한 이를 관리하기 위한 추가,삭제 메소드를 지니는 클래스

```java
/* Component */
public interface FileNode {
	public String getName();
}

/* Leaf */
public class File implements FileNode {
	private String name;
	@Override
	public String getName() {
		return name;
	}
}

/* Composite */
public class Directory implements FileNode {
	private String name;
	private List<FileNode> children = new ArrayList<FileNode>();
	@Override
	public String getName() {
		return name;
	}
	public void add(FileNode node) {
		children.add(node);
	}
}
```

```java
@Test
public void TestComposite() {
    Directory dir = new Directory();
    dir.add(new File());
    dir.add(new Directory());
    Directory secondDir =new Directory();
    secondDir.add(dir);
}
```

### 데코레이터 (Decorator)

- 객체에 동적으로 새로운 책임을 덧붙이는 패턴
- Wrapper라고도 불림
- Component : ConcreteComponent, Decorator 두 객체를 동등하게 다루기 위한 인터페이스
- ConcreteComponent : 기본 기능을 추가하는 클래스
- Decorator : 신규 기능을 추가할 추상 클래스
- ConcreteDecorator : Decorator에 명시된 기능을 구현하는 클래스

```java
/* Component */
public interface ChristmasTree {
	public String decorate();
}

/* ConcreteComponent */
public class DefaultChristmasTree implements ChristmasTree {
	@Override
	public String decorate() {
		return "Christmas Tree";
	}
}

/* Decorator */
public abstract class TreeDecorator implements ChristmasTree {
	private ChristmasTree christmasTree;

	public TreeDecorator(ChristmasTree christmasTree) {
		this.christmasTree = christmasTree;
	}

	@Override
	public String decorate() {
		return christmasTree.decorate();
	}
}


/* ConcreteDecorator */
public class Flowers extends TreeDecorator {
	public Flowers(ChristmasTree christmasTree) {
		super(christmasTree);
	}
	public String addFlowers() {
		return " with Flowers";
	}
	@Override
	public String decorate() {
		return super.decorate() + addFlowers();
	}
}

/* ConcreteDecorator */
public class Lights extends TreeDecorator {
	public Lights(ChristmasTree christmasTree) {
		super(christmasTree);
	}
	public String addLitght() {
		return " with Lights";
	}
	@Override
	public String decorate() {
		return super.decorate() + addLitght();
	}
}
```

```java
@Test
public void TestDecorator()
{
    ChristmasTree tree = new DefaultChristmasTree();
    System.out.println(tree.decorate());

    ChristmasTree treeWithLights = new Lights( new DefaultChristmasTree());
    System.out.println(treeWithLights.decorate());

    ChristmasTree treeWithLightsAndFlowers = new Flowers(new Lights(new DefaultChristmasTree()));
    System.out.println(treeWithLightsAndFlowers.decorate());
}

<Console>
Christmas Tree
Christmas Tree with Lights
Christmas Tree with Lights with Flowers
```



### 퍼사드 (Facade)

- 라이브러리, 프레임워크, 거대한 복잡한 클래스 집합에 대해 간략화된 인터페이스를 제공하는 패턴

```java
/* SubSystem1 */
public class Battery {
	public void connect() {
		System.out.println("Battery is connected With SmartPhone");
	}
	public void disconnect() {
		System.out.println("Battery is disconnected With SmartPhone");
	}
}

/* SubSystem2 */
public class Screen {
	public void connect() {
		System.out.println("Screen is connected With SmartPhone");
	}
	public void disconnect() {
		System.out.println("Screen is disconnected With SmartPhone");
	}
}

/* SubSystem3 */
public class MicroComputer {
	public void connect() {
		System.out.println("MicroComputer is connected With SmartPhone");
	}
	public void disconnect() {
		System.out.println("MicroComputer is disconnected With SmartPhone");
	}
}

/* Facade */
public class SmartPhone {
	private Battery battery = new Battery();
	private MicroComputer mc = new MicroComputer();
	private Screen screen = new Screen();

	public void on() {
		System.out.println("SmartPhone starts to power on");
		battery.connect();
		mc.connect();
		screen.connect();
		System.out.println("SmartPhone completes to power on");
	}

	public void off() {
		System.out.println("SmartPhone starts to power off");
		battery.disconnect();
		mc.disconnect();
		screen.disconnect();
		System.out.println("SmartPhone completes to power off");
	}
}
```

```java
@Test
public void TestFacade() {
    SmartPhone sp = new SmartPhone();
    sp.on();
    sp.off();
}

<Console>
SmartPhone starts to power on
Battery is connected With SmartPhone
MicroComputer is connected With SmartPhone
Screen is connected With SmartPhone
SmartPhone completes to power on
SmartPhone starts to power off
Battery is disconnected With SmartPhone
MicroComputer is disconnected With SmartPhone
Screen is disconnected With SmartPhone
SmartPhone completes to power off
```



