# Decorator Pattern

<p align="center">
    <img width="300" alt="Design Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/8a78460d-5643-4dac-a84d-c1d92d5fa44f">
    <p align="center"><I>Design Pattern</I></p>
</p>

## 개념

<p align="center">
    <img width="250" alt="Singleton Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/4f8adca9-88a7-45a4-8c93-0fe39851078f">
    <p align="center"><I>Decorator Pattern</I></p>
</p>

<center>

|클래스명|역할|
|---|---|
|class|원본 클래스와 장식 클래스 합친 클래스|
|concreteClass|원본 클래스|
|decorator|원본에 장식할 추상화 클래스|
|concreteDecorator|원본에 장식할 구체적 클래스|

</center>

- 확장이 필요할 때 상속의 대안으로도 사용되며, 필요한 형태로 꾸밀 때 사용되는 패턴

## 코드

간단한 예제를 위해 기본 휴대폰 케이스가 있고 이를 장식한 A, B 케이스를 만들어 봄.

### 원본 클래스
```java
public interface IPhoneCase {
    int getPrice();
    void showPrice();
}

public class Case implements IPhoneCase {
    private int price;

    public Case(int price) {
        this.price = price;
    }

    @Override
    public int getPrice() {
        return this.price;
    }

    @Override
    public void showPrice() {
        System.out.println("Case: " + getPrice());
    }
}
```

<br />

### 장식을 위한 추상화 클래스
```java
public class CaseDecorator implements IPhoneCase {

    protected IPhoneCase basicCase;
    protected String caseName;
    protected int casePrice;

    public CaseDecorator(IPhoneCase basicCase, String caseName, int casePrice) {
        this.basicCase = basicCase;
        this.caseName = caseName;
        this.casePrice = casePrice;
    }

    @Override
    public int getPrice() {
        return basicCase.getPrice() + this.casePrice;
    }

    @Override
    public void showPrice() {
        System.out.println(this.caseName + ": " + getPrice());
    }
}
```

<br />

### 장식할 구체적 클래스
```java
// A 케이스의 경우 기본 케이스에 1만원 추가
public class Acase extends CaseDecorator {
    public Acase(IPhoneCase basicCase, String caseName) {
        super(basicCase, caseName, 10000);
    }
}

// B 케이스의 경우 기본 케이스에 2만원 추가
public class Bcase extends CaseDecorator {
    public Bcase(IPhoneCase basicCase, String caseName) {
        super(basicCase, caseName, 20000);
    }
}

```

### Main Class
```java
public class Main {
    public static void main(String[] args) {
        IPhoneCase basicCase = new Case(10000);
        basicCase.showPrice();

        IPhoneCase caseA = new Acase(basicCase, "Acase");
        caseA.showPrice();

        IPhoneCase caseB = new Bcase(basicCase, "Bcase");
        caseB.showPrice();
    }
}

// 출력값
Case: 10000
Acase: 20000
Bcase: 30000
```