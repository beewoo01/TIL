## 자바에서 매우 중요한 예외

이번에는 예외처리 챕터이다.
항상 그랬듯이 이번 챕터에도 내가 몰랐던 상세한 부분들이 이 책에는 디테일하게 작성되어 있어서
깨닫는 점과 얻어가는 내용이 있다.
</br></br>

## try-catch는 짝이다.

```java
int[] intArray = new int[5];
System.out.println(intArray[5]);
System.out.println("Hello Exception!");
```
</br></br></br>

위 코드처럼 입력하면 `java.lang.ArrayIndexOutOfBoundesException : 5` 에러가 나타난다!
'배열의 범위 밖에 있는 위치를 요청한 예외'라는 의미다.
intArray 의 길이는 5이다. '0, 1, 2, 3, 4'
여기서 5를 가져오려고 하면 범위가 벗어나가 때문에 에러가 나는것이다.
이런 에러를 만나게 되면 해당 쓰레드는 JVM 시스템에 의해 더 이상 명령어를 수행하지 않고 종료시켜 버린다.
실행해보면 `Hello Exception!`의 문구를 확인 할 수 없다.
이것을 정상적으로 처리하기 위해서는
try, catch를 사용하면 된다.
그렇다고 모든 코드를 try에 묶어줄 필요는 없다.
예외처리가 필요한 부분에만 작성해주면 된다.

</br></br></br>

```java
int[] intArray = new int[5];
try {
  System.out.println(intArray[5]);
} catch(Exception e) {
  System.err.println(e);
}
System.out.println("Hello Exception!");
```

이 코드를 실행하면 에러 메시지와 함께 `Hello Exception!`도 출력되는것을 확인 할 수 있다.


```java
int[] intArray = null;
try {
  intArray = new int[5];
  System.out.println(intArray[5]);
} catch(Exception e) {
  System.err.println(e);
  System.out.println(intArray.length);
}
System.out.println("Hello Exception!");
```

이렇게 코드를 변경하면 어떻게 될까? `intArray.length`가 올바르게 동작 할까?
try에서 에러가 발생했다고 하더라도 에러전에 수행했던 코드는 전부 올바르게 동작한다.
`intArray.length`의 결과는 5라는것을 확인 할 수 있다.

## finally야~ 넌 무슨 일이 생겨도 반드시 실행해야 해
try-catch구문에 추가로 붙을 수있는 블록이 있다. finally이다.
finally는 에러가 발생했던 아니던 상관없이 무조건 실행한다.
나는 아직까지 실무에서 finally를 사용한 적은 없다.
책에도 작성되어 있듯이 '이걸 어디다가 써?'라고 생각한다.
하지만 책에서는 'finally 블록은 코드의 중복을 피하기 위해서 반드시 필요하다' 라고 말하며
26장 "파일에 있는거 읽고 쓰려면 아이오를 알라야죠"에서 다룬다고 한다.
</br></br>

## 두 개 이상의 catch
catch 블록을 시작되기 전에 소괄호에는 예외의 종류를 명시한다. 항상 (Exception e)를 사용해야 하는것은 아니다.
나는 try-catch문을 사용할 때 catch에 Exception을 어떠한 
문제라도 발생한다면 다 잡아야 한다는 의미로 항상 (Exception e)를 사용해 왔었다.
</br></br>

```java
public void multiCatch() {
  int[] intArray = new int[5];
  try {
    System.out.println(intArray[5]);
  } catch(ArrayIndexOutOfBoundsException e) {
    System.out.println("ArrayIndexOutOfBoudsException occurred");
  } catch(Exception e) {
    System.out.println(intArray.length);
  }
}
```

</br></br>
catch문 에서의 순서도 중요하다.
if, else if와 동일하게 순차적으로 수행된다.
만약 위 catch문의 순서를 변경해서 실행한다면 
`exeption ArrayIndexOutOfBoundsException has already been caught` 에러가 발생한것을 볼 수 있다.
모든 객체의 부모 클래스는 `Object`이다.
모든 예외의 부모 클래스는 `java.lang.Exception`이다.
`ArrayIndexOutOfBoundsException`의 부모 클래스는 `Exception`이다.
이미 `Exception`에서 `ArrayIndexOutOfBoundsException`을 잡기 때문에 `ArrayIndexOutOfBoundsException`를 
따로 빼서 작업하면 자식 클래스인 `ArrayIndexOutOfBoundsException`에서 catch할 기회가 전혀 없다.
`Exception`을 먼저 catch 해버리면 위 에러가 발생한다.
`Exception`은 맨 마지막으로 빼주는것이 좋으며, 예상치도 못한 예외가 발생하면 제대로 처리가 되지 않기 때문에 
마지막에 예외들이 빠져 나가지 않게 `Exception` 작성을 해주는것이 좋다.


### 여기까지 저자가 정리한 내용
- try 다음에 오는 catch 블록은 1개 이상 올 수 있다.
- 먼저 선언한 catch 블록의 예외 클래스가 다음에 선언한 catch블록의 부모에 속하면, 자식에 속하는 catch 블록은 절대 실행될 일이 없으므로 컴파일이 되지 않는다.
- 하나의 try 블록에서 예외가 발생하면 그 예외와 관련이 있는 catch 블록을 찾아서 실행한다.
- catch블록 중 발생한 예외와 관련있는 블록이 없으면, 예외가 발생되면서 해당 쓰레드는 끝난다.
* 추가(내가 몰랐던 내용) : java.lang패키지에 선언되어 있는것은 import 할 필요가 없다.

</br></br>

## 예외의 종류는 세 가지다
- checked exception
- error
- runtime exception 혹은 unchecked exception


### error
자바 프로그램 밖에서 발생한 예외를 말한다.
서버의 디스크 고장, 메인보드 고장 등으로 자바 프로그램이 제대로 동작하지 못하는 경우다.
모바일 부분에서 자주 발생하는 것은 `OutOfMemoryError`이다.
exception과 구분하는 방법은 이름으로 구분이 가능하다.
오류의 이름이 Error로 끝나면 에러이고, Exception으로 끝나면 예외다.



