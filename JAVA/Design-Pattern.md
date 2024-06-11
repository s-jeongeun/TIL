JAVA 언어로 배우는 디자인 패턴 입문
# 목차
- [목차](#목차)
- [Iterager](#Iterager)
    - [요소](#요소)
    - [예제](#예제)
    - [정리](#정리)


# Iterager
* 컬렉션의 구현 방법을 노출하지 않으면서 집합체 내부 모든 요소에 접근하기 위한 패턴

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
public interface Iterator<E> {
    public abstract boolean hasNext();
}

// Aggregate
public interface Iterable<E> {
    public abstract Iterator<E> iterator();
}

// ConcreteAggregate
public class BookShelf implements Iterable<Book> {
    @Override
    public Iterator<Book> iterator() {
        return new BookShelfIterator(this);
    }
}

// ConcreteIterator
public class BookShelfIterator implements Iterator<Book> {
    @Override
    public boolean hasNext() {
        return true;
    }
}
```

## 정리
1. Iterator를 사용함으로써 구현과 분리하여 반복할 수 있다.
    ```java
    BookShelf bookShelf = new BookShelf();
    Iterator<Book> it = bookShelf.iterator();

    while(it.hasNext()) {
        System.out.println("");
    }
    ```
    BookShelf를 어떻게 변경하든 BookShelf가 iterator 메소드를 가지고 있고 올바른 Iterator<Book>을 반환하면 위의 while 루프는 변경하지 않아도 동작한다.