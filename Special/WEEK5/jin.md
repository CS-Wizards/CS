# MYSQL 아키텍쳐

![image](https://github.com/user-attachments/assets/86695480-03bc-47b9-9d07-2ddcb047f110)
![image](https://github.com/user-attachments/assets/d2ede37d-8c81-4611-a14e-230efd4060dc)

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
