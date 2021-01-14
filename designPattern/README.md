# DesignPattern

- [Singleton](#singleton)
- [Prototype](#prototype)
- [Builder](#builder)
- [Factory Method](#factory-method)
- [Abstract Factory](#abstract-Factory)
- [Adapter](#adapter)
- [Composite](#composite)
- [Decorator](#decorator)
- [Bridge](#bridge)
- [Proxy](#proxy)

## Singleton

> 싱글턴 패턴은 생성 패턴 중의 하나이다. 그 중에서도 객체 생성에 관련 된 패턴이다.  
> 전역 변수를 사용하지 않으며, 객체를 하나만 생성하도록 하며, 생성된 객체를 어디에서든지 참조 할 수 있도록 하는 패턴이다.  
> 객체 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다.  

1. 객체 하나만 생성하는 이유는 커넥션 풀, 스레드 풀, 디바이스 설정 객체 등의 경우, 인스턴스를 여러 개 만들게 되면 자원을 낭비하게 되거나 버그를 발생시킬 수 있기 때문이다. 즉 자원의 낭비 와 동기화 관련 이슈로 인한 버그 발생을 방지하기 위한 목적이다.
2. 싱글턴 패턴을 작성할때 고려해야할 점은 첫째는 new 키워드 등을 통해서 생성을 하지 못하게 하는 것, 둘째는 동기화(비동기적으로 동시에 접근하면 여러 객체가 생성 되는가?)를 고려 하는 것(성능 조심), 셋째는 임계구역으로 Lock이 일어나는가 정도다.
3. 싱글턴 패턴을 가장 쉽게 만드는 방법은 1. 생성자를 막고, 2. 로딩 시점에서 객체를 생성하여, 3. static 메서드 같은 메서드로 생성된 메서드를 반환하는 방식으로 코드를 작성하는 것이다.
4. 그 외에도 객체 생성에 관한 동기화(멀티 스레드 환경), 정적 클래스, enum 클래스 등 여러가지 방법이 있다.

### 예제

1. 자바
```java
public class Singleton {
    private static volatile Singleton singletonObject = new Singleton();

    private Singleton() {}

    public static Singleton getSingletonObject() {
        return singletonObject;
    }
}
```
2. 자바스크립트 class 이용
```js
class Singleton {
    static getSingletonObject() {
        if(this.singletonObject === undefined){
            this.singletonObject = {}; 
        }
        return this.singletonObject;
    } 
}
```
3. 자바스크립트 클로저 이용
```js
const Singleton = (() => {
    let singletonObject;
    // 자바스크립트에서는 객체 리터럴 방식 = 싱글턴이다. 단 캡슐화가 불가능하기에 즉시실행 함수를 통한 싱글턴이 구현가능하다.
    const init = () => { 
        return {
            publicFun :  () => {
                return {

                };
            },
            publicProp : "core js"
        }
    }

    return {
        getSingletonObject : () => {
            if (!singletonObject) {
                singletonObject = init();
            }

            return singletonObject;
        }
    };
})();    
```

## Prototype

> 프로토타입 패턴은 생성 패턴 중 하나이다. 그중 객체 생성에 관한 패턴이다.  
> 프로토타입 패턴은 서브클래싱을 필요로 하지 않는 대신, 초기화 동작을 필요로 한다.  
> 생성할 객체들의 타입이 프로토타입인 인스턴스로부터 결정되도록 하며, 인스턴스는 새 객체를 만들기 위해 자신을 복제(clone)하게 된다.

1. 프로토타입 패턴은 추상 팩토리 패턴과는 반대로 클라이언트 응용 프로그램 내에서 객체 창조자(Creator)를 서브 클래스(SubClass)하는 것을 피하게 해준다.
2. 일반적으로 새로운 객체를 생성하는 일반적인 방법으로 객체를 생성하는 고유의 비용이 불가피하게 클때 이 비용을 감소시켜준다.
3. 객체를 복사 생성하는 방식으로 다수의 객체를 생성할 경우 객체 생성 비용을 효과적으로 감소 시킬 수 있다. 또 서브 클래스 수를 줄일 수 있다.
4. 원형 패턴의 핵심 요소 3가지 : Client 클래스는 복제 요청과 새로운 객체를 생성하는 역할, Prototype 클래스는 복제하는 인터페이스 정의, ConcreatePrototype 클래스는 복제하는 연산을 구현
5. 자바스크립트는 기본적으로 프로토 타입 언어라서 딱히 뭔가 추가적으로 알아야 할 부분은 없는거 같다. 그저 객체 생성 비용에 대해서 고려해볼만하다.


### 예제

1.자바
```java
/** Prototype Class **/
public class Cookie implements Cloneable {

   public Object clone() {
      try {
         Cookie copy = (Cookie)super.clone();

         //In an actual implementation of this pattern you might now change references to
         //the expensive to produce parts from the copies that are held inside the prototype.

         return copy;

      }
      catch(CloneNotSupportedException e) {
         e.printStackTrace();
         return null;
      }
   }
}

/** Concrete Prototypes to clone **/
public class CoconutCookie extends Cookie { }

/** Client Class**/
public class CookieMachine {

   private Cookie cookie;//could have been a private Cloneable cookie;

   public CookieMachine(Cookie cookie) {
      this.cookie = cookie;
   }
   public Cookie makeCookie() {
      return (Cookie)cookie.clone();
   }
   public Object clone() { }

   public static void main(String args[]) {
      Cookie tempCookie =  null;
      Cookie prot = new CoconutCookie();
      CookieMachine cm = new CookieMachine(prot);
      for (int i=0; i<100; i++)
         tempCookie = cm.makeCookie();
   }
}
```

## Builder

> 빌더 패턴은 생성 패턴 중 하나이다. 그중 객체 생성에 관한 패턴이다.  
> 불필요한 생성자를 만들지 않고 객체를 만든다. 데이터 순서에 상관 없이 객체를 만들어 낸다.  
> 생성할 객체의 프로퍼티들을 순서에 상관 없이 때로는 필요 없는 파라메터를 무시하고, 인자로 주고 빌드를 통해서 하나의 객체로 생성한다.

1. 빌더 패턴은 프로퍼티의 순서나 타입이 달라서 에러가 나는 상황을 막아준다.
2. 불필요한 생성자를 만들지 않고, 사용자가 봤을때 명시적이며 이해하기 쉽다.
3. 빌더 패턴은 Director(빌더 호출), Builder(빌더 프로퍼티 set), ConcreteBuilder(set된 프로퍼티로 객체 생성), product(생성된 객체)로 구성 되어있다.

### 예제

1. 자바
```java
/** "Product" */
class Pizza {
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
}

/** "Abstract Builder" */
abstract class PizzaBuilder {
	protected Pizza pizza;

	public Pizza getPizza() {
		return pizza;
	}

	public void createNewPizzaProduct() {
		pizza = new Pizza();
	}

	public abstract void buildDough();

	public abstract void buildSauce();

	public abstract void buildTopping();
}

/** "ConcreteBuilder" */
class HawaiianPizzaBuilder extends PizzaBuilder {
	public void buildDough() {
		pizza.setDough("cross");
	}

	public void buildSauce() {
		pizza.setSauce("mild");
	}

	public void buildTopping() {
		pizza.setTopping("ham+pineapple");
	}
}

/** "ConcreteBuilder" */
class SpicyPizzaBuilder extends PizzaBuilder {
	public void buildDough() {
		pizza.setDough("pan baked");
	}

	public void buildSauce() {
		pizza.setSauce("hot");
	}

	public void buildTopping() {
		pizza.setTopping("pepperoni+salami");
	}
}

/** "Director" */
class Cook {
	private PizzaBuilder pizzaBuilder;

	public void setPizzaBuilder(PizzaBuilder pizzaBuilder) {
		this.pizzaBuilder = pizzaBuilder;
	}

	public Pizza getPizza() {
		return pizzaBuilder.getPizza();
	}

	public void constructPizza() {
		pizzaBuilder.createNewPizzaProduct();
		pizzaBuilder.buildDough();
		pizzaBuilder.buildSauce();
		pizzaBuilder.buildTopping();
	}
}

/** A given type of pizza being constructed. */
public class BuilderExample {
	public static void main(String[] args) {
		Cook cook = new Cook();
		PizzaBuilder hawaiianPizzaBuilder = new HawaiianPizzaBuilder();
		PizzaBuilder spicyPizzaBuilder = new SpicyPizzaBuilder();

		cook.setPizzaBuilder(hawaiianPizzaBuilder);
		cook.constructPizza();

		Pizza pizza = cook.getPizza();
	}
}
```
2. 자바스크립트
```js
class PersonInfoBuilder {
    constructor(name, age, favoriteColor, favoriteAnimal, favoriteNumber){
        this.name = name || null;
        this.age = age || null;
        this.favoriteColor = favoriteColor || null;
        this.favoriteAnimal = favoriteAnimal || null;
        this.favoriteNumber = favoriteNumber || null;
    }
    setName(name){
        this.name = name;
        return this;
    }
    setAge(age){
        this.age = age;
        return this;
    }
    setFavoriteColor(color){
        this.favoriteColor = color;
        return this;
    }
    setFavoriteAnimal(animal){
        this.favoriteAnimal = animal;
        return this;
    }
    setFavoriteNumber(number){
        this.favoriteNumber = number;
        return this;
    }
    build(){
        return {
            name : this.name,
            age : this.age,
            favoriteColor : this.favoriteColor,
            favoriteAnimal : this.favoriteAnimal,
            favoriteNumber : this.favoriteNumber
        };
    }
}
```
3. 자바스크립트 함수 방식
```js
const personInfoBuilder = (name) => {
    let PersonAge, favoriteColor, favoriteAnimal, favoriteNumber;
    return {
        setAge : function(age) {
            PersonAge = age;
            return this;
        },
        setFavoriteColor : function(color) {
            favoriteColor = color;
            return this;
        },
        setFavoriteAnimal : function(animal) {
            favoriteAnimal = animal;
            return this;
        },
        setFavoriteNumber : function(number) {
            favoriteNumber = number;
            return this;
        },
        build : () => {
            return {
                name : name,
                age : PersonAge,
                favoriteColor : favoriteColor,
                favoriteAnimal : favoriteAnimal,
                favoriteNumber : favoriteNumber
            };
        }
    };
};
```

## Factory Method

> 팩토리 메서드 패턴은 생성 패턴의 한 종류이다. 객체 생성에 관한 패턴이다.  
> 팩토리 메서드 패턴은 객체 생성을 서브 클래스로 별도로 분리해 캡슐화 하는 패턴이다. 클래스간의 결합도를 낮추기 위한 것.  
> 팩토리 메서드 패턴은 Product(공통 인터페이스), ConcreteProduct(객체가 생성되는 클래스), Creator(팩토리 메서드를 갖는 클래스), ConcreteCreator(팩토리 메서드 구현 클래스 ConcreateProduct 객체 생성)로 구성되어있다.

1. 객체 생성을 전담하는 별도의 Factory 클래스를 이용(스트레인지, 싱글턴), 상속을 이용한다.
2. 직접 객체를 생성해 사용하는 것을 방지하고 서브 클래스에 위임해서 의존성을 제거한다.
3. 팩토리 매서드 패턴은 객체 생성 클래스간의 결합도를 낮추기 위한 것이다. 변경점이 생겼을때 최대한 다른 클래스에 영향이 가지 않도록한다.
### 예제

1.C#
```C#
public abstract class Pizza
{
    public abstract decimal GetPrice();

    public enum PizzaType
    {
        HamMushroom, Deluxe, Seafood
    }
    public static Pizza PizzaFactory(PizzaType pizzaType)
    {
        switch (pizzaType)
        {
            case PizzaType.HamMushroom:
                return new HamAndMushroomPizza();

            case PizzaType.Deluxe:
                return new DeluxePizza();

            case PizzaType.Seafood:
                return new SeafoodPizza();

        }

        throw new System.NotSupportedException("The pizza type " + pizzaType.ToString() + " is not recognized.");
    }
}
public class HamAndMushroomPizza : Pizza
{
    private decimal price = 8.5M;
    public override decimal GetPrice() { return price; }
}

public class DeluxePizza : Pizza
{
    private decimal price = 10.5M;
    public override decimal GetPrice() { return price; }
}

public class SeafoodPizza : Pizza
{
    private decimal price = 11.5M;
    public override decimal GetPrice() { return price; }
}

// Somewhere in the code
...
  Console.WriteLine( Pizza.PizzaFactory(Pizza.PizzaType.Seafood).GetPrice().ToString("C2") ); // $11.50
...
```
2. 자바스크립트
```js
//Our pizzas
function HamAndMushroomPizza(){
  const price = 8.50;
  this.getPrice = function(){
    return price;
  }
}

function DeluxePizza(){
  const price = 10.50;
  this.getPrice = function(){
    return price;
  }
}

function SeafoodPizza(){
  const price = 11.50;
  this.getPrice = function(){
    return price;
  }
}

//Pizza Factory
function PizzaFactory(){
  this.createPizza = function(type){
     switch(type){
      case "Ham and Mushroom":
        return new HamAndMushroomPizza();
      case "DeluxePizza":
        return new DeluxePizza();
      case "Seafood Pizza":
        return new SeafoodPizza();
      default:
          return new DeluxePizza();
     }
  }
}

//Usage
const pizzaPrice = new PizzaFactory().createPizza("Ham and Mushroom").getPrice();
alert(pizzaPrice);
```

## Abstract Factory

> 추상 팩토리 패턴은 생성 패턴의 하나이다, 그중 객체 생성을 위한 패턴이다.  
> 구체적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공 하는 패턴,다양한 구성요소 별로 이루어진 객체의 집합을 생성할때 유용하다.  
> 추상 팩토리는 AbstractFactory(실제 팩토리 클래스의 공통 인터페이스), ConcreteFactory(추상 팩토리 클래스의 추상 메서드를 오버라이드 해서 제품생성), ConcreteProduct(구체적인 팩토리 클래스에서 생성되는 객체), AbstractProduct(제품들의 공통 인터페이스) 로 구성되어 있다

1. 관련성 있는 여러 종류의 객체를 일관된 방식으로 생성하는데 유용하다. 싱글턴, 팩토리 매서드를 사용한다.
2. 같은 종류의 완제품의 부품이 제각기 달라진다 하더라도 완제품 입장에서는 같은 종류의 부품을 사용한다는 점은 동일하다.
3. 팩토리 메서드 패턴을 좀더 캡슐화해서 결합도를 낮춘 방법이라고 볼 수도 있다. mvc1 => mvc2처럼

### 예제

1.PHP
```php
interface Button
{
    public function paint();
}

interface GUIFactory
{
    public function createButton(): Button;
}

class WinFactory implements GUIFactory
{
    public function createButton(): Button
    {
        return new WinButton();
    }
}

class OSXFactory implements GUIFactory
{
    public function createButton(): Button
    {
        return new OSXButton();
    }
}

class WinButton implements Button
{
    public function paint()
    {
        echo "Windows Button";
    }
}

class OSXButton implements Button
{
    public function paint()
    {
        echo "OSX Button";
    }
}

$appearance = "osx";

$factory = NULL;

switch ($appearance) {
    case "win":
        $factory = new WinFactory();
        break;
    case "osx":
        $factory = new OSXFactory();
        break;
    default:
        break;
}

if ($factory instanceof GUIFactory) {
    $button = $factory->createButton();
    $button->paint();
}
```
2.자바스크립트
```js
/** interface */
class ButtonFactory {
    createButton () {
    }
}
class Button {
    paint () {
    }
}
/** Abstract Factory */
class GUIFactory {
    constructor(){
        this._os = {};
    }
    setOs (os, factory) {
       if(typeof os === "string" && factory.prototype.createButton){
           this._os[os] = factory;

           return this;
       } 
    }
    create (os) {
        const GUIFact = this._os[os];

        return GUIFact ? new GUIFact() : null;
    }
}

/** GUI Factory */
class WinFactory extends ButtonFactory {
    createButton () {
        return new WinButton();   
    }
}
class OSXFactory extends ButtonFactory{
    createButton () {
        return new OSXButton();   
    }
}

/** product */
class WinButton extends Button {
    paint () {
        console.log("Windows Button");
    }
}
class OSXButton extends Button {
    paint () {
        console.log("OSX Button");
    }
}

/** example */

/** 추상 팩토리 생성 */
const AbstractFactory = new GUIFactory();
/** 추상 팩토리를 통한 Concrete Factory 생성 */
const winGUI = AbstractFactory.setOs('win', WinFactory).create('win');
const OSXGUI = AbstractFactory.setOs('OSX', OSXFactory).create('OSX');
/** Concrete Factory를 통한 Concrete Product 생성 */
const winGUIButton = winGUI.createButton();
const OSXGUIButton = OSXGUI.createButton();
winGUIButton.paint();
OSXGUIButton.paint();
```

## Adapter

> 어댑터 패턴은 구조(Structural)패턴 중 하나이다. 객체 어댑터와 클래스 어댑터가 존재한다.  
> 한 클래스의 인터페이스를 클라이언트에서 사용하고자 하는 다른 인터페이스로 변환한다. 어댑터 패턴을 사용하면 인터페이스 호환성 문제를 어느 정도 해결 가능하다.  
> 어댑터 패턴은 Client(호출), Target(어댑터가 구현하는 인터페이스), Adapter(Client{Target}와 Adaptee 중간 연결), Adaptee(써드 파티, 변경되는 인터페이스) 구조로 구성되어있다.

1. 호환되지 않는 여러 인터페이스를 클라이언트에서 그대로 활용 할 수 있게 만들어준다.
2. 추후 인터페이스에 변경 사항이 있더라도 변경 내역이 어댑터 선에서 캡슐화 되기에 클라이언트 까지 영향이 가지 않는다.
3. API와 호출 하는 클라이언트 간의 결합도를 낮추어주는 패턴이라고 볼 수 있다.

### 예제

1.파이썬 오브젝트 어댑터
```py
# Python code sample
class Target(object):
    def specific_request(self):
        return 'Hello Adapter Pattern!'

class Adapter(object):
    def __init__(self, adaptee):
        self.adaptee = adaptee

    def request(self):
        return self.adaptee.specific_request()

client = Adapter(Target())
print client.request()
```
2. 자바 클래스 어댑터
```java
/**
 * Java code sample
 */

/* the client class should instantiate adapter objects */
/* by using a reference of this type */
interface Stack<T>
{
  void push (T o);
  T pop ();
  T top ();
}

/* DoubleLinkedList is the adaptee class */
class DList<T>
{
  public void insert (DNode pos, T o) { ... }
  public void remove (DNode pos) { ... }

  public void insertHead (T o) { ... }
  public void insertTail (T o) { ... }

  public T removeHead () { ... }
  public T removeTail () { ... }

  public T getHead () { ... }
  public T getTail () { ... }
}

/* Adapt DList class to Stack interface is the adapter class */
class DListImpStack<T> extends DList<T> implements Stack<T>
{
  public void push (T o) {
    insertTail (o);
  }

  public T pop () {
    return removeTail ();
  }

  public T top () {
    return getTail ();
  }
}
```

## Composite

> 컴포지트 패턴은 구조 패턴 중 하나이다. 주로 객체의 관계를 정의할때 사용된다.  
> 여러개의 객체들로 구성된 복합 객체와 단일 객체를 클라이언트 구분 없이 다룰 수 있다. 객체들의 관계를 트리구조로 부분-전체 계층을 표현한다.  
> 컴포지트 패턴은 Component(구체적인 부분), Leaf(구체적인 부분 클래스), Composite(전체 클래스) 로 구성 되어있다.

1. 컴포넌트라는 구체적인 객체들을 일반화(추상화)한 추상 클래스를 정의하고 해당 추상 클래스를 통해서 Leaf 와 Composite를 만든다.
2. 컴포지트에서 Leaf를 추가하고 제거해서 하나의 컴포지트(부분-전체) 객체를 만든다.
3. 컴포지트 패턴은 SOLID의 OCP를 준수한다.
4. 간단히 생각하면 그냥 비슷한 구조로 짜인 것들을 한군데서 모아서 처리한다. 그런 방식으로 짜니 변경의 영향은 덜 받으면서 확장에는 용이한 것



### 예제

1.Java
```java
import java.util.List;
import java.util.ArrayList;

/** "Component" */
interface Graphic {

    //Prints the graphic.
    public void print();

}

/** "Composite" */
class CompositeGraphic implements Graphic {

    //Collection of child graphics.
    private List<Graphic> mChildGraphics = new ArrayList<Graphic>();

    //Prints the graphic.
    public void print() {
        for (Graphic graphic : mChildGraphics) {
            graphic.print();
        }
    }

    //Adds the graphic to the composition.
    public void add(Graphic graphic) {
        mChildGraphics.add(graphic);
    }

    //Removes the graphic from the composition.
    public void remove(Graphic graphic) {
        mChildGraphics.remove(graphic);
    }

}


/** "Leaf" */
class Ellipse implements Graphic {

    //Prints the graphic.
    public void print() {
        System.out.println("Ellipse");
    }

}


/** Client */
public class Program {

    public static void main(String[] args) {
        //Initialize four ellipses
        Ellipse ellipse1 = new Ellipse();
        Ellipse ellipse2 = new Ellipse();
        Ellipse ellipse3 = new Ellipse();
        Ellipse ellipse4 = new Ellipse();

        //Initialize three composite graphics
        CompositeGraphic graphic = new CompositeGraphic();
        CompositeGraphic graphic1 = new CompositeGraphic();
        CompositeGraphic graphic2 = new CompositeGraphic();

        //Composes the graphics
        graphic1.add(ellipse1);
        graphic1.add(ellipse2);
        graphic1.add(ellipse3);

        graphic2.add(ellipse4);

        graphic.add(graphic1);
        graphic.add(graphic2);

        //Prints the complete graphic (four times the string "Ellipse").
        graphic.print();
    }
}
```

## Decorator

> 데코레이터 패턴은 구조 패턴 중 하나이다. 객체 책임에 관한 패턴이다.  
> 객체 결합을 통해서 기능을 동적으로 확장 가능한 패턴이다. 서브클래싱의 대안이 된다.  
> 데코레이터 패턴은 Component(공통 기능을 정의), ConcreteComponent(기본 기능 클래스), Decorator(다수 존재하는 구체적인 Decorator 공통 기능 제공), ConcreteDecorator(Decorator 하위의 개별적인 기능들)

1. 객체에 동적으로 새로운 책임을 추가 할 수 있고, 상속과 다르게 경우의 수를 위해서 여러 서브 클래스를 만들 필요가 없다.
2. 전체 클래스에 새로운 기능을 추가할 필요는 없지만, 개별적인 객체에 새로운 책임을 추가할 필요가 있을때 대안이된다.
3. 필요한 기능을 추가하는 다른 객체에다가 해당 구성요소를 둘러싸는 것. 이렇게 무엇인가를 감싸는 객체를 장식자(decorator)라고 한다.
4. 장식자는 자신이 둘러싼 요소, 구성요소가 갖는 인터페이스를 자신도 동일하게 제공하므로, 장식자의 존재는 이를 사용하는 사용자에게 은닉된다.
5. 장식자는 자신이 둘러싼 구성요소로 전달되는 요청을 중간에 가로채서 해당 구성요소에 전달해 줍니다.
6. 그렇기 때문에 이 전달 과정의 앞뒤에 다른 작업을 추가로 할 수 있고, 여기에는 투명성이 존재하기 때문에 장식자의 중첩이 가능해져 이를 통해 책임 추가를 유연하게 무한정으로 가능하게 된다.

### 예제

1. c#
```c#
#include <iostream>

using namespace std;

/* Component (interface) */
class Widget {

public:
  virtual void draw() = 0;
  virtual ~Widget() {}
};

/* ConcreteComponent */
class TextField : public Widget {

private:
   int width, height;

public:
   TextField( int w, int h ){
      width  = w;
      height = h;
   }

   void draw() {
      cout << "TextField: " << width << ", " << height << '\n';
   }
};

/* Decorator (interface) */
class Decorator : public Widget {

private:
   Widget* wid;       // reference to Widget

public:
   Decorator( Widget* w )  {
     wid = w;
   }

   void draw() {
     wid->draw();
   }

   ~Decorator() {
     delete wid;
   }
};

/* ConcreteDecoratorA */
class BorderDecorator : public Decorator {

public:
   BorderDecorator( Widget* w ) : Decorator( w ) { }
   void draw() {
      Decorator::draw();
      cout << "   BorderDecorator" << '\n';
   }
};

/* ConcreteDecoratorB */
class ScrollDecorator : public Decorator {
public:
   ScrollDecorator( Widget* w ) : Decorator( w ) { }
   void draw() {
      Decorator::draw();
      cout << "   ScrollDecorator" << '\n';
   }
};

int main( void ) {

   Widget* aWidget = new BorderDecorator(
                     new ScrollDecorator(
                     new TextField( 80, 24 )));
   aWidget->draw();
   delete aWidget;
}
```

## Bridge

> 브릿지 패턴은 구조 패턴 중 하나이다.  
> 구현과 추상 두개의 계층을 분리해서 서로 변형하기 수월하게 한 패턴이다.  
> 브릿지 패턴은 Abstraction(기능 계층 최상위 추상 인터페이스 Implementor와 레퍼런스를 유지), RefinedAbstraction(기능 계층 확장 추상 클래스),Implementor(Abstraction 클래스 구현 인터페이스), ConcreteImplementor(실제 기능 구현)로 구성 되어있다.

1. 브릿지는 기능을 구현하는 방법에서 추상 기능과 구현부를 나누어서 만드는 구조의 패턴이다.
2. 어댑터 패턴과 많이 유사한데, 어댑터 패턴은 설계가 완료된후 특정 요구사항을 위해서 적용하는 것, 브릿지 패턴은 설계 중에 적용하는 차이가 있다.
3. 추상, 구현 계층의 의존성주입을 통해서 두 계층의 결합도를 낮추고, 응집도를 높이기 위한 패턴이다. 즉 의존성 주입 설계를 통해서 결합도를 => 응집도로 전환하는 패턴

### 예제

1. Java
```java
interface DrawingAPI
{
    public void drawCircle(double x, double y, double radius);
}

/** "ConcreteImplementor" 1/2 */
class DrawingAPI1 implements DrawingAPI
{
   public void drawCircle(double x, double y, double radius)
   {
        System.out.printf("API1.circle at %f:%f radius %f\n", x, y, radius);
   }
}

/** "ConcreteImplementor" 2/2 */
class DrawingAPI2 implements DrawingAPI
{
   public void drawCircle(double x, double y, double radius)
   {
        System.out.printf("API2.circle at %f:%f radius %f\n", x, y, radius);
   }
}

/** "Abstraction" */
interface Shape
{
   public void draw();                                            // low-level
   public void resizeByPercentage(double pct);     // high-level
}

/** "Refined Abstraction" */
class CircleShape implements Shape
{
   private double x, y, radius;
   private DrawingAPI drawingAPI;
   public CircleShape(double x, double y, double radius, DrawingAPI drawingAPI)
   {
       this.x = x;  this.y = y;  this.radius = radius;
       this.drawingAPI = drawingAPI;
   }

   // low-level i.e. Implementation specific
   public void draw()
   {
        drawingAPI.drawCircle(x, y, radius);
   }
   // high-level i.e. Abstraction specific
   public void resizeByPercentage(double pct)
   {
        radius *= pct;
   }
}

/** "Client" */
class BridgePattern {
   public static void main(String[] args)
   {
       Shape[] shapes = new Shape[2];
       shapes[0] = new CircleShape(1, 2, 3, new DrawingAPI1());
       shapes[1] = new CircleShape(5, 7, 11, new DrawingAPI2());

       for (Shape shape : shapes)
       {
           shape.resizeByPercentage(2.5);
           shape.draw();
       }
   }
}
```
2. go
```go
package main

import (
	"fmt"
)

// Implementor
type DrawingAPI interface {
	drawCircle(float32, float32, float32)
}

// ConcreteImplementor 1/2
type DrawingAPI1 struct{}

func (d *DrawingAPI1) drawCircle(x float32, y float32, radius float32) {
	fmt.Printf("API1.circle at %f:%f radius %f\n", x, y, radius)
}

// ConcreteImplementor 2/2
type DrawingAPI2 struct{}

func (d *DrawingAPI2) drawCircle(x float32, y float32, radius float32) {
	fmt.Printf("API2.circle at %f:%f radius %f\n", x, y, radius)
}

// Abstraction
type Shape interface {
	draw()
	resizeByPercentage(float32)
}

// Refined Abstraction
type CircleShape struct {
	x, y, radius float32
	drawingAPI   DrawingAPI
}

// Implementation specific
func (c *CircleShape) draw() {
	c.drawingAPI.drawCircle(c.x, c.y, c.radius)
}

// Abstraction specific
func (c *CircleShape) resizeByPercentage(pct float32) {
	c.radius *= pct
}

// Client
func main() {
	shape1 := &CircleShape{1, 2, 3, &DrawingAPI1{}}
	shape1.resizeByPercentage(2.5)
	shape1.draw()

	shape2 := &CircleShape{5, 7, 11, &DrawingAPI2{}}
	shape2.resizeByPercentage(2)
	shape2.draw()
}
```

## Proxy

> 프록시 패턴은 구조 패턴 중 하나이다.  
> 실제 객체 대신 대리자(가상) 객체를 통해 로직의 흐름을 제어하는 패턴이다.  
> 프록시 패턴은 Client(호출자), Interface(공통 인터페이스), RealObject(실제 구현되는 객체), Proxy(실제 객체에 대한 참조를 가진 가상객체)로 구성 되어있다.

1. 실제 객체를 대신해서 프록시가 로직의 흐름을 제어하기에 부가적인 작업을 수행하기 좋다.
2. 실제 객체와 프록시나 같은 인터페이스를 구현하므로 사용하기 좋고, 큰 규모의 연산을 실제로 필요한 시점에 수행하기 좋다. 그러니까 로직 흐름 제어가 편리하다.
3. Proxy 는 실제 객체에 인터페이스 구현을 위임한다. 추가적인 로직 흐름을 기존 구조에서 따로 캡슐화 하기 위한 방법이다.
4. 프록시에는 원격 프록시, 가상 프록시, 보호 프록시, 방화벽 프록시, 스마트 레퍼런스 프록시, 캐싱 프록시, 동기화 프록시, 복잡도 숨김 프록시, 지연 복사 프록시 등이 있다.
5. 프록시는 흐름제어만 캡슐화 할뿐이다. 결과 값 조작이나 변경은 프록시가 담당하는 책임이 아니다.
6. 사실 디자인 패턴들은 그냥 SOLID원칙을 충실히 지킬뿐이다.

### 예제
1.Java
```java
import java.util.*;

interface Image {
    public void displayImage();
}

//on System A
class RealImage implements Image {
    private String filename;
    public RealImage(String filename) {
        this.filename = filename;
        loadImageFromDisk();
    }

    private void loadImageFromDisk() {
        System.out.println("Loading   " + filename);
    }

    @Override
    public void displayImage() {
        System.out.println("Displaying " + filename);
    }
}

//on System B
class ProxyImage implements Image {
    private String filename;
    private Image image;

    public ProxyImage(String filename) {
        this.filename = filename;
    }

    @Override
    public void displayImage() {
        if (image == null)
           image = new RealImage(filename);

        image.displayImage();
    }
}

class ProxyExample {
    public static void main(String[] args) {
        Image image1 = new ProxyImage("HiRes_10MB_Photo1");
        Image image2 = new ProxyImage("HiRes_10MB_Photo2");

        image1.displayImage(); // loading necessary
        image2.displayImage(); // loading necessary
    }
}
```
2. C#
```c#
using System;

namespace Proxy
{
    class Program
    {
        interface IImage
        {
            void Display();
        }

        class RealImage : IImage
        {
            public RealImage(string fileName)
            {
                FileName = fileName;
                LoadFromFile();
            }

            private void LoadFromFile()
            {
                Console.WriteLine("Loading " + FileName);
            }

            public String FileName { get; private set; }

            public void Display()
            {
                Console.WriteLine("Displaying " + FileName);
            }
        }

        class ProxyImage : IImage
        {
            public ProxyImage(string fileName)
            {
                FileName = fileName;
            }

            public String FileName { get; private set; }

            private IImage image;

            public void Display()
            {
                if (image == null)
                    image = new RealImage(FileName);
                image.Display();
            }
        }

        static void Main(string[] args)
        {
            IImage image = new ProxyImage("HiRes_Image");
            for (int i = 0; i < 10; i++)
                image.Display();
        }
    }
}
```