# Facade Pattern

<p align="center">
    <img width="300" alt="Design Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/8a78460d-5643-4dac-a84d-c1d92d5fa44f">
    <p align="center"><I>Design Pattern</I></p>
</p>

## 개념

<p align="center">
    <img width="250" alt="Facade Pattern" src="https://github.com/jongeunShin95/TIL/assets/20867824/3622580e-75e8-496a-8091-750009c51ac1">
    <p align="center"><I>Facade Pattern</I></p>
</p>

- 여러 복잡하게 얽힌 서브 인터페이스들의 기능을 하나로 모아 클라이언트에게 간단한 통합 인터페이스를 제공하는 패턴

## 코드

예제로는 ftp접속, 파일쓰기, 파일읽기 클래스들의 공통 기능을 하나로 모아 main에서는 각각 클래스를 생성하여 호출하는 것이 아닌 하나의 객체를 통해 사용할 수 있도록 하는 것을 구현

### 각 클래스 구현(FTP, WRITER, READER)
```java
// FTP
public class Ftp {
    private String host;
    private int port;
    private String path;

    public Ftp(String host, int port, String path) {
        this.host = host;
        this.port = port;
        this.path = path;
    }

    public void connect() {
        System.out.println("FTP Host: " + host + " Port: " + " 로 연결");
    }

    public void moveDirectory() {
        System.out.println("FTP path: " + path + " 로 이동");
    }

    public void disConnect() {
        System.out.println("FTP 연결을 종료합니다.");
    }
}

// Writer
public class Writer {

    private String fileName;

    public Writer(String fileName) {
        this.fileName = fileName;
    }

    public void fileConnect() {
        String msg = String.format("Writer %s 로 연결 합니다.", fileName);
        System.out.println(msg);
    }

    public void write() {
        String msg = String.format("Writer %s 로 파일쓰기를 합니다.", fileName);
        System.out.println(msg);
    }

    public void fileDisconnect() {
        String msg = String.format("Writer %s 로 연결 종료합니다.", fileName);
        System.out.println(msg);
    }
}

// Reader
public class Reader {
    private String fileName;

    public Reader(String fileName) {
        this.fileName = fileName;
    }

    public void fileConnect() {
        String msg = String.format("Reader %s 로 연결 합니다.", fileName);
        System.out.println(msg);
    }

    public void fileRead() {
        String msg = String.format("Reader %s 의 내용을 읽어 옵니다.", fileName);
        System.out.println(msg);
    }

    public void fileDisconnect() {
        String msg = String.format("Reader %s 로 연결 종료합니다.", fileName);
        System.out.println(msg);
    }
}

```

<br />

### SFTP Class
```java
// FTP, WRITER, READER 의 함수들을 묶어 SFTP 클래스를 만든다
public class SftpClient {
    private Ftp ftp;
    private Reader reader;
    private Writer writer;

    public SftpClient(Ftp ftp, Reader reader, Writer writer) {
        this.ftp = ftp;
        this.reader = reader;
        this.writer = writer;
    }

    public SftpClient(String host, int port, String path, String fileName) {
        this.ftp = new Ftp(host, port, path);
        this.reader = new Reader(fileName);
        this.writer = new Writer(fileName);
    }

    // connect 묶기
    public void connect() {
        ftp.connect();
        ftp.moveDirectory();
        writer.fileConnect();
        reader.fileConnect();
    }

    // disconnect 묶기
    public void disConnect() {
        writer.fileDisconnect();
        reader.fileDisconnect();
        ftp.disConnect();
    }

    public void read() {
        reader.fileRead();
    }

    public void write() {
        writer.write();
    }

}
```

### Main Class
```java
public class Main {
    public static void main(String[] args) {
        SftpClient sftpClient = new SftpClient("www.naver.com", 22, "/home/etc", "text.tmp");
        sftpClient.connect();

        sftpClient.write();

        sftpClient.read();

        sftpClient.disConnect();
    }
}

// 출력값
FTP Host: www.naver.com Port:  로 연결
FTP path: /home/etc 로 이동
Writer text.tmp 로 연결 합니다.
Reader text.tmp 로 연결 합니다.
Writer text.tmp 로 파일쓰기를 합니다.
Reader text.tmp 의 내용을 읽어 옵니다.
Writer text.tmp 로 연결 종료합니다.
Reader text.tmp 로 연결 종료합니다.
FTP 연결을 종료합니다.
```