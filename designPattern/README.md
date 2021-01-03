# DesignPattern

- [Singleton](#singleton)
- [Prototype](#prototype)

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