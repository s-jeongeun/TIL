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
// Iterator
public interface Iterator {
    public boolean hasNext();
    public Object next();
}

// Aggregate
public interface Aggregate {
    public abstract Iterator iterator();
}

// ConcreteAggregate
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

// ConcreteIterator
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
```

## 정리
* Iterator를 사용함으로써 구현과 분리하여 반복할 수 있다.
    ```java
    BookShelf bookShelf = new BookShelf();
    Iterator<Book> it = bookShelf.iterator();

    while(it.hasNext()) {
        System.out.println("");
    }
    ```
    BookShelf를 어떻게 변경하든 BookShelf가 iterator 메소드를 가지고 있고 올바른 Iterator<Book>을 반환하면 위의 while 루프는 변경하지 않아도 동작한다.


# Apapter
기존 코드를 클라이언트에서 요구하는 용도로 사용할 수 있도록 다른 인터페이스로 변환해주는 패턴
* 클래스에 의한 Adapter 패턴(상속)
* 인스턴스에 의한 Adapter 패턴(위임)

## 요소
* Targer (대상)
    * 현재 필요한 메소드 정의
* Client (의뢰자)
    * Target에서 정의된 메소드 사용
* Adaptee (적응 대상자)
    * 이미 준비된 메소드, adpater 적용 대상
* Adapter (적응자)
    * Target에서 정의한 메소드 구현
    * Adaptee의 메소드를 이용해 Client에서 요구하는 Target 구현

## 예제
* 클래스에 의한 Adapter 패턴(상속)
```java
// Target
public interface Target {
    public void targetService(String string);
}

// Client
public class Client {
    public static void main(String[] args) {
        Target t = new Adapter();
        t.targetService("Hello");
    }
}

// Adaptee
public class Adaptee {
    public void AdapteeService(String string) {
        System.out.println(string)
    }
}

// Adapter
public class Adapter extends Adaptee implements Target {
    @Override
    public void targetService(String string) {
        AdapteeService(string)
    }
}
```

* 인스턴스에 의한 Adapter 패턴(위임)
```java
// Target
public interface Target {
    public void targetService(String string);
}

// Client
public class Client {
    public static void main(String[] args) {
        Target t = new Adapter();
        t.targetService("Hello");
    }
}

// Adaptee
public class Adaptee {
    public void AdapteeService(String string) {
        System.out.println(string)
    }
}

// Adapter
public class Adapter extends Target {
    private Adaptee adaptee;

    @Override
    public void targetService(String string) {
        adpatee.AdapteeService(string)
    }
}
```

## 정리
* 기존 클래스에서 추가로 필요한 메소드가 있을 경우 빠르게 추가할 수 있다. 그리고 버그 발생 시 Adapter 역할의 클래스를 중점적으로 살펴보면 되므로 프로그램 검사가 편해진다.


# Template Method

## 요소

## 예제
```java

```

## 정리


# Factory Method

## 요소

## 예제
```java

```

## 정리


# Singleton

## 요소

## 예제
```java

```

## 정리