# Strategy Pattern

<h4 align="center"><I>"한줄 요약 "</I></h4>
<h6 align="center">스트래티지 패턴은 같은 문제를 해결하는 여러 알고리즘이 클래스별로 캡슐화되어 있고 이들이 필요할 때 교체할 수 있도록 함으로써 동일한 문제를 다를 알고리즘으로 해결할 수 있게 하는 디자인 패턴</h6>


## 로봇 만들기

아래와 같이 각각 다른 공격과 이동기능이 있는 아톰과 태권 V를 만들어 보자
<p align="center">
  <img src="http://www.plantuml.com/plantuml/png/VP513u8W583l_0gEkiRcMYSFpVv0XshTNygSGtWEdlRt4v8e3fsUVfz-m6WSWQLPZyofmKnWGNE3dNHoueVg2remoiDznLWxWdFyWW-axVa6KZtHtBVN4w5cB48kvac8u_Q3Tx-4XS5hBWneML_93gR_m5tC5C7ojowJxoMTLrEKqIK5HD0gRnOSkiGrBZBIjCy_YwLFycXIUJUSr5C_irvKMIlWlDD8qwqNz080" />
</p>

* 로봇
* 아톰 : 공격(주먹), 이동(날기 O, 걷기 O)
* 태권 V : 공격(미사일), 이동(날기 X, 걷기 O) 

위의 클래스 다이어그램에서 **Atom, TaekwonV 라는 클래스는 Robot 이라는 추상 클래스의 자식 클래스로 설정** 했다. 이렇게 설계한 이유는 아톰과 태권브이 모두 이동과 공격 메서드가 있는 로봇의 한 종류 이기 때문이다. 또한 아톰과 태권브이의 **이동, 공격 기능이 서로 다르기 때문에 Robot 클래스에서 이동과 공격 메서드를 추상 메서드로 설정해 자식 클래스에서 각각 정의**하도록 하였다.

```java
코드 들어갈 예정
```

하지만 위와 같은 방식은 다음과 같은 변경 작업을 할때 문제점이 발생한다.
* 기존 로봇의 공격 또는 이동 방법을 수정할때
* 새로운 로봇을 만들어 기존의 공격 또는 이동 방법을 추가하거나 수정할때

### 1. 기존 로봇의 공격과 이동 방법을 수정하는 경우
우선 첫번째 변경 작업을 살펴보자
> 기존 로봇의 공격 또는 이동 방법을 수정할때

```java
코드 들어갈 예정
```

만약 아톰이 날수 없고 오직 걷게만 만들고 싶다면 TaekwonV 클래스의 move 메서드를 수정해야 한다. 하지만 **이는 새로운 기능를 추가하기 위해 기존의 코드를 변경해야 하므로 OCP를 위반**하게 된다. 또한 위처럼 수정하게 되면 아톰, 태권브이 모두 걷는다는 동일한 기능을 실행하기 때문에 **Atom 클래스의 move와 TaekwonV의 클래스의 move 메서드가 동일한 기능을 실행하므로 기능이 중복되는 상황이 반복된다.** 따라서 지금처럼 로봇의 종류가 2가지 일때는 문제가 없지만 로봇의 종류가 다양해지면 중복 코드를 일관성 있게 유지 관리하는 일이 매우 어렵게 된다.

### 2. 새로운 로봇에 공격/이동 방법을 추가/수정하는 경우
현재 설계한 클래스 같은 경우는 로봇 자체가 캡슐화 단위 이므로 새로운 로봇을 추가하기가 매우 쉽다. 만약 새로운 로봇인 타요에 태권브이의 미사일 공격을 넣는다면? TaekwonV 클래스와 Tayo 클래스의 attack 메서드가 중복해서 사용되게 된다.
 
또한 로봇의 이동과 공격 기능은 계속해서 개발될 것이기 때문에 새로운 방식의 이동기능과 공격기능을 로봇에게 제공하려면 현재 시스템에서는 관련된 모든 기능을 수정해야하는 불상사가 발생하게 된다.

## 해결책

### 무엇이 변화하였는가?
로봇 예제에서 변화되면서 문제를 발생시키는 요인은 **로봇의 이동 방식과 공격 방식의 변화**이다 따라서 기존의 로봇이나 새로운 로봇이 이러한 기능을 별다른 코드 변경 없이 제공받거나 기존의 공격이나 이동방식을 다른 공격이나 이동 방식으로 쉽게 변경할 수 있어야 한다.
> 무엇이 변화되었는지를 찾은 후에 이를 클래스로 캡슐화 한다.

<p align="center">
  <img src="http://www.plantuml.com/plantuml/png/RP6xReGm44LxVyMKYQ8Waf8Y8aqwf4XRR3qoiwpbXv4VIFpzYYrOFBWzSvWx5_SnOKZP6X6rSZC6jE3yI95c-6eFCA3J6_nkXL0kKRYX9FXD2QM-f829flKm6FoYtBGFfC4OuJyxUMTiK34gGunUqUZpztzcFK9HfaC77_WaR-yTB5wD8CepDiFwXLvpmOBEHfbPsL2Kgjt06dAbikGppqtpmtyktrErzaPCOm_2QzMbdjoOhNa0" />
</p>

클라이언트에서는 연관 관계를 이용해 이동 기능과 공격 기능의 변화를 포함시킨다. Robot 클래스가 이동기능과 공격 기능을 이용하는 클라이언트 역할을 수행하고, 이클래스는 변화를 처리하기 위해 MovingStrategy와 AttackStrategy 인터페이스를 포함해야 한다.

<p align="center">
  <img src="http://www.plantuml.com/plantuml/png/VP8nJyCm48Lt_mgFgL2g2Z5bGEs2n524A8BvcXx5IcnNzgMe_7hYu3Rn9ShIvjvtVhvdtTeJE6fqBNobp0aSQ6di0JsUvCDg83emLH3lXs9PW_SR8gVs3U5pQSrE_Q9S2I6K8NHVKA9iEPJLZXmG7Yy3iBLdPOutq9d9ryQKtqpRAkzLZKpzXBeQdt-gBsFnpUujnUztmEh7cezORiXg8QwYpFF7s1t0eEn-Gyt7xW4B6aaXSmCQYrPlZzt4k-kLMKVbFrxq_8Zqqf9iKKX-5BgB0ZbRTUXrRSyuLZrIEdAcax9WYQOCqcWP96N1aaU3_bLdudRF77Zuvoor7IUN8fsSlu4KWYt9k2Fiown3NCYP6U9sphGmaxmvBPTyNxZFT1TF5DluLl9OqqrhD8T6rty0" />
</p>

* Robot 클래스의 입장에서 보면 구체적인 이동 방식과 공격 방식이 MovingStrategy와, AttackStrategy 인터페이스에 의해 캡슐화 되어 있기 때문에 추후에 새로운 이동 방식과 공격방식의 변화뿐만 아니라 현재 변화도 잘 처리할 수 있게 된다.

* 새로운 공격 방식이 개발되어 추가된다고 하더라도 AttackStrategy 인터페이스가 변화에 대한 일종의 방화벽 역할을 수행해 Robot 클래스의 변화를 차단해준다.(OCP를 만족하는 설계)
