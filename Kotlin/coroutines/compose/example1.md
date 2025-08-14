#Example compose 01

```kotlin
fun main() = runBlocking {
    val time = measureTimeMillis {
        val one = doSomethingUsefulOne()
        val two = doSomethingUsefulTwo()
        println("The answer is ${one + two}")
    }
    println("Completed in $time ms")
}

suspend fun doSomethingUsefulOne(): Int {
    delay(1000L) // pretend we are doing something useful here
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    delay(1000L) // pretend we are doing something useful here, too
    return 29
}
```
나는 `suspen func`은 비동기 통신을 하는 함수라고 알고 있었다.<br/>
하지만 그게 아니라 일시정지 가능한 함수라는 뜻이다.<br/>

위 예제는 코루틴에서 일반코드처럼 작성하면 비동기 일지라도 순차적으로 실행되는것이 기본이다. 라는 내용의 예제이다.

결과는

```text
The answer is 42
Completed in 2040 ms

Process finished with exit code 0
```
