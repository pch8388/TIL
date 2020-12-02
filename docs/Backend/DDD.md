# 도메인 주도 개발
- 도메인 : 소프트웨어로 해결하고자 하는 문제 영역. 즉, 업무를 영역짓는 것
- 도메인 모델 : 도메인을 소프트웨어로 표현하기 위해 모델링
  - 소프트웨어 개발자가 해당 도메인에 대해 처음부터 완벽하게 이해하고 구현하기 힘듬
  - 순수하게 도메인에 대해 모델링 하는 개념적인 도메인 모델을 실제 구현에 모두 반영할 수 없음
  - 기술과의 융합이나 구현상의 이점등을 고려하여 개념모델에 가깝게 구현모델을 가져가야 함

## 도메인 모델 패턴
- 일반적인 애플리케이션은 4계층의 아키텍처로 구성
  1. Presentation layer : 클라이언트와의 인터렉션
  2. Application layer : 클라이언트 요청 기능 실행, 도메인 모델을 조합하여 사용, 외부 라이브러리 등의 도메인 외적인 기능들을 이 계층에서 사용함
  3. Domain layer : 도메인 기능을 실행할 계층
  4. Infrastructure : 외부 시스템과의 연동, DB 나 메시징 시스템 등등
- 각 계층은 하위 계층으로만 의존해야 함(상위 계층일수록 클라이언트와 가까움)
> 여기서 클라이언트라 함은 꼭 사용자만을 이야기하는 것이 아니라, http 를 포함한 모든 외부 요청을 말함
- 히자만, 구현을 하다보면 하위계층에 대한 의존성이 생기기 쉬움.
  - 예를 들어, 스프링으로 개발을 하다보면 service 계층에서 persistence 계층의 기술인 특정 데이터베이스 기술에 의존하는 경우가 많음(사실 영속성을 관계형 데이터베이스로 가져간다라고 규정하는 것 자체가 의존하는 것이다)
  - DIP (Dependency Inversion Principle) : 저수준 모듈이 아닌 고수준 모듈에 의존하도록 함으로써 이를 해결
  - service -> repository(interface) 를 의존시키고, 구체화된 기술은 repository interface 를 구현하도록 하여 해결

## 도메인 모델링
1. 요구사항 정리
  - 요구사항을 원하는 형태로 정리
  - 문장이나 순서도, 워크플로우 등을 이용
2. 요구사항에 따른 모델을 정의
  - 추상적으로 이름만 정할수도 있고, 좀 더 구체화 할 수도 있음
  
### Entity
- 엔티티는 식별자를 갖는다는 점에서 value 와 차이가 있음

### Value
- 엔티티가 가질 수 있는 값의 의미를 좀 더 잘 드러내고자 할 때 사용가능 (도메인 개념을 나타낼 수 있음)
- 불변 객체로 사용해야 함
- 밸류 타입을 위한 기능 추가 가능(ex. Money 객체라면 돈에 관련된 계산 로직을 넣을 수 있음)

### 도메인 용어
- 도메인 용어는 통일되고 의미가 명확해야 함

### 도메인 영역의 주요구성 요소
- 엔티티 : 고유식별자, 라이프사이클을 가짐, 도메인 고유의 개념을 표현
- 밸류 : 도메인 객체의 속성 표현
- 애그리거트 : 엔티티 + 밸류를 개념적으로 하나로 묶음
- 리포지토리 : 도메인 모델의 영속성 처리
- 도메인 서비스 : 도메인 로직이 여러 도메인을 사용해야 할 때 사용