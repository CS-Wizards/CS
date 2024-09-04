## JPA

### SQL 중심적인 개발

지금 시대는 객체를 관계형 데이터베이스에 저장하고 있다. 우리는 프로그래밍을 자바로 하는 반면에, 데이터베이스는 SQL 언어만 알아들을 수 있다. 따라서 **SQL에 의존적인 개발**이 이루어질 수 밖에 없는데, 여기에는 많은 문제점이 존재한다.

예를 들어 아래와 같은 객체가 존재하고, INSERT 구문이 있다고 생각해 보자.

```java
public class Member {
	private String name;
}
```

```sql
INSERT INTO MEMBER(NAME) VALUES("김땡땡");
```

이 객체에 대한 CRUD를 구현하려면 INSERT INTO, UPDATE, DELETE, SELECT, 자바 객체 → SQL, SQL → 자바 객체의 과정을 거쳐야 한다.

즉 개발자가 서비스 로직이 아닌 데이터 변환과 SQL 매핑만 반복하는 지루한 일을 해야 한다. 이건 양반이다. 만약 객체가 아래처럼 변한다고 가정해 보자.

```java
public class Member {
	private String name;
	private int age;
}
```

이런 경우 Member에 관한 모든 SQL을 수정해야 하며, 이에 대한 오류가 발생하기도 쉽다.

### 객체 ≠ 관계형 데이터베이스

우리는 관계형 데이터베이스에 객체를 저장하고 있지만, 엄밀히 따지자면 둘은 완벽하게 일치하다고 할 수 없다.

> 객체 지향 프로그래밍은 **추상화**, **캡슐화**, **정보 은닉,** **상속**, 다형성 ****등 시스템의 복잡성을 제어할 수 있는 다양한 장치들을 제공한다.
> 

하지만 데이터베이스는 그저 정보만 저장할뿐, 객체로서의 활용 능력은 제로이다. 그 근거는 아래와 같다.

- 상속
    
    ![](https://velog.velcdn.com/images/morion002/post/1b302195-74d7-401e-82df-b51a91e78b93/image.png)

    
    만약 우리가 `Album`을 저장한다면  `List<Item> items.add(new Album())` 의 코드만 실행하면 된다. **다형성이 지원**되기 때문이다. 하지만 SQL의 경우 `Album`을 `Item`으로 분해하고, `Album`으로 분해해서 따로따로 테이블에 저장해 둬야 한다.
    
    INSERT 제외 SELECT, UPDATE 등등… 매우 귀찮다. 결국… SQL을 짜기 귀찮아서 DB에 저장할 객체에는 상속 관계를 쓰지 않게 된다. 데이터베이스에 이렇게 종속적인 코드는 객체지향의 장점을 살렸다고 말하기 어렵다.
    
- 연관관계
    
    ![](https://velog.velcdn.com/images/morion002/post/5007e1b7-1db7-46f2-8703-e00f3e7c108f/image.png)

    
    객체의 경우 `member.getTeam()`을 통해 쉽게 접근할 수 있다. 그러나 데이터베이스의 경우 `JOIN ON MEMBER.TEAM_ID = TEAM.TEAM_ID`로 조인을 해야 한다.
    
    따라서 원래 객체지향적이라면 이랬어야 할 코드가
    
    ```java
    public class Member {
    	private String name;
    	private Team team;
    }
    ```
    
    SQL 작성이 번거롭고 반복 작업이 지겨워서 아래처럼 되어 버린다. 객체지향이 매우 아쉬워할 소식이다.
    
    ```java
    public class Member {
    	private String name;
    	private Long team_id;
    }
    ```
    
    또한 객체들은 참조 관계가 설정되어 있다면 서로를 자유롭게 이동할 수 있어야 한다.
    
    ![](https://velog.velcdn.com/images/morion002/post/6b2fe54b-47fa-4fde-90e8-1b7b73b066ce/image.png)

    
    ex) `member.getOrder().getOrderItem().getItem();`
    
    하지만 SQL의 경우 처음 어떤 쿼리가 날라가고, 어디까지 조인되는지에 따라 탐색 범위가 결정된다. `JOIN ON MEMBER.ORDER_ID = ORDER.ORDER_ID` 여기까지만 조인하면, member는 Order은 접근할 수 있지만, OrderItem / Item은 접근할 수 없어진다.
    
- 데이터 식별 방법
    
    자바에서는 같은 객체를 비교할 때, 아래와 같이 신뢰성 있게 사용할 수 있다.
    
    ```java
    String name = "김땡땡";
    Member memberOne = list.get(name);
    Member memberTwo = list.get(name);
    
    memberOne == memberTwo; // 같다
    ```
    
    하지만 SQL을 조회하면 매번 새로운 객체를 생성해 반환하기 때문에, 객체 식별을 신뢰할 수 없다.
    
    ```java
    String name = "김땡땡";
    Member memberOne = MemberDAO.getMember(name);
    Member memberTwo = MemberDAO.getMember(name);
    
    memberOne == memberTwo; // 다르다
    
    class MemberDAO {
    	public Member getMember(String name) {
    		String sql = "SELECT * FROM MEMBER WHERE NAME = ?";
    		...
    		return new Member(...);
    	}
    }
    ```
    

### JPA (Java Persistence API) 의 등장

객체지향 장점 좀 살리게 **객체를 자바 컬렉션에 저장하는 것처럼** 데이터베이스에서 데이터들을 관리할 수 있도록 하기 위해 등장했다. JPA는 자바 진영의 ORM 기술 표준이다.

이때 ORM이란, Objected Relational Mapping으로 한국어로 번역하면 객체 관계 매핑이다. **개발자들이 지긋지긋해하는 객체와 데이터 사이의 매핑을 담당해 주는 기술**이다. 따라서 객체는 객체 패러다임으로 설계, 관계형 데이터베이스는 관계형 데이터베이스 패러다임으로 설계가 가능하다. ORM이 중간에서 매핑해 주는 것이다.

![](https://velog.velcdn.com/images/morion002/post/2855c66a-f394-40ba-ab28-65c7ee9f5549/image.png)


JPA는 애플리케이션과 JDBC 사이에서 동작하여, 애플리케이션의 객체와 JDBC의 관계형 데이터베이스 사이의 매핑을 담당해 준다. 여기서 JDBC는 자바에서 데이터베이스에 접속할 수 있도록 해 주는 API이다.

### JPA의 역사

![](https://velog.velcdn.com/images/morion002/post/90fafb29-e4e1-411b-80a0-ab42ad82106d/image.png)


원래 자바에는 EJB라고 자바 표준 ORM이 있었는데, 얘가 너무 짜증 나서 개빈 킹이라는 개발자가 **하이버네이트**를 새로 만들었다. 얘가 자바 진영을 먹고 다니기 시작하자, 자바 쪽에서 개빈 킹에게 연락해 그냥 하이버네이트로 JPA를 만들어 달라고 해서 현재의 버전이 탄생했다.

## JPA를 사용해야 하는 이유

### 생산성 향상

JPA를 사용한 CRUD 코드를 보자.

```java
C: jpa.persis(member);
R: Member member = jpa.find(name);
U: member.setName("박땡땡");
D: jpa.remobe(member);
```

**쿼리? 안 짜도 된다**. 따라서 파라미터 매핑, 객체를 SQL로 변환하기, 데이터베이스에 쿼리 날리기 모두 안 해도 된다. 한 줄이면 끝난다.

### 유지보수성

```java
public class Member {
	private String name;
}
```

앞서 이런 객체에 age라는 필드를 추가하면, Member와 관련된 모든 SQL을 수정해야 했다. 하지만 JPA에서는 내가 SQL을 짜지 않는다. 따라서 아래와 같이 객체만 수정하면 된다.

```java
public class Member {
	private String name;
	private int age;
}
```

SQL은 JPA가 처리한다.

### 객체와 관계형 데이터베이스의 불일치 해결

- 상속
    ![](https://velog.velcdn.com/images/morion002/post/efd73718-f76a-4ab7-bb6b-6e1b26b00b4c/image.png)


    
    ```java
    jpa.persist(album);
    ```
    
    위 코드만 실행하면 album을 ITEM, ALBUM 테이블에 넣는 건 다 JPA가 해 준다.
    
- 연관관계
    ![](https://velog.velcdn.com/images/morion002/post/8568e99a-b85c-49da-815b-ced45341c8db/image.png)


    
    member에 team을 저장할 때, 이제는 더 이상 team_id를 저장하지 않고 아래처럼 사용할 수 있다.
    
    ```java
    member.setTeam(team);
    jpa.persist(member);
    ```
    
    또한 객체끼리 참조가 되어 있다면, 자유롭게 다닐 수도 있다.
    
    ```java
    Member member = jpa.find(Member.class, name);
    Team team = member.getTeam();
    ```
    
- 데이터 식별 방법
    
    신뢰 가능한 엔티티 설계가 가능해진다.
    
    ```java
    String name = "김땡땡";
    Member memberOne = jpa.find(Member.class, name);
    Member memberTwo = jpa.find(Member.class, name);
    
    memberOne == memberTwo; // 같다
    ```
    

## Persistence Context

### Persistence Context

위에 쭉쭉 설명한 것처럼, **관계형 데이터베이스를 객체처럼 사용**할 수 있도록, 그 과정을 도와주는 역할을 한다고 생각하면 된다. 엔티티를 영구 저장하는 환경이라는 뜻을 가지고, JPA를 이해하는 데 가장 중요한 말 중 하나이다. 영속성 컨텍스트는 논리적인 개념이라 눈에 보이지 않는다. 엔티티 매니저를 통해서 영속성 컨텍스트에 접근할 수 있다.

### Entity Manager & Entity Factory Manager

엔티티 매니저는 영속성 컨텍스트에 접근할 수 있는 매니저이다. J2SE 환경에서는 아래 그림처럼 1:1 매핑으로 작동한다. 

![](https://velog.velcdn.com/images/morion002/post/d4ecc084-6eab-40fe-847f-bc19ed5f9321/image.png)



하지만 스프링 같은 프레임워크에서는, Entity Factory Manager에게 엔티티 매니저를 만들어 달라고 한 뒤, 여러 엔티티 매니저가 영속성 컨텍스트에 접근할 수 있도록 한다.

![](https://velog.velcdn.com/images/morion002/post/75e0f140-ba73-470a-b002-67a81642ed42/image.png)



### Entity Lifecycle

![](https://velog.velcdn.com/images/morion002/post/8bf5f6a4-7c35-4351-be50-25e7746edb4f/image.png)


| 비영속
(new/transient) | 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태 |
| --- | --- |
| 영속
(managed) | 영속성 컨텍스트에 관리되는 상태 |
| 준영속
(detached) | 영속성 컨텍스트에 저장되었다가 분리된 상태 |
| 삭제
(removed) | 삭제된 상태 |
- 비영속
    
    ![](https://velog.velcdn.com/images/morion002/post/81ca6238-94e7-47b2-a047-7d1c3aefc739/image.png)

    
    객체를 생성만 하고 JPA와는 아무것도 하지 않은 상태이다.
    
    ```java
    Member member = new Member();
    member.setName("김땡땡");
    ```
    
- 영속
    
    ![](https://velog.velcdn.com/images/morion002/post/7b503832-8daa-43a2-b474-48a5be9215d6/image.png)

    
    생성된 객체를 영속성 컨텍스트가 관리하고 있는 상태이다.
    
    ```java
    Member member = new Member();
    member.setName("김땡땡");
    
    EntityManager em = emf.createEntityManager();
    em.getTransaction().begin();
    
    em.persist(member);
    ```
    
- 준영속
    
    ```java
    em.detach(member);
    ```
    
- 삭제
    
    ```java
    em.remove(member);
    ```
    

## 영속성 컨텍스트를 사용해야 하는 이유

### 1차 캐시

처음에 객체를 생성하고 영속성 컨텍스트에 persist()해서 영속시키면, 영속성 컨텍스트는 이 객체의 ID와 Entity를 1차 캐시에 들고 있게 된다.

```java
Member member = new Member();
member.setId("member1");
member.setName("김땡땡");

EntityManager em = emf.createEntityManager();
em.getTransaction().begin();

em.persist(member);
```

위와 같은 코드를 작성했을 때, 영속성 컨텍스트는 아래와 같은 상태가 된다.

![](https://velog.velcdn.com/images/morion002/post/4af51abd-9461-4e67-8047-45def350a6cd/image.png)


이후에 만약 member1을 호출하게 된다면, JPA는 1차 캐시에 얘가 있는지 없는지부터 조사한다. **1차 캐시에 있다면 바로 값을 반환하고, SELECT 쿼리는 날리지 않는다.**

```java
Member member = new Member();
member.setId("member1");
member.setName("김땡땡");

EntityManager em = emf.createEntityManager();
em.getTransaction().begin();

em.persist(member); // 1차 캐시에 저장

Member targetMember = em.find(Member.class, "member1"); // 1차 캐시에서 조회
```

![](https://velog.velcdn.com/images/morion002/post/75585f58-bdc4-4500-98dc-6a6d9fd6e4e5/image.png)


만약 아래와 같은 객체가 DB에는 있지만 1차 캐시에는 없는 상태라면?

```java
Member member = new Member();
member.setId("member2");
member.setName("박땡땡");
```

우선적으로 1차 캐시를 훑고 원하는 데이터가 없다면 이때 DB에 SELECT 쿼리를 날린다. 그리고 읽은 데이터는 다시 1차 캐시에 저장해 둔다.
![](https://velog.velcdn.com/images/morion002/post/d4f92524-f9d8-45c5-a482-8cf4ecc59a91/image.png)


### 동일성 보장

DB가 아닌 1차 캐시에서 먼저 조회해 오기 때문에, 한 트랜잭션 안에서 같은 객체를 조회한다면 객체끼리의 동일성이 보장된다. 즉, 관계형 데이터베이스를 신뢰할 수 있는 엔티티 객체로 사용 가능해진다. 1차 캐시로 REPEATABLE READ 등급의 트랜잭션 격리 수준을 에플리케이션 차원에서 제공한다.

```java
Member memberOne = jpa.find(Member.class, "member1");
Member memberTwo = jpa.find(Member.class, "member1");

memberOne == memberTwo; // 같다
```

### 트랜잭션을 지원하는 쓰기 지연

JPA는 쿼리문을 만들어낼 때마다 데이터베이스에 바로 저장하지 않는다. 바로 쓰기 지연 저장소에 SQL을 차곡차곡 쌓아 둔 뒤, 한 번에 쿼리문을 전송해 버린다. 이해를 돕기 위해 아래와 같은 억지 코드로 예시를 들겠다. 왜 억지인지는 뒤에 나온다.

```java
EntityManager em = emf.createEntityManager();
em.getTransaction().begin(); // 트랜잭션 시작

em.persist(memberA); // SQL 저장소에 쿼리문 저장
em.persist(memberB); // SQL 저장소에 쿼리문 저장

transaction.commit(); // SQL 날림
```

이럴 때 memberA를 persist하면 아래 그림과 같아진다.

![](https://velog.velcdn.com/images/morion002/post/7cf4ce96-ea6b-4e02-9e66-11d4fd7e4452/image.png)


이후 memberB를 persist하면 아래와 같이 쓰기 지연 저장소에 쌓인다.

![](https://velog.velcdn.com/images/morion002/post/4bd47dbb-df67-433d-a099-47da663a7e5d/image.png)


트랜잭션이 커밋되면 쓰기 지연 저장소가 비워지고 쿼리문이 날라간다.

![](https://velog.velcdn.com/images/morion002/post/b278692d-deef-492b-bfef-e2fe26b0d135/image.png)


❗️억지인 이유

그림을 자세히 보면 member들은 모두 비영속 → 영속 상태로 가기 때문에 1차 캐시에 저장되어야 한다. 1차 캐시에는 엔티티의 Id를 가지고 있는데, 데이터베이스에 접근하지도 않았는데 AutoGenerate 되는 Id 값을 1차 캐시가 알 수 있을까? 그럴 수는 없다. 따라서 비영속 → 영속이 되는 Create 경우에만 바로 SQL이 날라간 뒤 Id 값을 가져와서 1차 캐시에 넣어 둔다.

### 변경 감지

```java
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();

Member targetMember = em.find(Member.class, "member1"); 

targetMember.setName("huisu");

em.getTransaction.commit();
```

위와 같이 엔티티를 수정했다고 생각해 보자.

`em.update(member);` 라는 코드가 필요할까? 아니다.

1차 캐시에는 1차 캐시에 들어올 때의 엔티티 상태를 스냅샷과 저장해 둔다. 따라서 트랜잭션이 커밋될 때 엔티티의 모습이 스냅샷과 다르다면, 자동으로 UPDATE 쿼리를 짜서 쓰기 지연 SQL 저장소에 넣는다. 이를 `Dirty Checking`이라고 한다.

![](https://velog.velcdn.com/images/morion002/post/da347f44-0a61-4a8b-af72-c61265e5b8cb/image.png)


### 지연 로딩

만약 아래와 같은 객체가 있다고 가정해 보자. 하나의 멤버는 하나의 팀에만 가입할 수 있고, 하나의 팀은 여러 멤버로 구성되어 있는 경우이다.

```java
public class Member {
	private Long memberId;
	private String name;
	private Team team;
}

public class Team {
	private Long teamId;
	private List<Member> members;
}

```

우리는 현재 JPA를 사용하기 때문에 `Team`에서의 멤버 관리를 `List<Long> membersId`로 하지 않고 `List<Member> members`로 하고 있다. 이럴 때 내가 어떤 팀의 아이디 조회한다고 가정해 보자.

팀을 조회하는 순간 해당 팀에 가입된 멤버들을 모두 조인하여 불러모으는 쿼리가 완성된다. 나는 팀 아이디 하나 검색하려고 했는데 JPA 혼자 열일 하는 어처구니 없는 상황이 발생한다. 이를 N + 1 문제라고 말하는데, 1 + N 문제라고 하는 게 더 명시적인 것 같다. 이런 문제를 해결하기 위해 JPA에서는 `FetchType.LAZY` 로 지연 로딩을 지원한다.

지연 로딩을 하게 되면 team을 부르는 순간 아래처럼 불러오게 된다.

```java
public class Team {
	private Long teamId;
	private List<Member$HibernateProxy$e97rdqZR> members;
}
```

`Member$HibernateProxy$e97rdqZR` 이 외계어는 뭘까. 팀을 모두 불러오면 N + 1 문제가 발생하기 때문에, 하이버네이트가 바이트 코드를 조작해서 프록시 객체를 만들어 조회하는 것이다. 즉 팀을 조회하면 `teamId`와 껍데기만 `Member`인 가짜 `Member` 객체가 1차 캐시로 올라오게 된다. `Member`를 SELECT하는 쿼리가 발생하지 않는 것이다.

```java
List<Member> targetTeamMembers = targetTeam.getMembers();
```

위 코드처럼 실제로 내가 `Member`를 필요로 하는 순간에 SELECT * FROM MEMBER 쿼리문이 나가게 된다.

만약 서비스에서 멤버와 팀은 항상 같이 조회되며, 굳이 SELECT를 나눠서 두 번 보내는 게 더 부담이라면 `FetchType.EAGER`를 사용해 N + 1개로 불러오면 된다.
