# 12장 모든 자바 클래스의 부모인 java.lang.Object 클래스

## Object 클래스

```java
public class inheritanceObject {
  public static void main(String[] args) throws Exception {
    InheritanceObject object = new InheritanceObject();
    ~~~
  }
}
```

위 클래스는 아무것도 확장하지 않는것을 알 수 있다.
<br/>`extends`가 없기 때문이다.
<br/>자바에서는 기본적으로 아무것도 상속받지 않으면 기본적으로 `Object`를 상속받게 되어있다.
<br/>
<br/>
<br/>
왜 모든 클래스는 `Object`클래스를 상속받게 받을까?
<br/>`Object` 클래스에 있는 메소드들을 통해서 클래스의 기본적인 행동을 정의할 수 있기 때문이다.
<br/>`Object` 클래스는 클래스를 구현하기 위해 가장 기본적인 메소드를 정의해 놓은 클래스이다.
<br/>
<br/>


---


## Object 클래스에서 제공하는 메소드들의 종류
|메소드|설명|
|----|---|
|protected Object clone()|객체의 복사본을 만들어 리턴한다.|
|public boolean equals(Object obj)|현재 객체와 매개 변수로 넘겨받은 객체가 같은지 확인한다. 같으면 true를, 다르면 false를 리턴한다.|
|protected void finalize()|현재 객체가 더 이상 쓸모가 없을 때 가비지 컬럭터에 의해 이 메소드가 호출된다.|
|public class<?> getClass()|현재 객체의 Class 클래스의 객체를 리턴한다.
|public int hashCode()|객체에 대한 해시코드(hash code)값을 라턴한다. 해시 코드라는 것은 "16진수로 제공되는 객체의 메모리 주소"를 말한다.|
|public String toString()|객체를 문자열로 표현하는 값을 리턴한다.|

<br/>
<br/>

---

## 직접해 봅시다.
- name, address, phone, email 변수가 선언되어 있는 것을 확인하자.
- Student 클래스의 내용 즉 name. address, phone, email값이 같으면 같은 클래스로 인식이 되도록 equals() 메소드를 직접 작성해보자.
- ManageStudent 클래스에 매개 변수 및 리턴값이 없는 checkEquals()라는 메소드를 만들자.
- CheckEquals() 메소드에 다음과 같이 값이 동일한 두 개의 Student 객체를 생성하자.
- a와 b객체가 같으면 "Equal"을, 다르면 "Not Equal"을 출력하는 문장을 checkEquals()메소드에 추가하자.
- ManageStudent 클래스의 main() 메소드에서 나머지 메소드 호출은 주석 처리하고, checkEquals()메소드를 호출하도록 하자.
- ManageStudent 클래스를 컴파일하고, 실행 결과가 "Equal"로 출력되는지 확인해 보자.

> Student class
```java
public class Student {
	String name, address, phone, email ;
	public Student(String name) {
		this.name=name;
	}
	public Student(String name, String address, String phone, String email) {
		super();
		this.name = name;
		this.address = address;
		this.phone = phone;
		this.email = email;
	}
	@Override
	public String toString() {
		return name + " " + address + " "+ phone + " " + email;
	}
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((address == null) ? 0 : address.hashCode());
		result = prime * result + ((email == null) ? 0 : email.hashCode());
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		result = prime * result + ((phone == null) ? 0 : phone.hashCode());
		return result;
	}
	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Student other = (Student) obj;
		if (address == null) {
			if (other.address != null)
				return false;
		} else if (!address.equals(other.address))
			return false;
		if (email == null) {
			if (other.email != null)
				return false;
		} else if (!email.equals(other.email))
			return false;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		if (phone == null) {
			if (other.phone != null)
				return false;
		} else if (!phone.equals(other.phone))
			return false;
		return true;
	}
	
}
```

<br/><br/><br/>
```java
public class ManageStudent {

	public static void main(String[] args) {
		ManageStudent sample=new ManageStudent();
		sample.checkEquals();
	}

	public Student[] addStudent() {
		Student[] student = new Student[3];
		student[0] = new Student("Lim");
		student[1] = new Student("Min");
		student[2] = new Student("Sook", "Seoul", "010XXXXXXXX","ask@godofjava.com");
		return student;
	}
	public void printStudents(Student[] student) {
		for(Student data:student) {
			System.out.println(data);
		}
	}
	
	public void checkEquals() {
		Student a=new Student("Min","Seoul","010XXXXXXXXX","ask@godofjava.com");
		Student b=new Student("Min","Seoul","010XXXXXXXXX","ask@godofjava.com");
		if(a.equals(b)) {
			System.out.println("Equal");
		} else {
			System.out.println("Not Equal");
	  }
	}
}
```

---

## 정리해 봅시다
Q1. 모든 클래스의 최상위 부모 클래스인 Object 클래스는 어떤 패키지에 선언되어 있나요?
<br/>
A1. java.lang package에 선언되어 있습니다.
<br/>

Q2. 클래스가 어떻게 선언되어 있는지 확인할 수 있는 명령어(실행 파일)의 이름은 무엇인가요?
<br/>
A2. 클래스 파일만 갖고 클래스가 어떻게 선언되어 있는지 확인하려면 javap 명령어를 사용하면 됩니다.
<br/>

Q3. Object 클래스에 선언되어 있는 모든 메소드는 여러분들이 Overriding해야 하나요?
<br/>
A3. 아닙니다. 필요한 메소드만 Overriding하여 사용하면 됩니다.
<br/>

Q4. Object 클래스의 clone() 메소드의 용도는 무엇인가요?
<br/>
A4. 객체를 복제하기 위함입니다.
<br/>

Q5. System.out.println() 메소드를 사용하여 클래스를 출력했을 때 호출되는 Object 클래스에 있는 메소드는 무엇인가요?
<br/>
A5. toString()에서 호출된 결과를 제공합니다.
<br/>

Q6. 객체의 주소를 비교하는 것이 아닌, 값을 비교하려면 Object 클래스에 선언되어 있는 어떤 메소드를 overriding해야 하나요?
<br/>
A6. 참조 자료형의 비교는 equals() 메소드를 사용해야 확실하게 비교가 가능하다.
<br/>

Q7. Object 클래스에 선언되어 있는 hashCode()라는 메소드는 어떤 타입의 값을 리턴하나요?
<br/>
A7. int 타입의 결과를 리턴합니다.
<br/>
