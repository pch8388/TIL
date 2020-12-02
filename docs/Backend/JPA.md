# 영속성 관리
## EntityManagerFactory
- DB 하나당 하나가 생성된다고 생각하면 됨
- EntityManager 를 생성
- Thread safe

## EntityManager
- Transaction 당 하나 생성한다고 생각하면 됨
- Trread safe 하지 않음

## 영속성 컨텍스트
- 엔티티를 영구 저장하는 환경
- EntityManager 를 이용하여 영속성 컨텍스트에 접근

### Entity lifecycle
1. 비영속 : 영속성 컨텍스트와 관계 없는 상태
2. 영속 : 영속성 컨텍스트에 저장된 상태 (persist, merge, find)
3. 준영속 : 영속성 컨텍스트에 저장되었다가 분리된 상태 (detach, clear, close)
4. 삭제 : 삭제된 상태(remove)

### 장점
- 1차캐시
- 동일성 보장 (같은 엔티티면 같은 주소의 레퍼런스)
- 트랜잭션을 지원하는 쓰기 지연
- 변경 감지
- 지연 로딩

### flush
- 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영
- 호출하는 방법
  1. flush() 직접호출
  2. 트랜잭션 커밋시 자동호출
  3. JPQL 쿼리 실행시 자동호출
    - JPQL 쿼리는 데이터베이스에 직접 쿼리를 실행하기 때문에 플러시 되지 않으면 영속성 컨텍스트의 내용이 적용되지 않은 채 조회하게 됨