### OCP
- 템플릿
  - 코드 중에서 변경이 거의 일어나지 않으며 일정한 패턴으로 유지되는 특성을 가지는 부분을 자유롭게 변경되는 성질을 가진 부분 (콜백) 으로부터 `독립` 시켜서 효과적으로 활용할 수 있도록 하는 방법

> 템플릿 메소드 패턴 <br/>
> 템플릿 메소드 패턴은 고정된 틀의 로직을 가진 템플릿 메소드를 슈퍼클래스에 두고, 바뀌는 부분을 서브클래스의 메소드에 두는 구조로 이뤄진다. 

> 콜백
> 콜백은 실행되는 것을 목적으로 다른 오브젝트의 메소드에 전달되는 오브젝트 <br/>
> 파타미터로 전달되지만 값을 참조하기 위한 것이 아니라 특정 로직을 담은 메소드를 실행시키는것이 목적이다. <br/>
> 하나의 메소드를 가진 인터페이스 타입 (SAM) 의 오브젝트 또는 람다 오브젝트 이다  <br/>

### 템플릿/콜백은 전략 패턴의 특별한 케이스
- 템플릿은 전략 패턴의 컨텍스트
- 콜백은 전략 패턴의 전략
- 템플릿/ 콜백은 메소드 하나만 가진 전략 인터페이스를 사용하는 전략 패턴

### 메소드 주입
- 의존 오브젝트가 메소드 호출 시점에 파라미터로 전달되는 방식이다.
- 의존 관계 주입의 한 종류
- 메소드 호출 주입(method call in injection) 이라고 한다.

### 현재의 WebApiExRateProvider 의 구성 <br/>
- URI 를 준비하고 예외처리를 위한 작업을 하는 코드
- API 를 실행하고, 서버로부터 받은 응답을 가져오는 코드
- JSON 문자열을 파싱하고 필요한 환율정보를 추출하는 코드

### 변경할 여지가 있는 코드
- URI 를 준비하고 예외처리를 위한 작업을 하는 코드
  - API 로부터 환율 정보를 가져오는 코드의 기본틀 (잘 바뀌지 않으려는 성질)
- API 를 실행하고, 서버로부터 받은 응답을 가져오는 코드 
  - API 를 호출하는 기술과 방법이 변경될 수 있다. (변경될 여지가 있는 성질)
- JSON 문자열을 파싱하고 필요한 환율정보를 추출하는 코드
  - API 응답의 JSON 구조에 딸아 정보를 추출하는 방식이 변경될 수 있다. (변경될 여지가 있는 성질)

### 위 내용을 대응하기 위해 다음과 같은 순서로 해결해 볼 수 있을것 같다.
- 1. 메서드로 추출한다.
- 2. 추출한 실행 메서드를 인터페이스로 분리한다
- 3. 추출한 메서드의 로직을 인터페이의의 구현체로 구현한다.

### ApiTemplate
- 환율 정보 API 로 부터 환율을 가져오는 기능을 제공하는 오브젝트
- API 호출과 정보 추출의 기본틀 제공
- 두 가지 콜백을 이용
- 유사한 여러 오브젝트에서 재사용 가능