# 의존성역전의 원칙 (Dependency Inversion Principle, DIP)

<h4 align="center"><I>"한줄 요약 "</I></h4>
<h6 align="center">DIP를 만족하려면 어떤 클래스가 도움을 받을 때 구체적인 클래스보다는 인터페이스나 추상 클래스와 의존관계를 맺도록 설계해야 한다.</h6>

## 의존성역전의 원칙

  ### 변하기 쉬운것?, 어려운것?
  * 정책, 전략과 같은 어떤 **큰 흐름이나 추상적인 것**은 **변하기 어려움**
  * **구체적인 방식, 사물** 등과 같은 것은 **변하기 쉬움**

  ### 예시
  만약 아래와 같이 아이가 장난감을 가지고 노는 경우라면 **아이가 장난감을 가지고 노는 사실은 변하기 어려울 것이다.** 하지만 **아이가 가지고 노는 장난감의 종류는 변하기 쉽다.**
  <p align="center">
    <img src="http://www.plantuml.com/plantuml/png/SoWkIImgAStDuIhEpimhI2nAp5N8oqz9BKujK30nG18iIipB1WaQ6N1n9IOLbnGb9fSel9ctz7NJd5SR9d0bvoGM5okycThoPkuMAEVrmZcljxH3EKglLwruEsCgKCU4r_qptsnOeGHJjQ3ZrUO5N5mGhGgwHPdf6IMwkdP8PaCCKj0jBT3LhK6Mm3GKYoA81LWa75BpKe0U1m00" />
  </p>

> 변하기 어려운 추상적인 것들은 추상 클래스와 인터페이스를 활용

이처럼 DIP를 만족하려면 **어떤 클래스가 도움을 받을 때 구체적인 클래스보다는 인터페이스나 추상 클래스와 의존관계를 맺도록 설계**해야 한다. 

## 의존성 주입(Dependency Injection)
의존성 주입이란 **클래스 외부에서 의존되는 것을 대상 객체의 인스턴스 변수**에 주입하는 기술이다.

### 예시
```java
class Toy {
    ...
}
```
아래와 같이 대상 객체를 변경하지 않고도 외부에서 대상 객체의 외부 의존 객체를 바꿀수 있다.
```java
class Kid {
	private Toy toy;
	
	public void setToy(Toy toy) {
		this.toy = toy;
	}

	public void play() {
		System.out.println(toy.toString());
	}
}
```
새로운 장난감을 가지고 놀고 싶다면 아래와 같이 코드를 작성하면 된다. Kid, Toy등 기존의 코드에 영향을 받지 않고 장난감을 추가할수 있다.
```java
class Robot extends Toy {
    public String toString() {
        return "Robot";
    }
}

class Lego extends Toy {
    public String toString() {
        return "Lego";
    }
}
```

사용할때도 마찬가지로  **외부에서 의존할 객체만 변경**해주면 **대상 객체를 변경하지 않고도 외부에서 대상 객체의 외부 의존 객체를 변경**할수 있다.
```java
public class HelloWorld{

     public static void main(String []args){
        Toy t1 = new Lego();
        Kid k1 = new Kid();
        k1.setToy(t1);
        k1.play();
        // Lego
        
        Toy t2 = new Robot();
        Kid k2 = new Kid();
        k2.setToy(t2);
        k2.play();
        // Robot
     }
}
```

---

### 참고 Plant UML Code

```
@startuml
skinparam nodesep 40
skinparam ranksep 20

abstract 장난감
class 아이
class 로봇
class "모형 자동차"
class 레고

아이 -right--> 장난감
로봇 -up--|> 장난감
"모형 자동차" -up--|> 장난감
레고 -up--|> 장난감
@enduml
```
