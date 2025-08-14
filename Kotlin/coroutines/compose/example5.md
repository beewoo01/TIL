# Example 5

```kotlin
import kotlinx.coroutines.*
import kotlin.system.*

fun main() = runBlocking<Unit> {
    val time = measureTimeMillis {
        println("The answer is ${concurrentSum()}")
    }
    println("Completed in $time ms")
}

suspend fun concurrentSum(): Int = coroutineScope {
    val one = async { doSomethingUsefulOne() }
    val two = async { doSomethingUsefulTwo() }
    one.await() + two.await()
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
이런식으로 사용해야 한다고 보여주는 예제다.


```text
The answer is 42
Completed in 1038 ms

Process finished with exit code 0
```

아래와 같이 예제를 변경해서 안전하다는것을 확인해 보자.
Exception이 발생하면 coroutine이 멈춘다.

결론은
GlobalScope에서 launch한 형태로 함수를 감싸서 만들지 말고,
이런 형태로 structured concurrency하게 suspending function을 조합하고 구성하는게 맞다라는 메시지를 준다.

```kotlin
fun main() = runBlocking<Unit> {
    try {
        val time = measureTimeMillis {
            println("The answer is ${concurrentSum()}")
        }
        println("Completed in $time ms")
    } catch (e : Exception) {
    }

}

suspend fun concurrentSum(): Int = coroutineScope {
    val one = async { doSomethingUsefulOne() }
    val two = async { doSomethingUsefulTwo() }

    delay(10)
    kotlin.io.println("Exception")
    throw Exception()
    one.await() + two.await()
}

suspend fun doSomethingUsefulOne(): Int {
    println("start, somethingUsefulOneAsync")
    delay(3000L) // pretend we are doing something useful here
    println("end, somethingUsefulOneAsync")
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    println("start, somethingUsefulTwoAsync")
    delay(3000L) // pretend we are doing something useful here, too
    println("end, somethingUsefulTwoAsync")
    return 29
}
```
