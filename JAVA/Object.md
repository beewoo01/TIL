# 12장 모든 자바 클래스의 부모인 java.lang.Object 클래스

## Object 클래스

```java
public class inheritanceObject {
  public static void main(String[] args) throws Exception {
    InheritanceObject object = new InheritanceObject();
    ~~~
  }
}
```

위 클래스는 아무것도 확장하지 않는것을 알 수 있다.
`extends`가 없기 때문이다.
자바에서는 기본적으로 아무것도 상속받지 않으면 기본적으로 `Object`를 상속받게 되어있다.

왜 모든 클래스는 `Object`클래스를 상속받게 받을까?
`Object` 클래스에 있는 메소드들을 통해서 클래스의 기본적인 행동을 정의할 수 있기 때문이다.
`Object` 클래스는 클래스를 구현하기 위해 가장 기본적인 메소드를 정의해 놓은 클래스이다.

---


## Object 클래스에서 제공하는 메소드들의 종류
|메소드|설명|
|----|---|
|protected Object clone()|객체의 복사본을 만들어 리턴한다.|
|public boolean equals(Object obj)|현재 객체와 매개 변수로 넘겨받은 객체가 같은지 확인한다. 같으면 true를, 다르면 false를 리턴한다.|
|protected void finalize()|현재 객체가 더 이상 쓸모가 없을 때 가비지 컬럭터에 의해 이 메소드가 호출된다.|
|public class<?> getClass()|현재 객체의 Class 클래스의 객체를 리턴한다.
|public int hashCode()|객체에 대한 해시코드(hash code)값을 라턴한다. 해시 코드라는 것은 "16진수로 제공되는 객체의 메모리 주소"를 말한다.|
|public String toString()|객체를 문자열로 표현하는 값을 리턴한다.|
