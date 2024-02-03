# Strategy Pattern

<p align="center">
    <img width="300" alt="Design Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/8a78460d-5643-4dac-a84d-c1d92d5fa44f">
    <p align="center"><I>Design Pattern</I></p>
</p>

## 개념

<p align="center">
    <img width="450" alt="Strategy Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/2756b821-a8c2-4666-82e3-99d36639fe34">
    <p align="center"><I>Strategy Pattern</I></p>
</p>

- 알고리즘(전략)을 선택하여 프로그램의 동작을 동적으로 바꾸어줌

## 코드

간단한 예제를 위해 기능을 수행하는 알고리즘(전략) 객체인 Normal encoder, Base64 encoder를 만들고 이를 사용하는 컨택스트 Encoder를 만들며 알고리즘(전략) 객체를 컨택스트에 전달하는 클라이언트(Main)을 만들어 볼 것이다.

### 알고리즘(전략) 객체 구현
```java
// 전략 객체의 공동 인터페이스
public interface EncodingStrategy {
    String encode(String text);
}

// Base64 encoder 알고리즘(전략)
public class Base64Strategy implements EncodingStrategy {
    @Override
    public String encode(String text) {
        return Base64.getEncoder().encodeToString(text.getBytes());
    }
}

// Normal encoder 알고리즘(전략)
public class NormalStrategy implements EncodingStrategy {
    @Override
    public String encode(String text) {
        return text;
    }
}

```

<br />

### 컨택스트 구현
```java
// 알고리즘(전략) 객체를 주입받는 Encoder
public class Encoder {
    private EncodingStrategy encodingStrategy;

    public String getMessage(String message) {
        return this.encodingStrategy.encode(message);
    }

    public void setEncodingStrategy(EncodingStrategy encodingStrategy) {
        this.encodingStrategy = encodingStrategy;
    }
}

```

<br />

### Main class

```java
// 알고리즘(전략) 객체를 컨택스트에 주입하고 실행하는 클라이언트
public class Main {
        Encoder encoder = new Encoder();

        // base64
        EncodingStrategy base64 = new Base64Strategy();
        // normal
        EncodingStrategy normal = new NormalStrategy();

        String message = "hello java";

        encoder.setEncodingStrategy(base64);
        String base64Result = encoder.getMessage(message);
        System.out.println(base64Result);
        // aGVsbG8gamF2YQ==

        encoder.setEncodingStrategy(normal);
        String normalResult = encoder.getMessage(message);
        System.out.println(normalResult);
        // hello java
}

```