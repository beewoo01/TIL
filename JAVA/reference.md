# 참조 자료형

## 자바에서 생성자는 왜 필요할까?
자바의 생성자는 자바 클래스의 객체(또는 인스턴스)를 생성하기 위해서 존재한다.
아래의 예시코드에 `ReferenceDefault reference = new ReferenceDefault();`라는 코드가 있다.
reference가 객체이다.


## 기본 생성자
- 자바는 생성자를 만들지 않아도 자동으로 만들어지는 기본 생성자가 있다.

> 예시코드
```java
public class ReferenceDefailt {
  public static void main(String[] args) {
    ReferenceDefault reference = new ReferenceDefault();
  }
}
```
> 예시코드에서 보면 `new` 옆에있는 `ReferenceDefault()`가 생성자이다.

- 실제 ReferenceDefailt class에서는 ReferenceDefault() 라는 코드는 작성하지 않았다.
- ReferenceDefault() 와 같은 기본 생성자는 다른생성자가 없을 경우 기본으로 컴파일할 때 만들어 진다.
- 다른 생성자가 있을 경우에는 기본 생성자는 컴파일 시 만들어 지지 않는다.
- 생성자는 메소드와 비슷하게 생겼지만, 리턴타입이 없고, 클래스 이름으로 되어있다.

> 만약 다른 생성자가 있는데 기본 생성자가 필요하다면?
```java
public class ReferenceDefailt {
  public ReferenceDefailt() {}
  public ReferenceDefailt(String arg) {}
  public static void main(String[] args) {
    ReferenceDefault reference = new ReferenceDefault();
  }
}
```
- 이렇게 `public ReferenceDefailt() {}` 직접 작성하면 된다.

  
* DTO (Data Transfer Object) : 데이터를 다른 서버로 전달하기 위한 목적
* VO (Value Object) : 데이터를 담아 두기 위한 목적



## 메소드 Overloading
- 클래스의 생성자 처럼 메서드도 이름은 같게 하고 매개 변수들은 다르게 하여 만들려고 한다면?
```java
public class ReferenceOverloading {
  public static void main(String args[]) {
    ReferenceOverloading reference = new ReferenceOverloading();
  }

  public void print(int data) {}
  public void print(String data) {}
  public void print(int intData, String stringData) {}
  public void print(String stringData, int intData) {}
}  
```
- 이렇게 메소드들의 이름이 모두 동일하고, 각 메소드의 매개 변수의 종류와 개수, 순서가 다르게 하면 Overloading이다.

- Overloading을 사용하는 이유는 개발시 편하려고가 가장 큰거 같다.
- System.out.println()의 경우 int, String, long등 여러 type을 받는다.
- 만약 타입마다 다르게 메소드 명을 다르게 작성해야 한다면 외울것도 많고 각 상황마다 다르게 작성해야 하므로 불편하다.
