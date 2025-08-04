# 참조 자료형

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

  
