# [Clean Code] 3. 함수

- 들어가기 앞서..
    
    ### - 함수는 한 가지를 해야 한다.
    ### - 그 한 가지를 잘 해야 한다.
    ### - 그 한 가지만을 해야 한다.
    
    이번 챕터에서 가장 중요하다 생각하는 문구이다.
    
    함수란 한번에 한가지만 수행하도록 코드를 짜야 잘 짠 코드이다.
    
    장점은 여러가지라고 생각한다. 
    
    (이해하기 쉽고, 가독성이 높으며, 객체지향적이고, 유지보수가 용이해진다고 생각함.)
    

### 1. SOLID 원칙

- **SRP** : 단일 책임 원칙
    
    한 클래스는 하나의 책임만을 가져야한다.
    
    - 변경에 의한 연쇄 작용으로 부터 자유로우며 가독성 향상과 유지보수가 용이해진다.
    - 클래스의 단일 책임 원칙
- **OCP** : 개방-폐쇄 원칙
    
    소프트웨어 요소는 확장에는 열려있으나 변경에는 닫혀있어야 한다.
    
    - 변경을 위한 비용은 줄이고 확장을 위한 비용은 가능한 극대화해야한다.
    - 요구사항에 변경 혹은 추가가 발생할 시, 기존 요소는 수정하지 않고 확장하여 재사용한다.
    - 객체지향의 추상화와 다형성을 활용한다.
- **LSP** : 리스코프 치환 원칙
    
    서브 타입은 언제나 기반 타입으로 교체할 수 있어야 한다.
    
    - 서브 타입은 기반 타입이 약속한 규약(접근제한자,예외포함)을 지켜야 한다.
    - 클래스 혹은 인터페이스 상속을 통하여 확장성을 획득한다.
    - 다형성과 확장성을 극대화하기 위하여 인터페이스를 사용하는 것이 좋다.
    - 합성(한번에 여러 인터페이스를 확장)을 이용할 수 있다.
- **ISP** : 인터페이스 분리 원칙
    
    자신이 사용하지 않는 인터페이스는 구현하지 말아야 한다.
    
    - 가능한 최소한의 인터페이스만 구현한다.
    - 만약 어떤 클래스를 이용하는 클라이언트가 여러개이고, 이들이 해당 클래스의 특정 부분만을 사용한다면 해당 클래스를 여러 인터페이스로 분리하여 클라이언트에게 필요한 기능만 제공한다.
    - 인터페이스의 단일 책임 원칙
- **DIP** : 의존성 역전 원칙
    
    상위 모델은 하위 모델에 의존하면 안된다. 
    
    둘 다 추상화에 의존하여야 한다.
    
    추상화는 세부 사항에 의존하여서는 안된다. 세부 사항은 추상화에 따라 달라진다.
    
    - 하위 모델의 변경이 상위 모듈의 변경을 요구하는 위계관계를 끊는다.
    - 실제 사용관계는 그대로이지만, 추상화를 매개로 메시지를 주고받으며 관계를 느슨하게 만든다.
    
    
    

```java
// 이해하기 쉽게 예제로 살펴보기
// 1. 확장에 유연하지 않고 하위 모델에 의존한 코드
class PaymentController{
		@RequestMapping(value = "/api/payment", method = RequestMethod.POST)
		public void pay(@RequestBody ShinhanCardDto.PaymentRequest req){
				shinhanCardPaymentService.pay(req);
		}
}

class ShinhanCardPaymentService {
		public void pay(ShinhanCardDto.PaymentRequest req){
				shinhanCardApi.pay(req);
		}
}
```

위 코드의 경우 하위 모델에 의존한 코드이기 때문에 만약 새로운 카드사가 추가되어 확장이 필요하다면, PaymentController의 pay 함수에서 if/else 문을 통하여 각각 다른 서비스를 호출해주어야함.

때문에, 의존성 역전 원칙에 의해 추상화 된 인터페이스 하나를 생성하여 추상화된 인터페이스에 의존하도록 소스를 수정하여야함.

```java
// 2. 의존성 역전 원칙에 의해 새로운 인터페이스 생성하여 소스 변경
// 새 인터페이스 생성 후 PaymentController의 pay함수는 인터페이스의 pay함수 호출
// 각 카드사의 pay 함수 호출은 해당 인터페이스를 구현받아 재정의된 함수로 실행됨.
class PaymentController{
		@RequestMapping(value = "/api/payment", method = RequestMethod.POST)
		public void pay(@RequestBody CardPaymentDto.PaymentRequest req){
				final CardPaymentService cardPaymentService = cardPaymentFactory.getType(req.getType());
				cardPaymentService.pay(req);
		}
}

public interface CardPaymentService {
		void pay(CardPaymentDto.PaymentRequest req);
}

public class ShinhanCardPaymentService implements CardPaymentService {
		@Override
		public void pay(CardPaymentDto.PaymentRequest req){
				shinhanCardApi.pay(req);
		}
}
```

### 2. 간결한 함수 작성

- 함수는 최대한 작게 쪼개고, 함수 내 추상화 수준이 동일하도록 작성하여야한다.
- 실제 연산 부분과 타입별 분기처리 부분을 분리하여 타입에 대한 분기 처리는 Factory 함수 내에서 처리하도록 분리하여야 한다.
- 함수의 인수 부분은 특별한 경우가 아니라면 0 ~ 2개가 적당하다.

### 3. 안전한 함수 작성하기

- 안전한 함수란 당연한 소리지만 Side Effect가 없는 함수이다.
- Side Effect 부수효과란 ? 값을 반환하는 함수가 외부 상태를 변환하는 경우를 말함..
- 함수와 관계없는 외부 상태를 변경하는 일은 없도록 해야합니다!!!!

### 4. 함수 리팩토링하기

- 함수 작성의 순서
    1. 기능을 구현하는 서투른 함수를 작성한다. ( 길고 복잡하고 중복도 있는 상태, 기능만 동작 )
    2. 테스트 코드를 작성한다. ( 함수의 분기 혹은 엣지 값마다 빠짐없이 테스트 가능한 코드작성 )
    3. 리팩토링 한다. ( 코드 다듬고, 쪼개고, 이름 바꾸고, 중복을 제거한다 )
    
    !! 보통 1 → 2 까지만 작업하고 3은 1에서 같이 작업하거나 작업하지 않았는데.. 
    
    제일 중요한 부분인 것 같다.. ㅠ 
    
    선 함수 작성 후 리팩토링 과정을 잊지말자!
    
- 원본 노션 링크 : https://www.notion.so/Clean-Code-3-c769ff015ede4f5e98e52a82ba87b901
