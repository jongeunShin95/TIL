# Singleton Pattern

<p align="center">
    <img width="300" alt="Singleton Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/8a78460d-5643-4dac-a84d-c1d92d5fa44f">
    <p align="center"><I>Design Pattern</I></p>
</p>

## 개념

<p align="center">
    <img width="250" alt="Singleton Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/bf4464df-520f-4f13-bb9d-3c4957c7cfcd">
    <p align="center"><I>Singleton Pattern</I></p>
</p>

- 어떠한 클래스(객체)가 유일하게 1개만 존재하며, 생성된 객체를 어디에서든 참조할 수 있도록 하는 패턴
- 한번의 생성 후 계속 사용되기에 메모리 낭비 방지가 가능
- 자원 공유가 가능

## 코드

간단한 예제를 위해 Singleton class 하나를 만든 후 Main class 에서 getInstance() 를 통해 2개 변수에 담아 생성된 2개가 같은지 확인하는 코드를 작성

### Singleton Class
```java
public class Singleton {

    private static Singleton singleton = null;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (singleton == null) singleton = new Singleton();
        return singleton;
    }
}
```

<br />

### Main Class
```java
public class Main {
    public static void main(String[] args) {
        Singleton singleton1 = Singleton.getInstance();
        Singleton singleton2 = Singleton.getInstance();

        // 할당받은 2개의 인스턴스가 같은지 확인
        System.out.println(singleton1.equals(singleton2)); // true
    }
}
```