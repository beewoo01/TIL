# Example2

```kotlin
// This file was automatically generated from composing-suspending-functions.md by Knit tool. Do not edit.
package kotlinx.coroutines.guide.exampleCompose02

import kotlinx.coroutines.*
import kotlin.system.*

fun main() = runBlocking<Unit> {
    val time = measureTimeMillis {
        val one = async { doSomethingUsefulOne() }
        val two = async { doSomethingUsefulTwo() }
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

- 1번, 2번 연산이 만약 독립적이라면?
- 더 빠르게 결과값을 얻고 싶다면 어떻게 하면 될까?


`await`는 결과가 나올때 까지 기다린다.

결과는 아래와 같다.
순차적으로 진행된다고 해도 실행 속도는 빠르다.
```text
The answer is 42
Completed in 1035 ms

Process finished with exit code 0

```


만약 1번이 끝난 다음 2번을 호출하고 싶으면 1번을 호출한 바로 직 후 `await`를 사용해서 기다리게 하면된다.
