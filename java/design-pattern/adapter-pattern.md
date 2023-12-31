# Adapter Pattern

<p align="center">
    <img width="300" alt="Design Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/8a78460d-5643-4dac-a84d-c1d92d5fa44f">
    <p align="center"><I>Design Pattern</I></p>
</p>

## 개념

<p align="center">
    <img width="250" alt="Adapter Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/7b62df1b-dd91-40db-afc9-f6a72d3016f0">
    <p align="center"><I>Adapter Pattern</I></p>
</p>

- 기존 코드를 재사용하기 위해 변환하는 기능을 가지는 패턴
- 예를 들면, 220V를 사용하는 우리나라에서 110V를 사용하는 외국에 어댑터를 가져가 사용하는데 이 때 사용하는 어댑터를 생각하면 됨

## 코드

간단한 예제를 위해 HDMI만 실행가능한 함수에 컨버터를 만들어 RGB 구현체를 실행하도록 구현

### Interface 구현
```java
// RGB interface
public interface IRGB {
    public void connect();
}

// HDMI interface
public interface IHDMI {
    public void connect();
}
```

<br />

```java
// RGB 구현체
public class RGB implements IRGB {
    @Override
    public void connect() {
        System.out.println("RGB connected...");
    }
}

// HDMI 구현체
public class HDMI implements IHDMI {
    @Override
    public void connect() {
        System.out.println("HDMI connected...");
    }
}

// RGB를 받아 실행하는 컨버터 구현체
public class Converter implements IHDMI {

    private RGB rgb;

    public Converter(RGB rgb) {
        this.rgb = rgb;
    }

    @Override
    public void connect() {
        rgb.connect();
    }
}

```

<br />

### Main class

```java
public class Main {
    // hdmi를 인자로 받아 connect 하는 함수
    public static void connect(Converter hdmi) {
        hdmi.connect();
    }

    // converter를 통해 rgb를 connect 함수를 통해 수행
    public static void main(String[] args) {
        RGB rgb = new RGB();
        Converter converter = new Converter(rgb);
        connect(converter);
    }
}

```