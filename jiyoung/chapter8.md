# [Clean Code] 8. 경계

### 경계?

이번 챕터의 주제는 경계이다.

코드에 대한 주제일텐데 코드에서의 경계란 무엇이고 왜 필요할까?

하나의 프로젝트에서 오픈소스 혹은 라이브러리를 쓰지 않고 프로그래밍을 하는 프로젝트는 없다.

해당 챕터에서 말하는 경계는 외부 소스와 직접 구현한 내부 소스 간의 경계를 말한다. 

우리가 만드는 코드와 외부에서 들어온 코드가 병합되게 되는데 외부코드는 외부에서 만든 코드로 보통 외부시스템을 호출하거나 혹은 단순히 외부에서 만들어진 코드이다.

상세하게 해당 코드의 내용까지 알 필요가 없기 때문에 우리 코드와 외부코드를 깔끔하게 통합하여 가독성을 높이기 위해서는 **모호한 경계를 잘 구분지어야 한다!!**

### 코드의 경우에 따라 경계를 구분짓는 방법!

1. 우리 코드를 보호하며 경계짓기 >> **"캡슐화"** 사용하여 정보 은닉하기.
우리코드의 자세한 로직은 숨기고 필요한 기능에 대한 정보만 오픈하도록 우리 코드를 캡술화 하여 보호하여야 합니다.

  **캡슐화란?**
  객체의 실제 구현을 외부로부터 감춰서 해당 객체 내의 값이나 기능을 사용하기 위해서는 캡슐화된 클래스에서 제공하는 메소드를 통해서만 접근해야합니다. ( getter,  setter등)

2. 외부코드와 호환되는 경우 >> **"Adapter Class 이용하기."**
외부코드를 사용하는 경우, 해당 코드를 우리가 사용하는 방식에 맞춰 중간 단계에 Adapter Class를 구현해줍니다. 우리가 정의한 인터페이스대로 호출하게 할 수 있습니다.


- Learning Test? 외부 라이브러리를 테스트하는 코드
해당 챕터를 읽고 강의를 들으며 처음 듣는 테스트 클래스였다.
외부 라이브러리나 오픈 소스를 사용하면서 개인적인 용도의 테스트 진행은 하였으나, 해당 테스트 코드를 따로 남겨두거나 하지 않았는데...
오픈소스나 라이브러리에 대한 이해도를 높여주며
해당 외부코드의 버전이 업그레이드 시 내부 소스와의 호환여부 테스트를 위해서라도 작성하면 좋을 듯 하다!

- 원본 노션 링크 : https://www.notion.so/Clean-Code-8-a36447869f92415a87b1199d7517d083
