# MYSQL 아키텍쳐

![image](https://github.com/user-attachments/assets/86695480-03bc-47b9-9d07-2ddcb047f110)
## 클라이언트
![image](https://github.com/user-attachments/assets/40313ffc-5834-40f8-afec-954b25eb9fe1)

첫 번째는 MYSQL 접속을 위한 클라이언트입니다. 대부분의 프로그래밍 언어에 대해 접속 API를 제공합니다. 그리고 셀에서도 셀 스크립트를 통해 MYSQL에 접속해서 사용할 수 있습니다.
## MYSQL 엔진
![image](https://github.com/user-attachments/assets/59e08cca-2525-4d50-bb7e-cda1eab8706b)

다음은 MYSQL의 엔진입니다. 엔진은 클라이언트 요청과 SQL 요청을 처리합니다. 엔진은 퀴리파서, 전처리기, 옵티마이저, 실행 엔진 등으로 이루어져 있습니다. 이중 옵티마이저는 SQL 문을 최적화하고 실행계획을 짜는 데 중요한 역할을 합니다.
## MYSQL 스토리지 엔진
![image](https://github.com/user-attachments/assets/743f342d-4660-425c-b93e-dcd0eb24b764)

다음은 SQL 스토리지 엔진입니다. 스토리지 엔진은 데이터를 디스크에 저장하거나 디스크에서 데이터를 읽어오는 역할을 담당합니다. MYSQL 엔진은 옵티마이저가 짠 실행계획에 따라서 스토리지 엔진을 적절히 호출해서 쿼리를 실행합니다. 엔진이 스토리지 엔진을 호출할 때 사용하는  API를 핸들러 API라고 합니다. 핸들러 API는 내 입맛대로 코딩해서 사용하는 것이 가능하다고 합니다.
## 운영체제와 하드웨어
![image](https://github.com/user-attachments/assets/31b9ac40-0e0d-4bc8-a60f-8e57f2af3f32)

마지막으로 실제 테이블 데이터와 로그 데이터를 저장하는 운영체제와 하드웨어 부분으로 나눠볼 수 있습니다.
## MYSQL 엔진 상세
![image](https://github.com/user-attachments/assets/d2ede37d-8c81-4611-a14e-230efd4060dc)

MYSQL 엔진이 어떻게 동작하는지 알아보겠습니다. 사용자가 SQL 요청을 보내면 가장 먼저 퀴리 캐시를 만납니다. 쿼리 캐시는 쿼리요청결과를 캐싱하는 모듈입니다. 즉 동일한 SQL의 결과를 더 빠르게 받을 수 있습니다. 사용하는 테이블이 바뀔 때마다 캐시를 비워줘야 하는데, 이때 캐시에 접근하는 스레드에 락이 걸리게 됩니다. 이는 동시처리 성능저하를 유발하여 MYSQL 8.0부터는 쿼리 캐시가 삭제되었습니다.

다음은 쿼리 파서입니다. 파서는 기본적인 SQL 문장 오류를 체크합니다. 그리고 SQL의 문장을 의미 있는 단위의 토큰으로 쪼갠 다음에 파스 트리를 만들고, 파스 트리를 사용해서 쿼리를 실행합니다.

다음은 전처리기입니다. 전처리기는 파스 트리를 기반으로 쿼리 문장에 구조적인 문제점이 있는지 검사합니다. 토큰을 하나하나 체크하면서 실제로 존재하는 값인지 보고 접근권한 같은 것도 같이 체크합니다.

다음은 옵티마이저 입니다. 옵티마이저는 SQL 문을 최적화해서 실행시키는 쿼리 실행 계획을 만듭니다. 옵티마이저가 최적화를 하는 방법에는
규칙 기반 최적화 : 옵티마이저에 내장된 우선순위에 따라 실행
비용 기반 최적화 : 작업의 비용과 대상 테이블의 통계 정보를 활용해서 실행 계획을 수립합니다.
가 있습니다.

쿼리 실행 엔진은 옵티마이저가 만든 실행 계획대로 스토리지 엔진을 호출해서 레코드를 읽고 쓰는 동작을 합니다.
스토리지 엔진은 쿼리 실행 엔진이 요청한 대로 데이터를 디스크로 저장하고 읽습니다.
핸들러 API에 의해서 동작합니다.

![image](https://github.com/user-attachments/assets/a3a155f3-6a4e-408e-a448-7b2db666b7b2)
![image](https://github.com/user-attachments/assets/995bcac4-5cff-44ec-aae7-805c01b6f1a0)
# InnoDB 엔진 주요 특징
- InnoDB는 PK를 기준으로 클러스터링
- 따라서, 다른 보조 Index보다 PK를 우선적으로 탐색하는 경향이 존재
- Oracle의 아키텍쳐 적용
- MVCC(Multi version Concurrency Control) 및 테이블 스페이스 등 오라클에서 주로 사용되는 개념들이 적용됨
- MVCC : 락을 걸지않고 작업을 수행함으로써, 다른 트랜잭션이 갖고있는 락을 기다리지않고 읽기작업이 가능
- InnoDB 스토리지 엔진은 트랜잭션과 격리 수준을 보장하기 위해 DML로 변경되기 이전 데이터를 백업한다. 이러한 변경 방식을 mvcc라고 한다.


![image](https://github.com/user-attachments/assets/7cba5a57-aa7c-47c0-abaf-be59bffa4c2d)
## InnoDB Adaptive Hash Index
InnoDB에서는 앞서 언급한 상황을 해결하기 위해, InnoDB Adative Hash Index 기능이 있습니다. 자주 사용되는 칼럼을 해시로 정의하여, B-Tree 를 타지 않고 바로 데이터에 접근할 수 있는 기능이죠. “Adaptive”라는 단어에서 예상할 수 있겠지만, 모든 값들이 해시로 생성이 되는 것이 아니라, 자주 사용되는 데이터 값만 내부적으로 판단하여 상황에 맞게 해시 값을 생성합니다.

![image](https://github.com/user-attachments/assets/4b12c38f-81c2-41f2-8aee-c6fd61f7896a)
![image](https://github.com/user-attachments/assets/ab439605-e12d-4012-91d1-e8596a2eda42)
![image](https://github.com/user-attachments/assets/79ed3ef5-aa4e-4378-8a54-69bb2aaef9b9)
![image](https://github.com/user-attachments/assets/b6337bef-c26e-4eb7-adb4-5172a2fd1760)
