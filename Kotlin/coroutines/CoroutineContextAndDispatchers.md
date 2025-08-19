```kotlin
fun main() = runBlocking<Unit> {
    launch {   // 인자를 안 넘겨주면 상위 코루틴의 Context 를 가짐
        println("main runBlocking       : 나는 ${Thread.currentThread().name} 에서 돌아")
    }
    launch(Dispatchers.Unconfined) {  // 별도로 지정을 안 해줌
        println("Unconfined             : 나는 ${Thread.currentThread().name} 에서 돌아")
    }
    launch(Dispatchers.Default) {  // DefaultDispatchers 등록
        println("Default                : 나는 ${Thread.currentThread().name} 에서 돌아")
    }
    launch(newSingleThreadContext("H43RO_Thread")) {  // 새로운 쓰레드 생성
        println("newSingleThreadContext : 나는 ${Thread.currentThread().name} 에서 돌아")
    }
}
```

- The coroutine contex includes a coroutine dispatcher
- determines what thread for its execution
- All coroutine builoders an optional CoroutineContext parameter
- that can be used to explicitly specift the dispatcher

- 코루틴 컨텍스트에는 코루틴 디스패처가 포함된다.
- 디스패처는 코루틴이 어떤 스레드에서 실행될지를 결정한다.
- 모든 코루틴 빌더(launch, async 등)는 선택적인 CoroutineContext 매개변수를 가진다.
- 이 매개변수는 디스패처를 명시적으로 지정하는 데 사용할 수 있다.

위 예제 코드를 실행해보면

> 1번 launch
main runBlocking
```kotlin
launch {
  println("main runBlocking       : 나는 ${Thread.currentThread().name} 에서 돌아")
}
```
코드를 보면 runBlocking에서 optional parameter 없이 실행한다.</br>
이런 경우 자신을 호출했던 코루틴에서 상속을 받아서 작업을 하게 된다.</br>
위 코드의 경우는 Main Thread에서 호출을 했기 때문에 Main Thread에서 실행된다.</br>
</br>

> 2번 launch(Dispatchers.Unconfined)
Unconfined가 main


> 3번 launch(Dispatchers.Default)</br>
Default DefaultDisparcher-worker-1 </br>

> 4번 launch(newSingleThreadContext("H43RO_Thread"))</br>
newSingleThreadContext MyOwnThread 비용이 높은 방식이다.</br>
해당 방식은 코루틴을 실행할 때 마다 쓰레드를 생성한다.</br>
예제처럼 사용하면 안되고 아래처럼 사용해야 한다.</br>
왜냐하면 use Block안에서 Close를 해줘야 한다.</br>
그렇지 않으면 memory leak이 발생한다.</br>

```kotlin
newSingleThreadContext("MyOwnThread").use {
  launch() {
    println("newSingleThreadContext : 나는 ${Thread.currentThread().name} 에서 돌아")
  }
}
```
</br></br>



## Debugging coroutines and threads
```kotlin
fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")
 
runBlocking{
    val a = async{
        log(“I’m a”)
        6
    }
    val b = async{
        log(“I’m b”)
        7
    }
    log(“answer is ${a.await() * b.await()}”)
}
```



## Jumping between threads
```kotlin
fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")

@OptIn(DelicateCoroutinesApi::class)
fun main() {
    newSingleThreadContext("Ctx1").use { ctx1 ->
        newSingleThreadContext("Ctx2").use { ctx2 ->
            runBlocking(ctx1) {
                log("Started in ctx1")
                
                withContext(ctx2) {
                    log("Working in ctx2")
                }

                log("Back to ctx1")
            }


        }
    }
}
```
- `withContext`를 이용하면 Thread를 Switch 할 수 있다.

실행화면
```text
[Ctx1] Started in ctx1
[Ctx2] Working in ctx2
[Ctx1] Back to ctx1
```

Ctx1에서 두 번 Ctx2에서 한 번 실행되는것을 확인 할 수 있고,</br>
`withContext`에서 Thread가 전환 되는것도 확인할 수 있다.</br>

</br></br>

### Child of a coroutine
코루틴을 사용하면 호출한 코루틴의 자식이 된다.</br>
하지만 안되는 경우도 있다. `GlobalScope.launch`를 사용했을 경우다.</br>
`GlobalScope.launch`는 독립적인 Scope이다.</br>
</br>



