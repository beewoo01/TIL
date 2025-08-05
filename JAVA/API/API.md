# API (Application Programming Interface)

### API란 무었일까?
> 애플리케이션에 선언되어 있는 클래스와 메소드에 대한 상세한 설명이 포함된 문서

- 

### Deprecated 란?
> 더 이상 사용을 권장하지 않는 클래스, 메서드, 필드 등을 나타내며ㅡ 향후 제거될 수 있음을 의미
> 한 마디로 "이제 이건 안 쓰는 거야"라고 선언한 것이다.
> <br/>deprecated의 사전적 의미는 "(강력히) 반대하다."라는 말이다.
> <br/>Deprecated는 생성자, 상수 필드, 메소드에 선언되어 있다.

JDK를 만들었을 때에는 필요해서 만들었지만, 나중에 사용하다 보니
문제를 야기하고, 혼동을 가져와서, 혹은 더 이상 가치가 없어졌을 때 Deprecated로 처리되어 버린다.

그렇다면 그냥 삭제해버리면 되는데 왜 Deprecated로 놔두는건가?
<br/>바로 **호환성** 때문이다.

이미 개발이 된 프로젝트에서 많은 비중으로 사용하는 코드가
자바 버전이 올라가면서 해당 API를 삭제해버린다면 컴파일 할 때 많은 부분에서 에러가 발생할 것이다.
그렇다면 자연스럽게 해당 코드들을 새로 변경해야하는데. 
이렇게 되면 많은 비용이 들며, 안좋게는 Java버전을 업그레이드를 포기해 버릴 수 있다.

---


## 직접해봅시다.
Q1: BigDemical 클래스의 용도가 무엇인지 확인해 보자.
> A1: 기본자료형에서 float이나 double이 가지는 부동소수점 오차 문제를 해결하고, 정밀한 소수 연산이 필요한 경우 사용한다.
> `BigDemical`은 immutable(불변 객체) 이며, 정수 부분과 소수 자릿수(스케일)를 분리하여 저장한다.

Q2: BigDemical 클래스의 생성자가 몇 개인지 확인해 보자.
> A2: 16개이다.

Q3: BigDemical 클래스에 있는 메소드 중 abs() 메소드의 용도는 무엇인가?
> A3: 절대값이다.
> 값이 음수면 양수로 바꾸고, 양수는 그대로 반환한다.

Q4: BigDemical 클래스에 있는 메소드 중 현재 값에서 특정 값을 빼는 연산을 수행하는 메소드는 어떤것인가?
> A4: subtract(BigDecimal subtrahend)

Q5: BigDemical 클래스에 있는 메소드 중 나누기와 관련된 메소드에는 어떤 것들이 있는지 확인해 보자.
> A5: 
```textarea
1. divide(BigDecimal divisor)
- 기능 : 정확한 나눗셈 결과를 반환
- 조건 : 결과가 종료 소수여야 함.
- 예외 : 결과가 무한소수면 `ArithmeticException` 발생
- 주의 : 정밀하지 않은 경우 예외가 발생하므로 대부분의 경우 `RoundingMode`를 사용하는 메소드 사용을 권장.


2. divide(BigDecimal divisor, RoundingMode roundingMode)
- 기능 : 나눗셈을 반올림해서 결과를 반환
- 장점 : 무산소수도 `RoundingMode`를 시용해 안전하게 처리 가능
- 예외 :
 1) divider == 0 : ArithmeticException
 2) roundingMode == UNNECESSARY인데 결과가 정확하지 않으면 : ArithmeticException
- 일반적인 나눗셈에서 가장 자주 사용됨.


3. divide(BigDecimal divisor, int scale, RoundingMode roundingMode)
- 기능 : 나눗셈 결과를 `scale` 자릿수까지 계산하고, `RoundingMode`에 따라 반올림.
- 예외 :
 1) divisor == 0 : ArithmeticException
 2) UNNECESSARY인데 결과가 정확하지 않으면 : ArithmeticException

4. divide(BigDecimal divisor, int scale, int roundingMode)
- 기능 : 3과 같지만 RoundingMode를 int 상수로 사용.
- 현재는 레거시 방식이고, RoundingMode enum을 사용하는 3방식 권장.


5. divide(BigDecimal divisor, MathContext mc)
- 기능 : MathContext에 정의된 precision(정밀도) 및 roundingMode에 따라 나눗셈 수행
- MathContext는 소수점 이하 몇 자리까지 계산할지 와 어떻게 반올림할지 를 정의
- 예외 : 결과가 정확해야 하는데 정밀도 안 맞으면 : ArithmeticException


6. divideToIntegralValue(BigDecimal divisor)
- 기능 : 나눗셈의 정수 부분만 반환(소수점 이하 버림)
- 예외 : divider == 0 이면 ArithmeticException


7. divideToIntegralValue(BigDecimal divisor, MathContext mc)
- 기능 : 6과 동일하지만, MathContext로 정밀도 제한을 둘 수 있음.
- 예외 : 나눈 값의 정수부가 mc.precision보다 크면 ArithmeticException
```
