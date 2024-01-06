# Proxy Pattern

<p align="center">
    <img width="300" alt="Design Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/8a78460d-5643-4dac-a84d-c1d92d5fa44f">
    <p align="center"><I>Design Pattern</I></p>
</p>

## 개념

<p align="center">
    <img width="450" alt="Proxy Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/8a57763b-5a33-4897-9b88-7783785c3dcb">
    <p align="center"><I>Proxy Pattern</I></p>
</p>

- Proxy 라는 말이 대리자, 대리인인 것처럼 해당 패턴도 클라이언트의 처리를 대신해주는 것을 말함
- 단, 결과값 자체를 조작하는 것은 안됨
- 해당 패턴을 통해 cache 기능도 사용이 가능함

## 코드

위에 설명처럼 Proxy Pattern을 통해 cache 기능도 활용할 수 있다. 예제로는 url을 입력받는 HTML 클래스를 만들어 Proxy에서는 해당 HTML에 url이 이미 있으면 따로 new를 통한 인스턴스 생성없이 넘어가는 Proxy 클래스를 구현해본다.

### Html Class
```java
// URL을 입력받은 후 가지고 있음
public class Html {
    private String url;

    public Html(String url) {
        this.url = url;
    }
}
```

<br />

### IBrowser Interface
```java
public interface IBrowser {
    public Html show();
}
```

### BrowserProxy Class

```java
public class BrowserProxy implements IBrowser {
    private String url;
    private Html html;

    public BrowserProxy(String url) {
        this.url = url;
    }

    // show() 함수가 수행되면 html 인스턴스가 있는지 확인하고
    // 없다면 새로 생성, 있다면 그냥 넘김
    @Override
    public Html show() {
        if (html == null) {
            this.html = new Html(url);
            System.out.println("생성");
        } else {
            System.out.println("cache 기능 적용");
        }

        return html;
    }
}
```

### Main Class
```java
public class Main {
    public static void main(String[] args) {
        IBrowser browser = new BrowserProxy("naver.com");
        browser.show(); // 첫 실행 시 '생성' 출력
        browser.show(); // 'cache 기능 적용' 출력
    }
}
```