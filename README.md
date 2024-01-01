# 📕 스프링부트 - Spring boot와 Java를 활용하여 Account(계좌 관리) 시스템을 만드는 프로젝트

## 🔧 기술 스택

- Java 11
- Spring Boot 2.6.8
- Spring Boot Data JPA
- Spring Boot Validation
- Redisson
- Embedded Redis
- H2DB

## 📖 API명세서

### 계좌 관련 API

- 계좌 생성
- 'POST' 요청을 사용해서 새 계좌를 등록할 수 있다.
  - 파라미터 : 사용자 ID(userId), 초기 잔액(initialBalance)
  - 결과
    - 실패: 사용자 없는 경우, 계좌가 10개(사용자당 최대 보유 가능 계좌)인 경우 실패 응답
    - 성공: 생성된 계좌번호(10자리 랜덤 숫자), 사용자 아이디, 등록일시

- 계좌 해지
- 'DELETE' 요청을 사용해서 기존 계좌를 해지할 수 있다.
  - 파라미터 : 사용자 ID(userId), 계좌번호(accountNumber)
  - 결과
    - 실패 : 사용자 없는 경우, 사용자 아이디와 계좌 소유주가 다른 경우, 계좌가 이미 해지 상태인 경우, 잔액이 있는 경우 실패 응답
    - 성공 : 사용자 아이디, 계좌번호, 해지일시
    
- 계좌 확인
- 'GET' 요청을 사용해서 사용자 계좌를 조회할 수 있다. 
  - 파라미터 : 사용자 ID(userId)
  - 결과
    - 실패 : 사용자가 없는 경우 실패 응답
    - 성공 : (계좌번호, 잔액) 정보를 Json list 형식으로 응답
  

### 거래 관련 API

- 잔액 사용
- 'POST'요청을 사용해서 잔액 사용을 등록할 수 있다.
  - 잔액 사용
  - 파라미터 : 사용자ID(userId), 계좌번호(accountNumber), 거래 금액(amount)
    - 결과
    - 실패 : 사용자가 없는 경우, 사용자 아이디와 계좌 소유주가 다른 경우, 계좌가 이미 해지 상태인 경우, 거래금액이 잔액보다 큰 경우, 거래금액이 너무 작거나 큰 경우
    - 성공 : 계좌번호, transaction_result, transaction_id, 거래금액, 거래일시

- 잔액 사용 취소
- 'POST' 요청을 사용해서 잔액 사용 취소를 등록할 수 있다.
  - 파라미터 : transactionId, 계좌번호(accountNumber), 거래 금액(amount)
  - 결과
    - 실패 : 원거래 금액과 취소 금액이 다른 경우, 트랜잭션이 해당 계좌의 거래가 아닌 경우
    - 성공 : 계좌 번호, transaction_result, transaction_id, 취소 거래금액, 거래 일시

- 거래 확인
- 'GET' 요청을 사용해서 사용자 계좌를 조회할 수 있다.
  - 파라미터 : transactionId
  - 결과
    - 실패 : 해당 transaction_id 없는 경우
    - 성공 : 계좌번호, 거래종류(잔액 사용, 잔액 사용 취소), transaction_result, transaction_id, 거래금액, 거래일시
      - 성공 거래 뿐 아니라 실패한 거래도 거래 확인 할 수 있어야 한다.
