# 단일 책임 원칙(SRP : Single Responsibility Principle)

<h4 align="center"><I>"한줄 요약 "</I></h4>
<h6 align="center">한 클래스에 너무 많은 책임을 부여하지 말고 단 하나의 책임만 수행하도록해 변경 사유가 될수 있는 것을 하나로 만들어야 한다</h6>

## 책임이란?
* 책임의 기본 단위는 객체
* 해야 하는 것
* 할 수 있는 것
* 해야 하는 것을 잘 할수 있는것

## 책임의 예시
```java
public class Student{
	public getCourses() {...}
	public addCourse(Course c) {...}
	public void save() {...}
	public void load() {...}
	public void printOnReportCard() {...}
	public void printOnAttendanceBook() {...}
}
```
위에 Student 클래스의 책임을 나열 해보면 ```수강과목을 추가하고``` ```수강목록을 나열하고``` ```성적표와 출석부를 출력하고``` ```학생정보를 저장하고 불러오는``` 책임이 있다. 하지만 Student 클래스에서 가장 잘 할 수있는 책임은 위의 6가지 중에서도 수강과목을 추가하고 불러오는 기능 두가지 일 것이다. **이처럼 클래스가 가장 적합한 책임만 수행하는 것이 단일 책임 원칙(SRP : Single Responsibility Principle)이다.**

## 책임 분리
만약 Student 클래스를 위와 같이 여러 책임을 수행 하도록 만든다면 Student 클래스를 사용하는 코드들도 많아지게 된다. 이렇게 되면 Student 클래스를 변경할때 Student 클래스를 사용하는 코드들도 직접 또는 간접적으로 사용하는 코드를 다시 테스트 해야하는 일이 발생한다. 

> 회귀 테스트 : 시스템에 변경이 발생할 때 기존의 기능에 영향을 주는지 평가하는 테스트 

**따라서 모든 코드를 테스트하는 문제를 해결하려면 한 클래스에 너무 많은 책임을 부여하지 말고 단 하나의 책임만 수행하도록해 변경 사유가 될수 있는 것을 하나로 만들어야 한다.**
