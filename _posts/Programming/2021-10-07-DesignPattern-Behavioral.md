---
title: 디자인 패턴(행동패턴)
last_modified_at: 2021-10-07
categories: 
  - Programming
tags:
  - 디자인패턴
---
## 행동패턴 (Behavioral Pattern)

클래스 또는 객체의 책임 분배에 관련된 패턴

### 책임 연쇄 (Chain of Responsibility)

- 요청 객체와 처리객체를 분리한 디자인 패턴
- 여러 개의 객체 중에서 어떤 것이 요구를 처리할 수 있는지 사전에 알 수 없을 때 사용
- Sender : Handler에게 처리를 요청하는 클래스
- Handler : 요청을 처리하기 위한 Receiver를 가지는 인터페이스
- Receiver : Handler의 인터페이스 구현, 요청 종류에 따라 자신이 처리할 수 있는 부분을 정의하는 클래스

```java
/* Handler */
public abstract class Handler {
	public Handler nextHandler;
	protected int assignNum;

	public Handler setNext(Handler next) {
		Handler h = this;
		while (h.nextHandler != null)
			h = h.nextHandler;
		h.nextHandler = next;
		return this;
	}

	public void handleRequest(String msg, int requestNum) {
		if (assignNum == requestNum) {
			processRequest(msg);
		} else {
			nextHandler.handleRequest(msg, requestNum);
		}
	}
	abstract void processRequest(String msg);
}

/* Receiver1 */
public class Receiver1 extends Handler {
	public Receiver1(int num) {
		this.assignNum = num;
	}
	@Override
	void processRequest(String msg) {
		System.out.println("this \'" + msg + "\' is handled from Receiver 1");
	}
}

/* Receiver2 */
public class Receiver2 extends Handler {
	public Receiver2(int num) {
		this.assignNum = num;
	}

	@Override
	void processRequest(String msg) {
		System.out.println("this \'" + msg + "\' is handled from Receiver 2");
	}
}

```

```java
/* Sender */
@Test
public void TestChainOfResponsibility()
{
    Handler handler = new Receiver1(1).setNext(new Receiver2(2));
    handler.handleRequest("Please send this message to Receiver1 ", 1);
    handler.handleRequest("Please send this message to Receiver2 ", 2);
}

<Console>
this 'Please send this message to Receiver1 ' is handled from Receiver 1
this 'Please send this message to Receiver2 ' is handled from Receiver 2
```



### 커맨드 (Command)

- 호출부(Invoker)와 실행부(Receiver)를 객체로 캡슐화하여 처리하는 패턴
- Invoker : 기능의 실행을 호출하는 클래스
- Receiver : ConcreteCommand에서 구현한 기능을 실행하는 클래스
- Command : 실행될 기능을 정의한 인터페이스
- ConcreteCommand : 실행될 기능을 구현한 클래스

```java
/* Receiver */
public interface ISwitchable {
	void PowerOn();
	void PowerOff();
}

/* Receiver (Concrete Receiver) */
public class Light implements ISwitchable {
	@Override
	public void PowerOn() {
		System.out.println("The Light is On");
	}

	@Override
	public void PowerOff() {
		System.out.println("The Light is Off");
	}
}

/* Command */
public interface ICommand {
	void Execute();
}

/* Concrete Command 1/2 */
public class OpenSwitchCommand implements ICommand {
	private ISwitchable _switchable;
	public OpenSwitchCommand(ISwitchable switchable) {
		this._switchable = switchable; 
	}
	@Override
	public void Execute() {
		_switchable.PowerOn();
	}
}

/* Concrete Command 2/2 */
public class CloseSwitchCommand implements ICommand {
	private ISwitchable _switchable;

	public CloseSwitchCommand(ISwitchable switchable) {
		this._switchable = switchable;
	}

	@Override
	public void Execute() {
		_switchable.PowerOff();
	}
}

/* Invoker */
public class Switch {
	ICommand _closedCommand;
	ICommand _openedCommand;

	public Switch(ICommand openedCommand, ICommand closedCommand) {
		this._openedCommand = openedCommand;
        this._closedCommand = closedCommand;
	}

	public void Close() {
		this._closedCommand.Execute();
	}

	public void Open() {
		this._openedCommand.Execute();
	}

}
```

```java
@Test
public void TestCommand() {
    ISwitchable lamp = new Light();
    ICommand switchClose = new CloseSwitchCommand(lamp);
    ICommand switchOpen = new OpenSwitchCommand(lamp);

    Switch _switch = new Switch(switchOpen, switchClose);

    _switch.Open();
    _switch.Close();
}

<Console>
The Light is On
The Light is Off
```



### 인터프리터 (Interpreter)

- 일련의 규칙을 정의된 문법으로 해석하는 패턴
- Context : 인터프리터에 보내는 정보, String 표현식
- Client : interpret()을 호출하는 클래스
- AbstractExpression : interpret()을 정의하는 인터페이스 또는 추상 클래스
- TerminalExpression : interpret()을 구현하는 클래스
- NonTerminalExpression : Non-Terminal의 interpret()을 구현하는 클래스 

```java
/* AbstractExpression */
public interface Expression {
	public boolean interpret(String context);
}

/* Terminal Expression */
public class TerminalExpression implements Expression {
	private String data;

	public TerminalExpression(String data) {
		this.data = data;
	}

	@Override
	public boolean interpret(String context) {
		if(context.contains(data))
			return true;
		return false;
	}
}

/* Non-Terminal Expression 1/2 */
public class AndExpression implements Expression{
	private Expression expr1 = null;
	private Expression expr2 = null;
	
	public AndExpression(Expression expr1, Expression expr2) {
		this.expr1 = expr1;
		this.expr2 = expr2;
	}

	@Override
	public boolean interpret(String context) {
		return expr1.interpret(context) && expr2.interpret(context);
	}
}

/* Non-Terminal Expression 2/2 */
public class OrExpression implements Expression {
	private Expression expr1 = null;
	private Expression expr2 = null;

	public OrExpression(Expression expr1, Expression expr2) {
		this.expr1 = expr1;
		this.expr2 = expr2;
	}

	@Override
	public boolean interpret(String context) {
		return expr1.interpret(context) || expr2.interpret(context);
	}
}
```

```java
@Test
public void TestInterpreter() {
    Expression robert = new TerminalExpression("Robert");
    Expression john = new TerminalExpression("John");

    Expression julie = new TerminalExpression("Julie");
    Expression married = new TerminalExpression("Married");

    Expression isMale = new OrExpression(robert, john);
    Expression isMarriedWoman = new AndExpression(julie, married);
    System.out.println("isMale : " + isMale.interpret("john male"));
    System.out.println("isMarriedWoman : " + isMarriedWoman.interpret("Married Julie"));
}

<Console>
isMale : false
isMarriedWoman : true
```



### 반복자 (Iterator)

- 집합체 안에 들어있는 모든 항목에 접근할 수 있는 방법을 추상화한 패턴
- Iterator : 집합체의 요소들을 순서대로 검색하기 위한 인터페이스 정의
- ConcreteIterator : Iterator 인터페이스를 구현함
- Aggregate : 여러 요소들로 이루어져 있는 집합체
- ConcreteAggregate : Aggregate 인터페이스를 구현하는 클래스

```java
/* Aggregate */
public interface Aggregate {
	public Iterator iterator();
}

/* Iterator */
public interface Iterator  {
	public boolean hasNext();
	public Object next();
}

/* Concrete Iterator */
public class BookShelfIterator implements Iterator {
	private BookShelf bookShelf;
	private int index;

	public BookShelfIterator(BookShelf bookShelf) {
		this.bookShelf = bookShelf;
		this.index = 0;
	}

	public boolean hasNext() {
		if (index < bookShelf.getLength())
			return true;
		else
			return false;
	}

	@Override
	public Object next() {
		Book book = bookShelf.getBookAt(index);
		index++;
		return book;
	}
}

/* Concrete Aggregate */
public class BookShelf implements Aggregate {
	private Book[] books;
	private int last = 0;

	public BookShelf(int maxSize) {
		this.books = new Book[maxSize];
	}

	public Book getBookAt(int index) {
		return books[index];
	}

	public void appendBook(Book book) {
		this.books[last] = book;
		last++;
	}

	public int getLength() {
		return last;
	}

	public Iterator iterator() {
		return new BookShelfIterator(this);
	}

}

/* Element */
public class Book {
	private String name;

	public Book(String name) {
		this.name = name;
	}

	public String getName() {
		return this.name;
	}
}

```

```java
@Test
public void TestIterator() {
    BookShelf bookShelf = new BookShelf(4);
    bookShelf.appendBook(new Book("Book1"));
    bookShelf.appendBook(new Book("Book2"));
    bookShelf.appendBook(new Book("Book3"));

    Iterator it = bookShelf.iterator();
    while (it.hasNext()) {
        Book book = (Book) it.next();
        System.out.println(book.getName());
    }
}

<Console>
Book1
Book2
Book3
```





### 옵저버 (Observer)

- 관찰 중인 객체에서 발생하는 이벤트를 여러 다른 객체에 알리는 메커니즘을 정의 가능한 패턴
- Observer :  변경사항을 알릴 내용을 정의한 인터페이스
- Concretate Observer :  통보받을 변경사항을 구현할 클래스
- Subject : Observer 대상자를 등록, 해제하고 변경사항을 알리는 클래스

```java
/* Observer */
public interface Observer {
	void notify(String message);
}

/* Concrete Observer 1/2 */
public class ConcreteObserverA implements Observer {
	@Override
	public void notify(String message) {
		System.out.println("ConcreteObserser A is received message : " + message);
		
	}
}

/* Concrete Observer 2/2 */
public class ConcreteObserverB implements Observer {
	@Override
	public void notify(String message) {
		System.out.println("ConcreteObserser B is received message : " + message);
	}
}

/* Subject */
public interface Subject {
	void registerObserver(Observer observer);
	void unregisterObserver(Observer observer);
	void notifyObservers(String message);
}

/* Concrete Subject */
public class ConcreteSubject implements Subject {
	private final List<Observer> observers = new ArrayList<>();

	@Override
	public void registerObserver(Observer observer) {
		observers.add(observer);
	}

	@Override
	public void unregisterObserver(Observer observer) {
		observers.remove(observer);
	}

	@Override
	public void notifyObservers(String message) {
		for (Observer observer : observers) {
			observer.notify(message);
		}
	}
}
```

```java
@Test
public void TestObserver()
{
    ConcreteObserverA observerA = new ConcreteObserverA();
    ConcreteObserverB observerB = new ConcreteObserverB();
    ConcreteSubject subject = new ConcreteSubject();
    subject.registerObserver(observerA);
    subject.registerObserver(observerB);
    subject.notifyObservers("This message is test");
}

<Console>
ConcreteObserser A is received message : Hello. Everyone
ConcreteObserser B is received message : Hello. Everyone
```



### 전략 (Strategy)

- 행위를 클래스로 캡슐화하여 동적으로 행위를 자유롭게 바꿀수 있게 해주는 패턴
- Strategy : 알고리즘을 호출하는 방법을 명시하는 인터페이스
- ConcreteStrategy : 알고리즘을 구현한 클래스
- Context : 전략 패턴을 이용하는 클래스


```java
/* Strategy */
public interface Strategy {
	double GetPrice(double rawPrice);
}

/* Concrete Strategy 1/2 */
public class NormalStrategy implements Strategy{
	@Override
	public double GetPrice(double rawPrice) {
		return rawPrice;
	}
}

/* Concrete Strategy 2/2 */
public class SpecialStrategy implements Strategy {
	@Override
	public double GetPrice(double rawPrice) {
		return rawPrice * 0.5;
	}
}

/* Context */
public class Context {
	private Strategy salesStrategy;

	public Context(Strategy strategy) {
		this.salesStrategy = strategy;
	}

	public double printPrice(double rawPrice) {
		return salesStrategy.GetPrice(rawPrice);
	}
}
```

```java
@Test
public void TestStrategy() {
    Context context = new Context(new NormalStrategy());
    System.out.println(context.printPrice(100.0));
    context = new Context(new SpecialStrategy());
    System.out.println(context.printPrice(100.0));
}

<Console>
100.0
50.0
```









