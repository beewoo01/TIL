# Composing Suspending Functions

```kotlin
fun main() = runBlocking<Unit> {
  val time = measureTimeMillis {
    val one = doSomethingUsefulOne()
    val two = doSomethingUsefulTwo()
    println("The answer is ${one + two}")
  }
  println("Completed is $time ms")
}

suspend fun doSomethingUsefulOne() : Int {
  delay(1000L)
  return 13
}

suspend fun doSomethingUsefulTwo() : Int {
  delay(1000L)
  return 29
}
```

예제에서는 단순히 delay를 적용했지만 안드로이드로 개발을 한다는 가정이라면 서버호출 부분이 딱 적절하다.</br>
변수 one, two는 각 서버를 2번 호출해서 2개의 결과값을 합쳐 print하는것이다.</br>
코루틴은 일반 코드처럼 순차적으로 실행되게끔 해준다.</br>
callback을 순차적으로 실행할 수 있다.</br>
