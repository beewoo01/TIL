# Coroutine Passing Style

## 어떻게 함수가 중단되고 재개가 되는걸까?
## 자바는 안되느 코틀린은 왜 되는걸까?
## 코틀린은 어떻게 JVM에서 동작하는 걸까?

`Coroutine`으로 코드를 작성하면 컴파일러는 `Coroutine Passing Style (CPS)`로 변환하게 된다.

`Coroutine Passing Style (CPS)`는 Callback을 계속해서 넘겨주는 형태라고 한다.

```kotlin
suspend fun createPost(token: Token, item : Item) : Post { ... }
```
이런 `Coroutine`코드를 컴파일 하게 되면 JVM 에서는 아래와 같이 변환하게 된다.
</br>

```Java
Object createPost(Token token, Item item, Continuation<Post> cont) { ... }
```

이렇게 변환되는것이 `Coroutine Passing Style (CPS)`로 바뀐것이다.
</br></br>

1. Labels 작업이 된다.
```kotlin
suspend fun postItem(item: Item) {
// LABEL 0
  val token = requestToken()
// LABEL 1
  val post = createPost(token, item)
// LABEL 2
  processPost(post)
}
```
컴파일 시 `LABEL`이 찍히게 된다.
해당 함수가 재게 되어야 하는데 재게 되는데 사용할 Point가 필요하다. 
(중단 지점과 다시 재게되어야 할 지점을 알아야 하기 때문)
컴파일러는 아래처럼 코드를 변환하게 된다.

```kotlin
suspend fun postItem(item: Item) {
  switch(lebel) {
    case : 0:
      val token = requestToken()
    case : 1:
      val post = createPost(token, item)
    case : 2:
      processPost(post)
  }
}
```

중단된 시점부터 다시 재게가 되어야 하는것을 `Switch`로 구분하는 것이다.
0번 부터 시작하라, 1번 부터 시작하라 ....
</br>
`LABEL`이 완성되고 나면 `Continuation Passing Style`로 변환하게 된다.

```kotlin
suspend fun postItem(item: Item, cont: Continuation) {
  val sm = object : CoroutineImpl { ... }
  switch(sm.lebel) {
    case : 0:
      val token = requestToken(sm)
    case : 1:
      val post = createPost(token, item, sm)
    case : 2:
      processPost(post)
  }
}
```

"`Continuation`은 `Callback`과 같은것이다." 라고 한다.
모호해서 이걸 ChatGPT에게 자세히 물어보니까
```kotlin
suspend fun foo(): Int {
    return 10
}
```
```kotlin
fun foo(cont: Continuation<Int>): Any {
    cont.resume(10) // 결과를 continuation(콜백)에 넘김
}
```
`Continuation`은 "suspend 함수가 끝난 후 실행할 코드 블록을 담고 있어서 사실상 콜백과 같은 역할을 하는것이다.
라고 설명한다.

이어서 진행해보자.</br>
switch별 각각의 method에 `Continuation`을 넘겨 준다.
`val sm = object : CoroutineImpl { ... }`는 
```kotlin
suspend fun postItem(item: Item, cont: Continuation) {
  val sm = cont as? ThisSM ?: object: ThisSM{
    fun resume(...) {
      postItem(null, this)
    }
  }
  switch(sm.lebel) {
    case : 0:
      sm.item = item
      sm.label = 1
      val token = requestToken(sm)
    case : 1:
      val post = createPost(token, item, sm)
    ...
}
```
이렇게 구성된다.</br>
예를들어 코드에서 `requestToken(sm)`이 끝이나면 `resume`을 호출하게 된다.</br>
코드 순서를 보면 자기자신의 `method`를 호출하는것을 볼 수 있다.</br>

`requestToken`가 끝나면 다시 `postItem`을 호출해서 `createPost`를 호출하고 이런 형식으로 이어서 동작한다.
