# DesignPattern

- [Singleton](#singleton)
- [Prototype](#prototype)
- [Builder](#builder)
- [Factory Method](#factory-method)

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
        return this;
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