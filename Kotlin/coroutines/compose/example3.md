# Example3

```kotlin
import kotlinx.coroutines.*
import kotlin.system.*

fun main() = runBlocking<Unit> {
    val time = measureTimeMillis {
        val one = async(start = CoroutineStart.LAZY) { doSomethingUsefulOne() }
        val two = async(start = CoroutineStart.LAZY) { doSomethingUsefulTwo() }
        // some computation
        one.start() // start the first one
        two.start() // start the second one
        println("The answer is ${one.await() + two.await()}")
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

`async`매게변수에 `CoroutineStart.LAZY`를 넣으면 해당 코루틴은 바로 실행되지 않는다.
실행 시키는 방법음 `start()`를 해주거나 `await()`를 해줘야한다.

```text
The answer is 42
Completed in 1053 ms

Process finished with exit code 0
```

Lazy로 했지만 1초가 걸렸다.
start를 해주지 않으면?

2초 정도가 나온다.
왜냐하면 `one.await()`를 하게 되면 one이 끝날때 까지 기다렸다가, `two.await()`를 실행하게 된다.
