# Hilt Annotations

### 1. Application - level Annotation
 `HiltAndroidApp`
  - Purpose: Initializes Hilt in your Android application<br/>
  App에서 Hilt를 초기화 한다
  - Usage: Must be added to the Application class.<br/>
  Application class를 추가해야 한다.
  
### 2. Android Components Annotations
 - These annotations allow Hilt to inject dependencies **into Android** components.<br/>
 이 Annotation들은 Hilt가 Android 구성 요소에 의존성을 주입할 수 있게 해준다.
<br/><br/>

@AndroidEntyPoint
 - Purpose: Enables dependency injection into Android framework classes
(e.g., Acrivity, Fragment, Service).<br/>
Android(ex: Activity, Fragment, Service) 프레임워크 클래스에 의존성을 주입할 수 있게 한다.

 - Usage: Must be added to Activties, Fragments or Service <br/>
Activity, Fragment들 또는 Servicedp 추가해야 한다.
<br/><br/>

@EntryPoint
- Purpose: Used to inject dependencies into non-Hilt-managed classes.<br/>
Hilt가 관리하지 않는 클래스에 의존성을 주입하는 데 사용돤다.
- Usage: Commonly udes in Broadcast Receivers or Content Providers.<br/>
주로 Broadcast receiver나 content provider에서 사용된다.
<br/><br/>
---

### 3. Module Annotations 
Modules define howto provide dependencies<br/>
모듈은 의존성을 제공하는 방법을 정의한다.

@Module
- Purpose: Marks a class as a Hilt module for providing dependencies.<br/>
클래스를 의존성을 제공하기 위한 Hilt모듈로 지정한다.
<br/><br/>

@InstallIn
- Purpose: Specifies the lifecycle scope of the module(e.g., SingletonComponent, ActivityComponent)<br/>
모듈의 생명주기 범위를 지정한다.
- Common Components:
    - SingletonComponent: Application-wide singleton dependencies.<br/>
    애플리케이션 전체 범위의 싱글톤 의존성
    - ActivityComponent: Scoped to a single activity.<br/>
    단일 액티비티 번위로 제한된다.
    - FragmentComponent: Scoped to a fragment.<br/>
    프래그먼트 범위로 제한된다.
<br/><br/>

@Provides
- Purpose: Defines a method to create and provide a dependency.<br/>
의존성을 생성하고 제공하는 메서드를 정의한다.
<br/><br/>

@Binds
- Purpose: Used to bind interfaces to their implementations.<br/>
인터페이스를 해당 구현체에 바인딩`(연결)`하는 데 사용된다.
- Requirement: The method must be abstract<br/>
Bind를 사용하려면 추상(abstract)메서드 이어야 한다.
<br/><br/>
--- 

### 4. Injection Annotations

@Inject
- Purpose: Marks a constructor, field, or method for injection.<br/>
생성자, 필드, 또는 메서드를 주입 대상으로 지정한다.
- Usage: Common in Constructor, Filed, or Method Injection.<br/>
생성자, 필드 또는 메서드 주입에서 흔히 사용된다.
<br/><br/>

@Named
- Purpose: Differentiates between dependencies of the same type.<br/>
동일한 타입의 의존성들을 서로 구분한다.
- Usage: Used when you have multiple implementations of the same type.<br/>
동일한 타입의 구현체가 여러 개 있을 때 사용된다.

@Qualifire
- Purpose: Custom version of @Named for more descriptive dependency lables.<br/>
더 명확하고 설명적인 의존성 라벨을 제공하기 위한 `@Named`의 사용자 정의 버전이다.
<br/><br/>
--- 
### 5. Scoping Annotations
These Annotations define lifecycle scopes of dependencies.<br/>
이 Annotation들은 의존성의 생명주기 범위를 정의한다.

@Singleton
- Purpose: Ensures a single instance of a dependency is created **application-wide**.<br/>
애플리케이션 전체에서 의존성의 단일 인스턴스가 생성되도록 보장한다.
<br/><br/>

@ActivityScoped
- Purpose: The dependency lives as long as the Activity is alive. </br>
해당 의존성은 Activity가 살아있는 동안 유지된다.
<br/><br/>

---
### 6. Testing Annotations
@HiltAndroidTest
- Purpose: Used in test classed to enable Hilt testing.<br/>
Hilt 테스트를 활성화하기 위해 테스트 클래스에서 사용된다.
<br/><br/>

@UninstallModules
- Purpose: Excludes specific modules during testing.<br/>
테스트 중에 특정 모듈을 제외한다.





