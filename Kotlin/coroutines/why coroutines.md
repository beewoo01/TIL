# Coroutine
- 코루틴(coroutine)은 루틴의 일종
- 협동 루틴이라 할 수 있다.
- 코루틴의 "Co"는 with 또는 together를 뜻한다. (함께 하는 루틴 인건가)
- 코루틴은 이전에 자신의 실행이 마지막으로 **중단**되었던 지점 다음의 장소에서 실행을 <font color="red">재개</font>한다.
- 코루틴은 렵력 작업, 예외, 이벤트 루프, 반복자, 무한 목록 및 파이프와 같은 친숙한 프로그램 구성 요소를 고현하는 데 적합하다.

# Android Developer Coroutine
- A coroutine is a concurrency design pattern that you can use on Android to simplify code that executes asynchronously.
</br>해석: 코루틴은 안드로이드에서 비동기적으로 실행되는 코드를 단순화하기 위해 사용할 수 있는 **동시성 설계 패턴**이다.
</br>강사의 말 : 비동기처리에 대한 코드를 간단하게 작성할 수 있게 해준다.
- On Android, coroutines help to manage long-running tasks that might otherwise block the main thread and cause your app to become unresponsive
</br>해석: 안드로이드에서 코루틴은 메인 스레드를 차단해 앱이 응답하지 않게 만들 수 있는 **장시간 실행되는 작업**을 관리하는 데 도움을 준다.
</br>강사의 말 : 메인 Thread가 blocking되는 부분의 도움을 준다.

# Use Kotlin Coroutines in your Android App
- Coroutines are a Kotlin feature that converts async callbacks for long-running tasks, such as database or network access, into sequential code.
<br/>해석: 코루틴은 데이터베이스나 네트워크 접근처럼 오래 걸리는 작업의 비동기 콜백을 순차적인 코드로 변환하는 코틀린 기능입니다.
- Use suspend functions to make async code sequential
<br/>해석: 비동기 코드를 순차적으로 만들기 위해 suspend 함수를 사용하세요.
<br/>
<br/>
---

## The Dream Code
안드로이드 공식 홈페이지에서 소개한 영상에서 다룬 내용이다.
아래와 같이 아무런 비동기 처리 없이 바로 사용하는 것을 Dream Code라고 하는거 같다.
```kotlin
val user = fetchUserData()
textView.text = user.name
```

하자만 이렇게 작성하면 에러가 난다. `NetworkOnMainThreadException`

```kotlin
thread {
  val user = fetchUserData()
  textView.text = user.name
}
```
그래서 이렇게 thread를 사용하려고 하면 `CalledFromWrongTheadException`이 나온다.
`textView.text`의 코드는 UI를 업데이트 하는 코드이다.
에러가 나오는 이유는 UI업데이트 코드는 UI Thead에서 해줘야 하는데 
별도의 Thread에서 하기 때문이다.
</br>

## The OK Code
그래서 나온 해결책은
```koltin
fetchUserData { user -> // callback
  textView.text =  user.name
}
```
Thead를 이용해서 서버를 Call한 후 다시 메인 Thread로 스위칭 한 후 작업을 하는것이다.
이것도 문제가 있다 `OutOfMemoryError`이다. 
이런 방식은 메모리를 철저하게 관리해줘야 한다.
call한 객체들은 dispose혹은 clear해줘야 한다.

이렇게 되면 thread에서 `calcel()` 코드들이 상당히 많이 들어간다.
```kotlin
override fun onStop() {
  subscription.cancel()
  subscription1.cancel()
  subscription2.cancel()
  subscription3.cancel()
  subscription4.cancel()
  subscription5.cancel()
  subscription6.cancel()
}
```
이것들을 간단하게 해결하는데 구글이 추천하는 것은 kotlin coroutine이다.
</br>

코루틴을 사용히 suspend를 사용해서 Dream Code와 상당히 유사하게 작성할 수 있게 된다.
```kotlin
suspend fun loadUser() {
  val user = api.fetchUser()
  show(user)
}
```
이렇게 되면 위에서 봤던 Dream Code와 비교를 해보면 suspend fun으로 사용하는것 말고는 동일하다.
