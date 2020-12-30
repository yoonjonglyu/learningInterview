# DesignPattern

- [Singleton](#singleton)

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