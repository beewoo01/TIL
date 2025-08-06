## 시스템 개발의 '방법론'
- 분석
> 요구사항 분석 : 시스템을 어떻게 개발하기를 원하는지 파악하는 단계이다.

- 설계
> 대략적인 그림을 프로그램으로 만들 수 있도록 설계하는 작업이다.
> 어던 메소드를 만들지, 데이터는 어떻게 저장할지 등등의 세부적인 것들을 정리한다.

- 개발 및 테스트
> 설계에서 만들기로 한 것들을 개발하는 단계이다.
> 실제 시스템에서 제공해야 하는 기능을 만드는 단계이다.
> 필요한 기능들이 제대로 동작하는지 확인하는 "테스트" 작업을 수행한다.


- 시스템 릴리즈
> 사용자들에게 시스템을 제공하는 단계
> 오픈 이후 운영/유지보수 단계를 거치면서 문제가 있는 부분들을 고쳐나간다.
> 



## Interface란?
- 메서드의 시그니처만 정의하고 구현은 제공하지 않음
- 다중 구현 가능
- 설계의 역할: 이 인터페이스를 구현한 클래스는 이 메서드들을 반드시 구현해야 한다.

```java
public interface Animal {
    void sound();  // 추상 메서드
}


public class Dog implements Animal {
    @Override
    public void sound() {
        System.out.println("멍멍");
    }
}

```

<br/>설계 단계에서 인터페이스만 만들어 놓고, 
<br/>개발 단계에서 실제 작업을 수행하는 메소드를 만들면 
<br/>설계 단계의 산출물과 개발 단계의 산출물 이 보다 효율적으로 관리된다.
<br/><br/><br/>

--- 

## Abstractclass(추상화 클래스)
- 하나 이상의 추상 메서드를 포함할 수 있는 클래스
- 직접 인스턴스화할 수 없음
- 상속받는 자식 클래스에서 반드시 구현해야 함

```java
public abstract class Animal {
    // 일반 메서드 (구현 O)
    public void eat() {
        System.out.println("먹는다");
    }

    // 추상 메서드 (구현 X → 자식 클래스가 반드시 구현해야 함)
    public abstract void sound();
}


public class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("멍멍");
    }
}

```

- abstract 클래스는 클래스 선언시 abstract라는 예약어가 클래스 앞에 추가되면 된다.
- abstract 클래스 안에는 abstract으로 선언된 메소드가 0개 이상 있으면 된다.
- abstract로 선언된 메소드가 하나라도 있으면, 그 클래스는 반드시 abstract로 선언되어야만 한다.
- abstract 클래스는 몸톤이 있는 메소드가 0개 이상 있어도 전혀 상관 없으며, static이나 final 메서드가 선언되어 있으면 안 된다.

> * abstract 클래스는 인터페이스처럼 implements라는 예약어를 사용하려 구현할 수 없다. 왜냐하면 인터페이스가 아니라 클래스이기 때문이다.
