# Observer Pattern

<p align="center">
    <img width="300" alt="Design Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/8a78460d-5643-4dac-a84d-c1d92d5fa44f">
    <p align="center"><I>Design Pattern</I></p>
</p>

## 개념

<p align="center">
    <img width="250" alt="Observer Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/46f25588-6836-413d-afc3-2df768bfecf0">
    <p align="center"><I>Observer Pattern</I></p>
</p>

- 객체에 변화가 일어났을 때, 등록된 다른 객체들에게 통보를 해주는 패턴을 말함
- 대표적인 예로, Event listener가 있음

## 코드

예제로는 button을 객체를 만들어 클릭 이벤트를 리스닝하는 내용을 코드로 구현해봄.

### 리스닝 인터페이스와 버튼 클래스
```java
// 이벤트를 리스닝 할 인터페이스
public interface IButtonListener {
    void clickEvent(String event);
}

// 버튼 객체 구현
public class Button {
    private String name;

    // 리스닝 할 변수 할당
    private IButtonListener buttonListener;

    public Button(String name) {
        this.name = name;
    }

    // 해당 리스닝 인스턴스는 main에서 받아옴 (익명 함수)
    public void addListener(IButtonListener buttonListener) {
        this.buttonListener = buttonListener;
    }

    // main 에서는 clickEvent 함수 실행 시 message를 출력하는 함수를 구현
    public void click(String message) {
        buttonListener.clickEvent(message);
    }
}

```

<br />

### Main Class
```java
public class Main {
    public static void main(String[] args) {
        Button button = new Button("button");

        // 익명함수로 구현
        button.addListener(new IButtonListener() {
            @Override
            public void clickEvent(String event) {
                System.out.println(event);
            }
        });

        button.click("click!");
        button.click("click!");
        button.click("click!");
    }
}

// 출력값
click!
click!
click!
```