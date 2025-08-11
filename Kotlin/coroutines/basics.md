# Example 1

```kotlin
fun main() {
  GlobalScope.launch {
    delay(1000L)
    println("World!")
  }
  println("Hello, ")
  Thread.sleep(2000L)
}
```
- Essentially, coroutines are light-weight threads
- launch (coroutine builder)
- CoroutineScope
- GlobalScope
- replacing GlobalScope.launch { } => thread {}

```kotlin
import kotlinx.coroutines.DelicateCoroutinesApi
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.delay
import kotlinx.coroutines.launch

@OptIn(DelicateCoroutinesApi::class)
fun main() {
    GlobalScope.launch {
        delay(1000L)
        println("World!")
    }

    println("Hello,")
    Thread.sleep(2000L)
}
```
이 예제는 'Hello,'가 먼저 나오고 1초 뒤 'World!'가 출력 된 후 2초 후 종료되는 프로그램이다.

`launch`에서 coroutine이 만들어진다.

`launch`는 내부적으로 coroutine을 반환한다.

```kotlin
public fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job {
    val newContext = newCoroutineContext(context)
    val coroutine = if (start.isLazy)
        LazyStandaloneCoroutine(newContext, block) else
        StandaloneCoroutine(newContext, active = true)
    coroutine.start(start, coroutine, block)
    return coroutine
}
```

여기서 알 수 있는것은 `launch`는 coroutine 빌더며, 내부적으로 coroutine 반환한다.
CoroutineScope는 전역 Scope이다.
builder(launch)를 사용하기 위해서는 Scope가 필요하다.


여기서 coroutine의 코드를 thread로 변경하면 완전 동일한 결과가 된다는 것을 알 수 있다.

```kotlin
fun main() {
    thread {
        Thread.sleep(1000L)
        println("World!")
    }

    println("Hello,")
    Thread.sleep(2000L)
}
```


# Example 2
runBlocking
```kotlin
fun main() {
    GlobalScope.launch {
        delay(1000L)
        println("World!")
    }

    println("Hello,")
    runBlocking {
        delay(2000L)
    }
}
```


```kotlin
public actual fun <T> runBlocking(context: CoroutineContext, block: suspend CoroutineScope.() -> T): T {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
    val currentThread = Thread.currentThread()
    val contextInterceptor = context[ContinuationInterceptor]
    val eventLoop: EventLoop?
    val newContext: CoroutineContext
    if (contextInterceptor == null) {
        // create or use private event loop if no dispatcher is specified
        eventLoop = ThreadLocalEventLoop.eventLoop
        newContext = GlobalScope.newCoroutineContext(context + eventLoop)
    } else {
        // See if context's interceptor is an event loop that we shall use (to support TestContext)
        // or take an existing thread-local event loop if present to avoid blocking it (but don't create one)
        eventLoop = (contextInterceptor as? EventLoop)?.takeIf { it.shouldBeProcessedFromContext() }
            ?: ThreadLocalEventLoop.currentOrNull()
        newContext = GlobalScope.newCoroutineContext(context)
    }
    val coroutine = BlockingCoroutine<T>(newContext, currentThread, eventLoop)
    coroutine.start(CoroutineStart.DEFAULT, coroutine, block)
    return coroutine.joinBlocking()
}
```

`runBlocking`또한 코루틴을 만드는 builder이다.
예제코드 `runBlocking {delay(2000L)}` 를 보면 delay를 주려고 blocking할 수 있는 코루틴을 사용하는 것을 볼 수 있다.
* Blocking이란 해당 작업이 끝나기 전까지 해당 Thread가 아무런 작업을 못하도록 하는것이다.


## join
```kotlin
fun main() = runBlocking {
    val job = GlobalScope.launch {
        delay(1000L)
        println("World!")
    }

    println("Hello,")
    job.join()
}
```

`launch`를 하게 되면 `job`이 반환된다.
`job.join()`코드는 job이라는 객체가 완료될 때 까지 기다리게 된다.

이런 코드는 단점이 있다.
1. 이렇게 된다면 launch를 사용할 때마다 계속 join을 써줘야 한다.
2. 비동기의 장점을 버리고 순차 실행처럼 동작한다.
3. 예외나 취소가 부모로 전파되지 않는다.

위 단점들을 해결하기 위한 대안으로 `runBlocking` 안에서 `Scope`를 하는 것이다.
```kotlin
fun main() = runBlocking {
    launch {

        delay(1000L)
        println("World!1")
    }

    launch {
        delay(1000L)
        println("World!2")
    }

    println("Hello,")
}
```
`runBlocking`이 두 `launch`가 끝날 때까지 자동으로 기다려준다.
예외, 취소 모두 부모-자식 관계에서 안전하게 전파된다.
