# 중단은 어떻게 작동할까?

중단 함수는 코튼린 코루틴의 핵심이다.</br>
코루틴이 중단되었을 때 `Continuation` 객체를 반환한다.</br>
`Continuation`을 이용하면 멈췄던 곳에서 다시 코루틴을 실행할 수 있다.</br></br>

스레드와 비교하면</br>
스레드는 멈추는 것은 가능하지만 저장이 불가능 하다.</br></br>

`Continuation`객체는 직렬화와 역직렬화가 가능하며 다시 실행될 수 있다.</br>
--- 
</br>
## 재개

코루틴은 `runBlocking`나 `launch`와 같은 코루틴 빌더를 통해 만들 수 있다.</br></br>
중단함수 : 코루틴을 중단할 수 있는 함수.</br>
중단함수는 반드시 코루틴에 의해 호출되어야 한다.</br>
중단함수는 반드시 중단할 수 있는 곳이 필요하다.</br>

```kotlin
suspend fun main() {
  println("Before")
  suspendCoroutine<Unit> { }
  println("After")
}
```

위 코드의 동작은 `Before`를 실행 한 후 멈추게 된다.</br>
`suspendCoroutine<Unit> { }`에는 위에서 언급했던 `Continuation` 객체를 인자로 받을 수 있다.</br>
`suspendCoroutine<Unit> { }`은 코루틴을 멈추게 하는데 `{ }`가 끝나면서 멈추게 된다.
</br>

```kotlin
suspend fun main() {
  println("Before")

  suspendCoroutine<Unit> { continuation ->
    println("Before too")
  }

  println("After")
}
```
</br>
위 코드가 끝나면 Before, Before too가 출력된 후 코루틴은 멈추게 된다.</br>
코루틴을 재개하려면 `continuation`을 이용하면 된다. `continuation.resume(Unit)` 이 코드를 사용하면 재개 된다.</br>
</br>

```kotlin
fun continueAfterSecond(continuation: Continuation<Unit>) {
  thread {
    Thread.sleep(1000)
    continuation.resume(Unit)
  }
}

suspend fun main() {
  println("Before")

  suspendCoroutine<Unit> { continuation ->
    continueAfterSecond(continiation)
  }

  println("After")
}
```
위 코드의 결과는 Before -> (1초 후) After </br>
위 코드는 좋은 코드는 아니다 왜냐하면 스레드를 만드는 비용은 많은데</br>
1초 후에 사라지는 스레드를 사용하는 것은 불필요하다.</br>
더 좋은 코드라고 책에는 나와 있지만 굳이 여기서 적을 필요는 없을거 같아서 적지 않겠다.</br>
</br>

## 값으로 재개하기
`val ret: Unit = suspendCoroutine<Unit> { cont: Continuation<Unit> -> cont.resume(Unit) }`
</br>

`Unit`은 함수의 리턴 타입이며, `Continuation`의 제네릭 타입 인자다.</br>
`suspendCorountine`을 호출할 때 컨티뉴에이션 객체로 반활 될 값의 타임을 지정 할 수 있다.</br>
`resume`을 통해 반환되는 값은 반드시 지정된 타입과 같은 타입이어야 한다.</br>

```kotlin
suspend fum main() {
  val i: Int = suspendCorountine<Int> { cont ->
    cont.resume(42)
  }
  println(i) //42

  val str: String = suspendCoroutine<String> { cont ->
    cont.resume("Some text")
  }
  println(str) //Some Text

  val b: Boolean = suspendCoroutine<Boolean> { cont ->
    cont.resume(true)
  }
  println(b) // true
}
```

