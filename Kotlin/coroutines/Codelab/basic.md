## launch

### 동기코드

```kotlin
import kotlin.system.*
import kotlinx.coroutines.*

fun main() {
    val time = measureTimeMillis {
        runBlocking {
            println("Weather forecast")
            printForecast()
            printTemperature()
        }
    }
    println("Execution time: ${time / 1000.0} seconds")
}
suspend fun printForecast() {
    delay(1000)
    println("Sunny")
}

suspend fun printTemperature() {
    delay(1000)
    println("30\u00b0C")
}

//Weather forecast
//Sunny
//30°C
/Execution time: 2.128 seconds

```

### 비동기코드
```kotlin
import kotlin.system.*
import kotlinx.coroutines.*

fun main() {
    val time = measureTimeMillis {
        runBlocking {
            println("Weather forecast")
            launch {
                printForecast()
            }
            launch {
                printTemperature()
            }
        }
    }
    println("Execution time: ${time / 1000.0} seconds")
}

// 출력
// eather forecast
// Sunny
// 30°C
// Execution time: 1.122 seconds

```

음 시간이 빨라진것을 볼 수 있다.</br>
동시 작업을 추가하면 프로그램을 더 빠르게 실행할 수 있다.</br>


```kotlin
fun main() {
    runBlocking {
        println("Weather forecast")
        launch {
            printForecast()
        }
        launch {
            printTemperature()
        }
        println("Have a good day!")
    }
}

//Weather forecast
//Have a good day!
//Sunny
//30°C
```
-----
### asnyc

음 이번에는 내가 정말 좋다고 느꼈던 동시성으로 실행하여 병렬실행 처럼? 보이는 코드다.</br>
병렬, 동시성 코드를 알기 전 까지 나는 Api호출을 직렬?로 했었다.</br>
그래서 한번에 호출해야 할 게 많았을 경우 프로그램이 너무 느렸었다.</br>

```kotlin
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        println("Weather forecast")
        val forecast: Deferred<String> = async {
            getForecast()
        }
        val temperature: Deferred<String> = async {
            getTemperature()
        }
        println("${forecast.await()} ${temperature.await()}")
        println("Have a good day!")
    }
}

suspend fun getForecast(): String {
    delay(1000)
    return "Sunny"
}

suspend fun getTemperature(): String {
    delay(1000)
    return "30\u00b0C"
}
```
async는 Deferred를 반환한다.</br>
코드 구조를 설명하면 2개의 비동기 객체를 만든 후
한꺼번에 await하여 2개를 실행한다.
이렇게 되면 2개가 동시에 실행하기 때문에 시간이 많이 줄어든다.


