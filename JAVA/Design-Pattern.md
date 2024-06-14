JAVA 언어로 배우는 디자인 패턴 입문
# 목차
- [목차](#목차)
- [Iterager](#iterager)
    - [요소](#요소)
    - [예제](#예제)
    - [정리](#정리)
- [Adapter](#adapter)
    - [요소](#요소-1)
    - [예제](#예제-1)
    - [정리](#정리-1)
- [Template Method](#template-method)
    - [요소](#요소-2)
    - [예제](#예제-2)
    - [정리](#정리-2)
- [Factory Method](#factory-method)
    - [요소](#요소-3)
    - [예제](#예제-3)
    - [정리](#정리-3)
- [Singleton](#singleton)
    - [요소](#요소-4)
    - [예제](#예제-4)
    - [정리](#정리-4)

<br/>

# Iterager
컬렉션의 구현 방법을 노출하지 않으면서 집합체 내부 모든 요소에 접근하기 위한 패턴

## 요소
* Iterator (반복자)
    * 요소에 접근하는 인터페이스(API) 정의
* Concretelterator (구체적인 반복자)
    * Iterator에서 정의한 API 구현
* Aggregate (집합체)
    * Iterator를 만들어 내는 인터페이스(API) 정의
* ConcreteAggregate (구체적인 집합체)
    * Aggregate에서 정의한 API 구현
    * ConcreteIterator의 인스턴스 생성

## 예제
```java
public interface Iterator {
    public boolean hasNext();
    public Object next();
}

public class ConcreteIterator implements Iterator {
    private Object[] arr;
    private int idx;

    public ConcreteIterator(Object[] arr) {
        this.arr = arr;
        this.idx = 0;
    }

    @Override
    public boolean hasNext() {
        return idx < arr.length;
    }

    @Override
    public Object next() {
        if (!hasNext()) {
            throw new NoSuchElementException();
        }
        return arr[idx++];
    }
}

public interface Aggregate {
    public abstract Iterator iterator();
}

public class ConcreteAggregate implements Aggregate {
    private Object[] arr;
    private int idx = 0;

    public ConcreteAggregate(int size) {
        this.arr = new Object[size];
    }

    public void append(Object obj) {
        this.arr[idx] = obj;
        idx++;
    }

    @Override
    public Iterator iterator() {
        return new ConcreteIterator(arr);
    }
}
```

## 정리
* Iterator를 사용함으로써 구현과 분리하여 반복할 수 있다.
    ```java
    ConcreteAggregate ca = new ConcreteAggregate();
    Iterator it = ca.iterator();

    while(it.hasNext()) {
        it.next();
    }
    ```
    ConcreteAggregate를 어떻게 변경하든 ConcreteAggregate가 iterator 메소드를 가지고 있고 올바른 Iterator를 반환하면 위의 while 루프는 변경하지 않아도 동작한다.

<br/>

# Adapter
기존 코드를 클라이언트에서 요구하는 용도로 사용할 수 있도록 다른 인터페이스로 변환해주는 패턴
* 클래스에 의한 Adapter 패턴(상속)
* 인스턴스에 의한 Adapter 패턴(위임)

## 요소
* Adaptee (적응 대상자)
    * 이미 준비된 메소드, adpater 적용 대상
* Targer (대상)
    * 현재 필요한 메소드 정의
* Adapter (적응자)
    * Target에서 정의한 메소드 구현
    * Adaptee의 메소드를 이용해 Client에서 요구하는 Target 구현
* Client (의뢰자)
    * Target에서 정의된 메소드 사용

## 예제
* 클래스에 의한 Adapter 패턴(상속)
```java
public class Adaptee {
    public void AdapteeService(String msg) {
        System.out.println("adaptee, " + msg);
    }
}

public interface Target {
    public void targetService(String msg);
}

public class Adapter extends Adaptee implements Target {
    @Override
    public void targetService(String msg) {
        AdapteeService("adapter, " + msg);
    }
}

public class Client {
    public static void main(String[] args) {
        Target t = new Adapter();
        t.targetService("Hello");
    }
}
```
```
adaptee, adapter, Hello
```

* 인스턴스에 의한 Adapter 패턴(위임)
```java
public class Adaptee {
    public void AdapteeService(String msg) {
        System.out.println("adaptee, " + msg);
    }
}

public abstract class Target {
    public abstract void targetService(String msg);
}

public class Adapter extends Target {
    private Adaptee adaptee;

    @Override
    public void targetService(String msg) {
        adaptee.AdapteeService("adapter, " + msg);
    }
}

public class Client {
    public static void main(String[] args) {
        Target t = new Adapter();
        t.targetService("Hello");
    }
}
```
```
adaptee, adapter, Hello
```

## 정리
* 기존 클래스에서 추가로 필요한 메소드가 있을 경우 빠르게 추가할 수 있다. 그리고 버그 발생 시 Adapter 역할의 클래스를 중점적으로 살펴보면 되므로 프로그램 검사가 편해진다.

<br/>

# Template Method
상위 클래스에서 공통적으로 사용할 메소드를 정의하고, 하위 클래스에서 구체적인 처리 방식을 구현하는 패턴

## 요소
* AbstractClass (추상 클래스)
    * 템플릿 메소드 구현, 템플릿 메소드에서 사용할 추상 메소드 선언
* ConcreteClass (구현 클래스)
    * AbstractClass에서 정의된 추상 메소드를 구체적으로 구현

## 예제
```java
public abstract class AbstractClass {
    public final void templateMethod() {
        abstractMethod1();
        abstractMethod2();
    }

    public abstract void abstractMethod1();
    public abstract void abstractMethod2();
}

public class ConcreteClass extends AbstractClass {
    @Override
    public void abstractMethod1() {
        System.out.println("abstractMethod1");
    }

    @Override
    public void abstractMethod2() {
        System.out.println("abstractMethod2");
    }
}
```
```java
public class Main {
   public static void main(String[] args) {
       AbstractClass template = new ConcreteClass();
       template.templateMethod();
   }
}
```
```
abstractMethod1
abstractMethod2
```

## 정리
* 로직을 공통화할 수 있다.
* 상위 클래스와 하위 클래스가 긴밀하게 연계하여 동작하므로 프로그램 설계 시 어떤 수준에서 처리를 나눌지 고려해야 한다.

<br/>

# Factory Method
상위 클래스에서 객체를 생성을 위한 인터페이스를 정의하여, 서브 클래스에서 구체적인 객체를 결정하여 구현할 수 있도록 하는 패턴

## 요소
* Product (제품)
    * 객체가 가져야할 인터페이스(API) 선언
* ConcreteProduct (구체적인 제품)
    * 템플릿 메소드 구현, 템플릿 메소드에서 사용할 추상 
* Creator (작성자)
    * AbstractClass에서 정의된 추상 메소드를 구체적으로 구현
* ConcreteCreator (구체적인 작성자)
    * AbstractClass에서 정의된 추상 메소드를 구체적으로 구현

## 예제
```java
// 제품 클래스
public abstract class Product {
    public abstract void test();
}

public class ConcreteProduct extends Product {
    private String subject;

    ConcreteProduct(String subject) {
        this.subject = subject;
        System.out.println(subject + " 테스트 준비 완료");
    }

    @Override
    public void test() {
        System.out.println(subject + " 테스트 수행");
    }
}

// 공장 클래스
public abstract class Factory {
    public final Product create(String subject) {
        Product product = createProduct(subject);
        return product;
    }

    protected abstract Product createProduct(String subject);
}

public class ConcreteCreator extends Factory {
    @Override
    protected Product createProduct(String subject) {
        return new ConcreteProduct(subject);
    }
}
```
```java
public class Main {
    public static void main(String[] args) {
        Factory factory = new ConcreteCreator();

        Product product1 = factory.create("A");
        Product product2 = factory.create("B");

        product1.test();
        product2.test();
    }
}
```
```
A 테스트 준비 완료
B 테스트 준비 완료
A 테스트 수행
B 테스트 수행
```

## 정리
* 공장 클래스는 제품 클래스에 의존하지 않으므로 유지보수에 용이하다.
* 시간의 흐름에 따라 코드 복잡성이 증가할 수 있으므로 설계, 개발 시 주석 및 문서 작성을 꼼꼼히 하는 것이 좋다.

<br/>

# Singleton
인스턴스를 하나만 생성하기 위한 패턴

## 요소
* Singleton
    * 유일한 인스턴스를 얻기 위한 static 메소드를 갖으며, 해당 메소드는 항상 같은 인스턴스를 return 한다.

## 예제
```java
public class Singleton {
    private static Singleton singleton = new Singleton();
    
    private Singleton() {
        System.out.println("인스턴스 생성");
    }
    
    public static Singleton getInstance() {
        return singleton;
    }
}
```
```java
public class Main {
    public static void main(String args[]) {
      Singleton s1 = Singleton.getInstance();
      Singleton s2 = Singleton.getInstance();
      
      System.out.println(s1.toString());
      System.out.println(s2.toString());
    }
}
```
```
인스턴스 생성
Singleton@2a139a55
Singleton@2a139a55
```

## 정리
* 생성자 메소드에 private를 붙여 외부 클래스에서 new를 통해 인스턴스화하는 것을 제한한다.
* getInstance() 메소드를 호출할 때 생성자가 초기화되며 유일한 인스턴스가 생성된다.
* eunm의 요소는 상수로서 인스턴스의 유일성을 보증받기 때문에 enum을 통해 Singleton을 구현할 수 있다.
    ```java
    enum Singleton {
        INSTANCE;
    
        public String test() {
            return "test";
        }
    }
    ```
    ```java
    public class Main {
        public static void main(String args[]) {
            System.out.println(Singleton.INSTANCE);
            System.out.println(Singleton.INSTANCE.test());
        }
    }
    ```
    ```
    INSTANCE
    test
    ```