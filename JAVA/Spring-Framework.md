Udemy-Spring Boot3 & Spring Framework 6 마스터하기!

# Spring Framework 기능

## @PostConstruct 및 @PreDestroy
1. 생성자 호출
2. 의존성 주입
3. @PostConstruct 실행
4. @PreDestroy 실행
5. Bean 삭제
```java
@Component
public class DBConnect {
    public DBConnect() {
        System.out.println("생성자 호출");
    }

    @PostConstruct
    public void initialize() {
        System.out.println("Bean 초기화 콜백");
    }

    @PreDestroy
    public void close() {
        System.out.println("Bean 삭제 전 콜백");
    }
}
```