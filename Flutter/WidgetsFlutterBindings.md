# WidgetsFlutterBindings.ensureInitialized()는 왜 쓰는 걸까?

Flutter 프로젝트를 진행하다 보면 `WidgetsFlutterBindings.ensureInitailized()` 코드를 흔하게 볼 수 있다.
```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(const MyApp());
}
```

결론 부터 말하면 `WidgetsFlutterBindings.ensureInitailized()`는 Flutter framework와 engine을 연결하는 역할을 한다.
보통은 `runApp`이 알아서 해주지만, `runApp`이 시작되기 전에 네이티브의 기능(통신, 센서, 설정 등)을 급하게 사용해야 
할 때 우리가 수동으로 "Engine이랑 Framework랑 이어주는 다리먼저 건설해!"라고 하는것이다.


## Flutter의 구조
1. Flutter Framework (Dart): 우리가 코드를 작성하는 곳 (Widget, UI 등)
2. Flutter Engine (C++): 실제 화면을 그리고 네이티브(Android/iOS)와 소통하는 본체

이 둘은 하나가 아니다. 원래 따로 떨어져 있다.
이 둘을 이어주는 Binding(연결, 접착?)이 필요하다.
그 역할을 하는 총괄 매니저가 바로 `WidgetsFlutterBinding`이다.


--- 

## 작동원리

case A: `ensureInitialized()`없이 `runApp()`만 실행할 때
```dart
void main() {
  runApp(MyApp());
}
```

- 결과: 정상작동
- 이유: runApp 함수는 실행되자마자 엔진이랑 연결이 안된걸 파악하고 알아서 ensureInitialized()를 호풀해서 다리를 짓고 앱을 그린다. 순수 UI만 그리는 앱이라면 신경 쓸 필요 없다.

case B: `runApp()`전에 비동기 작업을 할 때
```dart
void main() async {
  final prefs = await SharedPreferences.getInstance();
  runApp(MyApp());
}
```
- 결과: 에러발생
- 이유: runApp이 실행되기 전이라 아직 Engine과 Framework가 Binding이 되어있지 않다.

case C: `ensureInitialized()`를 직접 호출할 때
```dart
void main() async {
  WidgetsFlutterBinding.ensureIntialized();

  final prefs = await SharedPreferences.getInstance();

  runApp(MyApp());
}
```
- 결과: 정상 작동
- 이유: 우리가 강제로 다리를 먼저 놓았기 때문에, `runApp` 전이라도 네이티브와 자유롭게 통신할 수 있다.


## main에서 다 로딩하면 어떨까?
기술적으로 `main()`에서 모든 초기화(DB 연결, 서버 통신 등)를 끝내고 runApp을 해도 된다. 하지만 사용자 경험(UX) 측면에서 고민이 필요하다.

Native Splash(운영 체제가 띄워줌)
   - 앱 아이콘을 누르면 OS가 즉시 띄워주는 이미지다. 앱을 실행하는 순간 부터 첫 프레임을 렌더링하는 순간 사라진다.
   - `runApp`전에 무거운 작업을 하면 그 시간만큼 첫 프레임이 늦게 그려진다. 결과적으로 네이티브 스플래시 화면이 오래 떠 있게 된다.(앱이 느리다고 느낄 수 있음)
