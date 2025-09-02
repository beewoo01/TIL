# 코루틴 빌더

중단 함수는 Continueation 객체를 다른 중단 함수에 전달해야한다.</br>
이 말은 중단 함수는 일반함수를 호출 할 수 있지만, 일반 함수는 중단함수를 호출 할 수 없다.</br>
모든 중단함수는 중단함수에 의해 호출 되어야 한다.</br>
준단 함수를 연속으로 호풀하면 시작되는 지점이 반드시 있다. **코루틴 빌더**가 그 역할을 한다.</br>
</br>
---

## launch 빌더
launch가 작동하는 방식은 Thread가 작동하는 방식과 비슷하다.</br>
```kotlin
fun main() {
  GlobalScope.launch {
    delay(1000L)
    println("World!")
  }

  GlobalScope.launch {
    delay(1000L)
    println("World!")
  }

  GlobalScope.launch {
    delay(1000L)
    println("World!")
  }
  println("Hello,")
  Thread.sleep(2000L)
}

// Hello
// (1초 후)
// World!
// World!
// World!
```

참고 사항 : GlobalScope의 사용을 지양해야 한다.</br>
이유
1. 수명과 무관하게 동작한다.
> `GlobalScope`는 Application 전역에 살아이쓴ㄴ 스코프라서, 어디서 시작하든 앱이 좋료될 때까지 취소되지 않는다.
> 메모리 누수(leak) 가능성 있다.
> 화면이 사라진 뒤에도 네트워크/DB작업이 수행될 수 있음
</br>
2. 구조적 동시성을 깨뜨린다.
> 이 내용은 다음에 다룰예정이라고 한다. 사실 이유는 나중에 설명해준다고 책에 적혀있는데 내가 궁금해서 chatGPT에게 질문해서 작성한거다.
</br>
3. 예외처리의 어려움
</br>

위 코드를 분석해보자</br>
main 함수의 끝에 Thread.sleep을 호출 해야한다.</br>
이유는 Sleep이 없다면 코루틴이 실행할 시간조차 주지않고 종료된다.</br>
</br>
---
</br>

## runBlocking 빌더
코루틴은 스레드를 블로킹하지 않고 중단시키기만 하는 것이 일반적이다.</br>
하지만 블로킹을 해야할 경우도 있다. (블로킹이란 Thread가 어떤 작업이 끝날 때까지 다른 일을 하지 못하고 멈춰있는 상태다.)</br>
메인 함수의 경우 프로그램을 너무 빨리 끝내지 않기 위해 스레드를 블로킹해야 한다.</br>
이럴 때 `runBlocking`을 사용하면 된다.</br>

코루틴이 중단되었을 경우 `runBlocking` 빌더는 중단 메인 함수와 마찬가지로 시작한 스레드를 중단 시킨다.</br>
> runBlocking 내부에서 delay(1000L)을 호출하면 Thread.sleep(1000L)과 비슷하게 작동한다.</br>

```kotlin
fun main() {
  runBlocking{
    delay(1000L)
    println("World!")
  }
  runBlocking{
    delay(1000L)
    println("World!")
  }
  runBlocking{
    delay(1000L)
    println("World!")
  }
  println("Hello,")
}

// (1초 후)
// world
// (1초 후)
// world
// (1초 후)
// world
// Hello,


fun main() {
  Threa.sleep(1000L)
  println("World!")
  Threa.sleep(1000L)
  println("World!")  Threa.sleep(1000L)
  println("World!")
  println("Hello,")
}


// (1초 후)
// world
// (1초 후)
// world
// (1초 후)
// world
// Hello,
```
</br>

runBlocking이 사용되는 특수한 경우 2가지</br>
1. 프로그램이 끝나는 걸 방지하기 위해 스레드를 블로킹할 필요가 있는 메인 함수다.</br>
2. 프로그램이 끝나는 걸 방지하기 위해 스레드를 블로킹할 필요가 있는 유닛 테스트다.</br>
</br>

```kotlin
fun main() = runBlocking {
  ...
}

class MyTests {
  @Test
  fun `a test`() = runBlocking {
  
  }
}
```

`runBlocking`은 코루틴 빌더로 중요하게 사용되었으나, 지금은 거의 사용되지 않는다.</br>
유닛 테스트에서는 코루틴을 가상 시간으로 실행시키는 runTest가 주로 사용되고 있다.</br>
메인 함수에서는 `suspend`를 붙여 중단 함수로 만드는 방법을 주로 사용한다.</br>
```koltin
suspend fun main() {
  ...
}
```
</br>

--- 
## async 빌더
launch와 비슷하나 값을 생성하도록 설계되었다.</br>
이 값은 람다 표현식에 의해 반환되어야 한다.</br>
Deferred<T> 타입의 객체를 리턴한다.</br>
Deferred에는 작업이 끝나면 값을 반환하는 중단 메서드인 await이 있다.</br>

```kotlin
fun main() = runBlocking {
  val resultDeferred: Deferred<Int> = FlobalScope.async {
    delay(1000L)
    42
  }

  val result: Int = resultDeferred.await() //1초 후
  println(result) // 42
  println(resultDeferred.await()) // 42
}
```

async빌더는 호출 되자마자 바로 시작된다.</br>
즉 몇 개의 작업을 한번에 시작하고 모든 결과를 한꺼번에 기다릴 때 사용한다.</br>
