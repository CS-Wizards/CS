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

위 사진들은 컴파일러의 실행과정중의 일부 입니다. 위의 MYSQL동작과 유사한점이 있어 이해하는데 도움이 될 것 같아 가져왔습니다. 소스 프로그램을 토큰으로 토큰을 트리로 만들고 트리를 사용해 중간코드를 생성합니다. 파서는 토큰을 받아 파스트리를 생성합니다.


# InnoDB 엔진 주요 특징
- innodb의 핵심 특징으로는 프라이머리 키에 의한 클러스터링, 트랜잭션 지원, 버퍼풀과 어댑티브 해시 인덱스 등이 있습니다.
- innodb는 프라이머리 키 (PK)로 데이터를 정렬하고 불러올때도 pk를 통해서 데이터 파일에 접근을 합니다. pk로 인해서 접근속도가 빨라지지만 클러스터링 떄문에 쓰기 성능이 저하됩니다. (pk값이 바뀌면 다른 데이터들의 위치도 바뀌기 떄문에)
- 사용자가 pk를 정하지 않으면 내부적으로 pk를 생성하는데 이것은 사용자가 사용할 수 없습니다. 그래서 테이블을 설계할 떄는 pk를 직접 설정해주는 것이 좋습니다.
- Secondaray Key는 인데스키+pk를 조합으로 정렬합니다. 즉, 특정 데이터를 찾기 위해서는 sk에서 pk를 찾고, 그 pk를 통해 다시 원하는 데이터로 찾아가는 형태로 데이터가 처리 됩니다.


![image](https://github.com/user-attachments/assets/7cba5a57-aa7c-47c0-abaf-be59bffa4c2d)
## InnoDB Adaptive Hash Index
InnoDB에서는 앞서 언급한 상황을 해결하기 위해, InnoDB Adative Hash Index 기능이 있습니다. 자주 사용되는 칼럼을 해시로 정의하여, B-Tree 를 타지 않고 바로 데이터에 접근할 수 있는 기능이죠. “Adaptive”라는 단어에서 예상할 수 있겠지만, 모든 값들이 해시로 생성이 되는 것이 아니라, 자주 사용되는 데이터 값만 내부적으로 판단하여 상황에 맞게 해시 값을 생성합니다.

![image](https://github.com/user-attachments/assets/4b12c38f-81c2-41f2-8aee-c6fd61f7896a)

![image](https://github.com/user-attachments/assets/ab439605-e12d-4012-91d1-e8596a2eda42)
![image](https://github.com/user-attachments/assets/79ed3ef5-aa4e-4378-8a54-69bb2aaef9b9)
## 스토리지 엔진 특징
버퍼풀은 변경된 데이터가 디스크에 반영되기 전까지 잠시 머무르는 공간이고 언두는 변경되기 이전 데이터를 백업 해두는 공간입니다.
이 상태에서 다른 트랜잭션이 레코드를 조회하면 데이터 베이스에 설정된 트랜잭션 격리 수준에 따라 부르는 데이터가 다릅니다. 격리 수준이 read_uncommitted라면 버퍼풀을 참고하고 그게 아니면 언두로그를 참고합니다. 이렇게 트랜잭션 격리 레벨에 따라서 조회되는 데이터가 달라지게 하는 기술을 MVCC라고 합니다.

![image](https://github.com/user-attachments/assets/b6337bef-c26e-4eb7-adb4-5172a2fd1760)
## In-Memory Structures
![image](https://github.com/user-attachments/assets/35c0d040-8791-4c16-bbc2-eb0cb9130d14)
   
- Buffer Pool

   버퍼풀은 테이블 및 인덱스 데이터를 캐싱하는 메인 메모리 영역입니다. 쓰기 버퍼링 기능을 제공합니다. 즉, 쿼리가 실행됐지만 아직 COMMIT되지 않은데이터들은 버퍼풀에서 들고 있게 됩니다. 쿼리가 실행됐는데, 해당하는 데이터들이 버퍼풀에 없다면 , 디스크에서 읽기/쓰기 해서 가져오게 됩니다.
  
![image](https://github.com/user-attachments/assets/c4a96b73-23f0-4b82-a581-308340178695)

- Change Buffer
 
   보조 인덱스(Secondary Index)의 변화를 캐싱하는 메모리 영역이다. 쉽게 이야기 하면, 어떤 데이터의 인덱싱된 외래키가 새롭게 생기거나 수정되거나 삭제되면 체인지 버퍼에 그 변화를 캐싱한다는 것입니다. 보조 인덱스와 관련된 데이터가 버퍼풀에 없는 상태에서 보조 인덱스에 변화가 생겼을때 이 변화를 캐싱합니다.
  
 ![image](https://github.com/user-attachments/assets/f965bb19-099e-4bc5-ad8b-99130e7ba540)


- Adaptive Hash Index
 
  자주 사용되는 컬럼을 해시로 정의해서 PK기반으로 생성된 TREE를 매번 스캔하지 않고도 데이터를 탐색할 수 있게 해주는 기능입니다. 자주 사용되는 컬럼에 대해서만 해시를 생성하므로, 성능 향상에 도움이 됩니다.

![image](https://github.com/user-attachments/assets/260158da-29a3-4ad3-897c-e15f30357fa9)
  

- Log Buffer

   MYSQL서버에서 실행된 트랜잭션은 Redo log에 록이 되는데, 매번 기록하는 것은 성능상 문제가 될 수 있으므로 Log Buffer에 기록해뒀다가 한꺼번에 Redo log에 기록을 합니다. 즉, log buffer는 redo log의 캐시와 같은 역할을 합니다.
  
## On-Disk Structures
- Tables

   우리가 아는 BD의 테이블들을 이야기합니다. 테이블 정보는 .frm파일에 저장해서 관리 합니다.
  
-  Tablespaces


   테이블 스페이스는 시스템 테이블 스페이스, File-per-table 테이블스페이스, 임시 테이블스페이스 등등이 있습니다.

   시스템 테이블스페이스 : 시스템 전체에 대한 정보가 저장되는 파일입니다. 데이터 Dictionary, Undo Log와 같은 것들을 저장합니다.

   File-per-table 테이블스페이스 : 이름에서도 유추 할 수 있듯이 DB에 생성되어 있는 테이블 각각의 정보를 기록하는 파일입니다. 데이터와 인덱스 정보가 기록이 되고, .idb확장자를 갖습니다.
-   Doublewrite Buffer

    버퍼풀에서 데이터가 Flush되어 디스크에 저장될 때, 이중 쓰기 버퍼에도 데이터가 같이 기록이 됩니다.
    쉽게 얘기하면, 데이터의 복사본이라고 생각할 수 있고, 이는 예상치 못한 장애가 생겼을 때 안전하게 데이터를 복구하기 위함입니다.


- Redo Log

    트랜잭션의 내용을 기록하는 파일입니다.
    혹시나 MySQL서버에 장애가 생겨서 MySQL서버의 메모리에 저장됐던 정보들이 모두 사라지면, Redo Log를 참고하여 장애 이전 시점으로 복구합니다.


- Undo Logs

    어떤 트랜잭션이 수행되기 이전의 데이터를 기록하는 파일입니다.
    트랜잭션을 수행하고 해당 트랜잭션을 취소하는 상황처럼 트랜잭션이 수행되기 이전의 데이터가 필요한 경우를 위해서 존재합니다.
