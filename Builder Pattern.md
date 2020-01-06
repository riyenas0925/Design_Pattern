# Builder Pattern

### 1. Builder Pattern 예시
```java
Student student = Student.builder()
    .firstName("강")
    .lastName("동민")
    .graduated(true)
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
##### Main.java

```java
class Student{
    private String firstName;
    private String lastName;
    private boolean graduated;
    
    public Student(String firstName, String lastName, boolean graduated){
        this.firstName = firstName;
        this.lastName = lastName;
        this.graduated = graduated;
    }
    
    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }
    
    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
    
    public void setGraduated(boolean graduated) {
        this.graduated = graduated;
    }
    
    @Override
    public String toString() {
        return "Student{" +
                "fisrtName='" + this.firstName + "\'" +
                ", lastName='" + this.lastName + "\'" +
                ", graduated='" + this.graduated + "\'" +
                '}';
    }
}

public class Main{
    public static void main(String []args){
        Student student = createStudent();
        System.out.println(student.toString());
    }

    public static Student createStudent() {
        Student student = new Student("강", "동민", true);
        return student;
    }
}
```

#### 문제점
만약 Student Class에서 firstName, lastName 둘만 초기화 하고싶을수도 있고 graduated만 초기화 하고싶을수 있다 하지만 일반적인 방식은 초기화 하고 싶은 파라미터가 달라지면 그에 맞게 생성자를 새로 만들어줘야한다.

```java
public Student(String firstName, String lastName, boolean graduated) {...}  // 전부다 초기화 하고 싶을때
public Student(String firstName, String lastName, ) {...}                   // fisrt,lastName만 초기화 하고 싶을때
public Student(boolean graduated) {...}                                     // graduated만 초기화 하고 싶을때
```
생성자를 이용한 방식은 파라미터의 순서도 매우 중요하고 위치만 봐서는 자료형이 같다면 값이 뒤바뀌어도 문법적으로는 오류가 없기때문에 개발자가 어떤 값을 넣어야 하는지 파악하기 힘들다 따라서 ```Builder Pattern```을 사용하여 문제를 해결할수 있다.

```java
//생성자 예시
public Student(String firstName, String lastName, boolean graduated) {...}

//객체 생성
Student student = new Student("강", "동민", true); //문법적 오류 x
Student student = new Student("동민", "강", true); //문법적 오류 x
```

### 4. Builder Pattern을 사용한 객체 생성
##### Main.java
```java
class Student{
    private String firstName;
    private String lastName;
    private boolean graduated;
    
    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }
    
    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
    
    public void setGraduated(boolean graduated) {
        this.graduated = graduated;
    }
    
    public static StudentBuilder builder() {
        return new StudentBuilder();
    }
  
    @Override
    public String toString() {
        return "Student{" +
                "fisrtName='" + this.firstName + "\'" +
                ", lastName='" + this.lastName + "\'" +
                ", graduated='" + this.graduated + "\'" +
                '}';
    }
}

class StudentBuilder{
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
        Student student = new Student();
        student.setFirstName(firstName);
        student.setLastName(lastName);
        student.setGraduated(graduated);
        return student;
    }
}

public class Main{
    public static void main(String []args){
        Student student = Student.builder()
          .firstName("강")
          .lastName("동민")
          .graduated(true)
          .build();
       
        System.out.println(student.toString());
    }
}
```
