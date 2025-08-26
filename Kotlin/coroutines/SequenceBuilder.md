코틀린의 시퀀스는 List나 Set과 같은 컬렉션이랑 비슷한 개념이지만, </br>
필요 할 때마다 값을 하나씩 계산하는 지연(lazy) 처리를 한다.</br>
</br>
> 시퀀스의 특징
> - 요구되는 연산을 최소한으로 수행한다.
> - 무한정이 될 수 있다
> - 메모리 사용이 효율적이다.

이러한 특징 때문에 값을 순차적으로 계산해서 필요할 때 반환하는 빌더를 정의하는것이 좋다.</br>
시퀀스는 sequence라는 함수를 이용해 정의한다.</br>
시퀀스의 람다 표현식 내부에서는 yield 함수를 호출해 sequence를 호출한다.</br>

```kotlin
val seq = sequence {
  yield(1)
  yield(2)
  yield(3)
}

fun main() {
  for (num int seq) {
    print(num)
  } // 123
}
```
여기서 중요한점은</br>
각 숫자가 미리 생성되는 대신, 필요할 때마다 생성된다.</br></br>

`sequence` Block안의 코드는 즉시 전부 실행되지 않는다.</br>
`for(num in seq)` 루프가 처음 돌기 시작하면, 블록이 실행되면서 `yield(1)`을 만나고 -> 1을 내보내고 잠깐 멈춘다.</br>
루프가 다음 값을 원할 때 블록이 다시 이어서 실행되고 -> `yield(2)` -> 2를 내보내고 또 멈춘다.</br>
또 마찬가지로 `yield(3)`을 내보내고 블록이 끝난다.</br>

그래서 `for(num in seq)`가 없으면, 사실상 아무 값도 생성되지 않는다.</br>
`yield(1)` `yield(2)` `yield(3)` 자체가 게으르게 실행된다.</br>

```kotlin
val seq = sequence {
  println("Generating first")
  yield(1)
  println("Generating second")
  yield(2)
  println("Generating third")
  yield(3)
  println("Done")
}

fun main() {
  for (num int seq) {
    print(num)
  } // 123
}
```
반복문과의 결정적 차이는 이전에 다른 숫자를 찾기위해 멈췄던 지점에서 다시 실행이 된다.<br/>
중단 체제가 없으면 함수가 중간에 멈췄다가, 나중에 중단된 지점에서 다시 실행되는 건 불가능 하다.<br/>
중단이 가능하기 때문에 main 함수와 시퀀스 제너레이터가 번갈아 가면서 실행된다.<br/>
<br/><br/>
시퀀스 빌더느 반환(`yield`)이 아닌 중단 함수를 사용하면 안된다.</br>
중단이 필요하다면 데이터를 가져오기 위해 플로우(`Flow`)를 사용하는 것이 낫다.</br>


