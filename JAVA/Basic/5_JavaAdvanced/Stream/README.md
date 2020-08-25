# Stream

# IO(InputOutput) Stream
* Decorator Pattern으로 구현되어 있음

## IO Stream 구분
* IO 대상 기준 : Input Stream, Output Stream
* 자료의 종류 : Byte 단위 Stream, 문자 단위 Stream
* Stream 기능 : 기반 Stream, 보조 Stream

## Stream 종류
1. Byte 단위 Stream
    * **InputStream**
    * **OutputStream**

2. 문자 단위 Stream
    * **Reader**
    * **Writer**

## 표준 입출력
* System 클래스의 표준 입출력 멤버
```java
public class System{
    public static PrintStream out;
    public static InputStream in;
    public static PrintStream err;
}
```

## Byte 단위 Stream
* Byte 단위로 자료를 읽고 씀(동영상, 음악파일 등)
* Function(Input)
    * **int read()** : 입력 스트림으로부터 1byte 읽고 Byte 리턴, 읽을게 없다면 -1 Return
    * **int read(byte[] b)** : 읽은 Byte들을 배열 b에 저장하고, 실제로 읽은 Byte 수를 리턴
* Function(Output)
    * **void write(int b)** : b의 끝 1byte를 출력 버퍼에 보낸다.
    * **void wirte(byre[] b)** : 배열 b의 모든 Byte를 보낸다.
    * **void flush()** : 출력 버퍼를 비운다.
### File Stream
* **FileInputStream** : File이 없는 경우 예외 발생
* **FileOutputStream** : File이 없는 경우 파일 생성하여 출력
   
```java
public class InputOutputStream {
    public static void main(String[] args) {

        System.out.println("입력 ");
        try {
            int i;
            InputStreamReader isr = new InputStreamReader(System.in);
            while((i = isr.read()) != '끝') {
                System.out.print((char) i);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
} 
```    

## 문자 기반 Stream
* 2 Byte씩 처리해야 함

### File Stream
* **FileReader**
* **FileWriter**
```java
public class ReaderWriterStream {
    public static void main(String[] args) throws IOException {
        FileReader fis = new FileReader("src/reader.txt");
//        FileInputStream fis = new FileInputStream("src/reader.txt");
//        InputStreamReader isr = new InputStreamReader(fis);
        int i;
        while((i = fis.read())!= -1){
            System.out.println((char)i);
        }
        fis.close();
    }
}
```

## 보조 Stream
* 실제로 읽고 쓰는 Stream이 아닌 보조적인 기능을 추가하는 Stream
* FilterInputStream과 FileOutputStream이 보조 Stream의 상위 클래스
    * **pretected FilterInputStream(InputStream in)**
    * **public FilterOutputStream(OutputStream out)**

### 종류
* **InputStreamReader(InputStream is)** : Byte를 문자 기반으로 읽어들인다.
* **Buffered Stream** : 내부에 8192byte 배열을 가지고 있어, 읽거나 쓸 때 속도가 빠름.
    * **BufferedInputStream(InputStream is)**
    * **BufferedOutputStream(OutputStream os)**
    * **String readLine()** : Line Feed('\n')와 Carriage Return('\r')을 제거해주고 한 줄을 읽어 들인다.
    
* Data Stream : 자료가 저장된 상태 그대로 자료형을 유지하며 읽기, 쓰기 기능을 제공하는 Stream
    * **DataInputStream**
    * **DataOutputStream**

```java
public class SubStream{
    public static void main(String[] args){
        try(FileOutputStream fos = new FileOutputStream("data.txt");
                DataOutputStream dos = new DataOutputStream(fos);
                FileInputStream fis = new FileInputStream("data.txt");
                DataInputStream dis = new DataInputStream(fis)){

            dos.writeByte(100);
            dos.write(100);
            dos.writeChar('A');
            dos.writeUTF("안녕하세요");

            System.out.println(dis.readByte());
            System.out.println(dis.read());
            System.out.println(dis.readChar());
            System.out.println(dis.readUTF);

        }catch(Exception e){
            e.printStackTrace();            
        }
    }
}
```