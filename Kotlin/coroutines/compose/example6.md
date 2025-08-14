# Example 6

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking<Unit> {
    try {
        failedConcurrentSum()
    } catch(e: ArithmeticException) {
        println("Computation failed with ArithmeticException")
    }
}

suspend fun failedConcurrentSum(): Int = coroutineScope {
    val one = async<Int> { 
        try {
            delay(Long.MAX_VALUE) // Emulates very long computation
            42
        } finally {
            println("First child was cancelled")
        }
    }
    val two = async<Int> { 
        println("Second child throws an exception")
        throw ArithmeticException()
    }
    one.await() + two.await()
}
```

Structured Concurrency안에서 실행된 Async가 있다.</br>
Exception이 발생하면 취소가 전파가 되고 catch에 잡히게 된다.</br>

프로그램 실행중 졸료가 되면서 이 소스를 다 해제하여 정상적인 형태로 종료가 된다.</br>

```text
Second child throws an exception
First child was cancelled
Computation failed with ArithmeticException

Process finished with exit code 0
```
