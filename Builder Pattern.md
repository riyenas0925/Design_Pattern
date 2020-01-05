# Builder Pattern

### 1. Builder Pattern 예시
```java
Student student = Student.builder()
    .name("강동민")
    .age(19)
    .build();
```
> 위와같은 패턴을 Dot(.) Chain 문법이라고 한다.

### 2. Builder 패턴이란??
* GoF
    > 객체의 생성 알고리즘과 조립 방법을 분리하는 것이 목적

* Effective Java  
    > 코드 읽기/유지보수가 편해짐

*생성자 인자가 많을때 Builder Patter은 효율적이고 유연하게 생성할수 있음!!!*

### 3. 일반적인 객체 생성
##### Student.java
```java
public class Student(){
  private String firstName;
  private String lastName;
  private boolean graduated;
  
  //setter, getter, toString 생략
}
```

##### StudentTest.java
```java
public class StudentTest {
  public static void main(String[] args) {
    Student student = createStudent();
    System.out.println(student.toString())
  }
  
  public static Student createStudent() {
    student student = new Student();
    student.setFirstName("강");
    student.setLastName("동민");
    student.setGraduated(true);
    return student;
  }
}
```

만약 Student Class에서 firstName, lastName 둘만 초기화 하고싶을수도 있고 graduated만 초기화 하고싶을수 있다 하지만 일반적인 방식은 초기화 하고 싶은 파라미터가 달라지면 그에 맞게 생성자를 새로 만들어줘야하는 문제점이 있고 생성자를 이용한 방식은 파라미터의 순서도 매우 중요하고 위치만 봐서는 자료형만 같다면 값이 뒤바뀌어도 문법적으로는 오류가 없기때문에 개발자가 파악하기 힘들다.

### 4. Builder Pattern을 사용한 객체 생성
##### Student.java
```java
public class Student(){
  private String firstName;
  private String lastName;
  private boolean graduated;
  
  public static Student builder() {
    return new StudentBuilder();
  }
  
  //setter, toString 생략
}
```

##### StudentBuilder.java
```java
public class Student(){
  private String firstName;
  private String lastName;
  private boolean graduated;
  
  public StudentBuilder firstName(String firstName){
    this.firstName = firstName;
    return this;
  }
  
  public StudentBuilder lastName(String lastName){
    this.lastName = lastName;
    return this;
  }
  
  public StudentBuilder graduated(boolean graduated){
    this.graduated = graduated;
    return this;
  }

  public Student build() {
    student student = new Student();
    student.setFirstName("강");
    student.setLastName("동민");
    student.setGraduated(true);
    return student;
  }
}
```

##### StudentTest.java
```java
public class StudentTest {
  public static void main(String[] args) {
    Student student = Student.builder()
      .name("강동민")
      .age(19)
      .build();
    System.out.println(student.toString())
  }
}
```
