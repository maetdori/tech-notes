# Repository: Spring Data JPA의 핵심


<br/>


## 목차

1. JPA와 Spring Data JPA
2. Repository
3. Repository 의외의 기능


<br/>


## 1. JPA와 Spring Data JPA

  ### 1) JPA

  일반적으로 백엔드 API라 함은 클라이언트가 어떻게 서버를 사용해야 하는지를 정의한 기술 명세를 뜻한다. 그리고 JPA는 `Java Persistence API`의 약자이다. 여기서 우리는 JPA의 정체를 추측해볼 수 있다. **JPA는 자바 진영에서 어떻게 관계형 데이터베이스를 사용해야 하는지를 정의한 기술 명세이다.**        

  여기서 주목할 점은 JPA가 어떤 특정 기능을 구현하는 라이브러리가 아니라, **인터페이스**라는 것이다. 따라서 우리는 JPA에 정의된 오퍼레이션을 직접 구현하거나, JPA를 구현한 **구현체**를 이용함으로써 JPA에 기술된 기능들을 활용할 수 있다.           

  대표적인 JPA 구현체로는 **Hibernate**가 있는데, 우리는 Hibernate를 이용해 Raw JPA를 직접 사용할 수도 있고, 또는 **Spring Data JPA**라는 것을 통해 JPA를 간접적으로 사용할 수도 있다.              

  우선 전자의 경우에, JPA를 이용하기 위해서는 Hibernate가 구현해 놓은 클래스를 가져오고, `EntityManagerFactory`를 생성해 `EntityManager` 인스턴스를 찍어내고, 생성된 `EntityManager` 인스턴스를 통해 직접 영속성 관리를 하는 작업들이 필요하다.      

  이러한 작업들이 약간은 복잡하고 반복적이게 느껴지는데, Spring Data 팀에서는 개발자로 하여금 이러한 복잡한 과정을 겪지 않고, 좀 더 쉽게 JPA를 사용할 수 있게끔 **Spring Data JPA**라는 것을 제공하고 있다. 

  > #### JPA <sub>Java Persistence API</sub>
  > : 자바 진영에서 관계형 데이터베이스를 사용하는 방식을 정의한 **인터페이스**
  >
  > * JPA는 기술 명세이기 때문에 구현체가 필요하다.
  > * Hibernate: JPA를 구현하는 성숙한 라이브러리
  >    * 직접: Raw JPA 사용 (e.g., `EntityManager`)
  >    * 간접: Spring Data JPA (Repository) 사용


  ### 2) Spring Data JPA

  Spring Data JPA는 기존 JPA를 한 단계 더 **추상화** 함으로써 개발자로 하여금 JPA를 좀 더 편리하게 사용할 수 있게 해준다.       

  앞서 말했듯이, Raw JPA를 사용하게 되면 `EntityManagerFactory`를 만들고, `EntityManager` 객체를 생성하는 등 직접적으로 영속성을 관리하는 작업들이 필요하게 되는데, Spring Data JPA는 이러한 공통된 일련의 작업들을 Repository라는 이름의 인터페이스로 한번 감쌌다. 즉, JPA layer와 Application layer 사이에 Repository라는 layer를 하나 추가함으로써 EntityManager와 관련된 작업들을 내부로 숨겨버린 것이다.              

  예를 들어, Spring에서 제공하는 Repository 인터페이스의 default 구현체로 `SimpleJpaRepository`라는 것이 있는데, 그 코드를 살펴보면 내부적으로 `EntityManager`를 선언하고 사용하는 것을 볼 수 있다. `EntityManager`를 통해 도메인 계층과 데이터 계층 사이를 중재하는 역할을 Repository에 위임한 것이다.           

  따라서 Spring Data JPA를 사용하면, DB에 접근할 필요가 있는 대부분의 상황에서 Repository를 활용하면 되고, 개발자는 더 이상 `EntityManager`를 직접 다루지 않아도 된다. 

  > #### Spring Data JPA
  > : JPA를 편리하게 사용할 수 있도록 Spring에서 제공하는 모듈
  > 
  > * 엔티티 매니저를 직접 다루지 않아도 된다.
  > * `Repository`의 개념을 통해 실현
  >     * 데이터 계층과 도메인 계층을 매핑하는 역할
  >     * 개발자는 `Repository`를 통해 도메인 객체에 접근


<br/>


## 2. Repository

  ### 1) Repository

  다시 한번 정리하자면, Repository라는 것은 Spring Data JPA가 JPA를 편리하게 사용할 수 있도록 하는 데에 핵심이 되는 개념이다. Repository는 기본적으로 도메인 계층과 데이터 계층 사이에 존재해서 둘을 중재하는 역할을 하며, 우리는 Repository를 통해 Entity 객체에 접근할 수 있게 된다. 

  > #### Repository
  > 
  > * 데이터 계층과 도메인 게층을 매핑
  > * `Repository`를 통해 `Entity` 객체에 접근하게 된다. 



  ### 2) Spring Data JPA Repository

  Spring Data JPA를 사용할 수 밖에 없게 만드는, Spring Data JPA에서 제공하는 강력한 기능 두 가지가 있는데, 그 중 하나는 **JPA 공통 인터페이스**이고, 나머지 하나는 **쿼리 메서드**이다. 

  #### JPA 공통 인터페이스
  Spring Data 프로젝트는 Repository와 관련해서 몇 가지 인터페이스들을 공통적으로 제공하고 있다. 그 중에서도 `JpaRepository`를 주로 사용하게 되는데, 이를 JPA 공통 인터페이스라 한다. 

  <img src="img.png" width="320px">         

  `JpaRepository`는 그림에서 보이는 상속 관계를 따라 확장된 인터페이스이기 때문에, 기본적인 CRUD 기능과 Paging 및 Sorting 등 다양한 기능을 가지고 있다. 우리는 어떤 특정 Entity의 Repository를 선언할 때 이 `JpaRepository`를 상속하는 것 만으로도 별다른 구현 없이 여기에 포함된 오퍼레이션들을 모두 사용할 수 있기 때문에 상당수의 **Boilerplate code를 제거**할 수 있다는 장점이 있다. 

  ```java
  public interface UserRepository extends JpaRepository<User, Long> {
  }
  ```

  예를 들어 우리가 `User`라는 도메인을 정의하고, 이 `User` 데이터를 관리하는 Repository를 하나 선언하고 싶을 때 단순히 `JpaRepository` 인터페이스를 상속받기만 하면 `findAll()`, `save()`, `delete()` 등의 메서드를 그냥 사용할 수 있게 된다.         

  JPA 공통 인터페이스를 상속함으로써 Entity 별로 CRUD 메서드를 선언하고 구현하는 단순 반복 작업들을 더 이상 하지 않아도 되며, 이는 상당량의 Boilerplate Code가 제거될 수 있음을 의미한다.          

  이러한 강력한 이점 때문에, JPA 공통 인터페이스라는 것이 JPA와 Spring Data JPA를 구분 짓는 첫 걸음이라 봐도 무방하다. 

  #### 쿼리 메서드
  쿼리 메서드는 Spring Data JPA Repository에서 제공하는 또 다른 강력한 기능이다. 원하는 메서드를 네이밍 룰에 따라 선언하기만 하면 자동으로 DB 조회 쿼리가 생성되어 나간다. 

  ```java
  public interface CouponRepository extends JpaRepository<Coupon, Integer> {
      //select * from Coupon where payment_id = {paymentId}
      Optional<Coupon> findByPaymentId(Integer paymentId);

      //select * from Coupon where user_id = {userId} and used = false
      List<Coupon> findAllByUserIdAndUserFalse(Integer userId);
  }
  ```

  예시 코드에서와 같이 명명 규칙에 맞춰서 메서드를 선언하기만 하면, Spring이 애플리케이션 실행 시점에 알아서 해당 메서드 이름에 적합한 쿼리를 날리는 구현체를 동적으로 생성해준다.     
     
  ```java
  //'네이밍 룰'에 맞춰서 메서드를 선언해야 한다고 하기는 하지만 굉장히 직관적이어서 
  // 경험 상 SQL 쿼리 문법을 잘 몰라도 의식의 흐름대로 작성하다 보면 대부분 다 잘 된다(?)   
  List<Coupon> findAllByUserIdAndAmountGreaterThanAndExpiryDateIsGreaterThanEqual(Integer userId, int min, LocalDate now);
  ```     

  > #### Spring Data JPA Repository
  >
  > 1. 간단한 CRUD 기능을 공통으로 처리하는 **JPA 공통 인터페이스** 제공
  >    * `JpaRepository`를 물려받기만 하면 기본적인 CRUD 메서드 및 다양한 기능들을 사용할 수 있게 된다. 
  >    * 반복적으로 사용되는 Boilerplate Code를 제거할 수 있다. (No-code Repository)
  >    * `findAll()`, `save()`, `delete()` ...
  >
  > 2. 메서드 명명 규칙을 통해 쿼리를 자동 생성해주는 **쿼리 메서드** 기능 제공
  >    * 네이밍 룰에 맞춰서 메서드를 선언하기만 하면 Spring Data JPA가 알아서 쿼리를 수행해준다.
  >    * 의식의 흐름대로 작성하다 보면 대부분 다 된다. 


<br/> 


## 3. Repository 의외의 기능

  개인적으로 공부하면서 새롭게 느껴졌던 Repository의 기능들에 대해 정리해보려 한다. 

  ### 1) JOIN 쿼리도 실행할 수 있다.

  ```java
  @Entity
  public class Member {
      @OneToMany
      private List<MemberDetail> details;
  }

  @Entity
  public class MemberDetail {
      @EmbeddedId
      private Pk pk;

      @Embeddable
      public static class Pk {
          private String type;
      }
  }
  ```

  위와 같은 엔티티로 매핑되는 두 테이블을 조인해서 데이터를 가져와야 하는 상황을 생각해보자. 나는 이럴 때면 우선 한 테이블에서 데이터를 검색해서 쫙 가져오고, 또 다른 테이블에서 데이터를 검색해서 쫙 가져온 다음, 공통 조건을 만족하는 데이터를 골라내는 식으로 코드를 작성했었다.                   

  그런데, JPA의 Repository는 이미 JOIN 쿼리 메서드까지도 제공하고 있었다.            

  ```java
  public interface MemberRepository extends JpaRepository<Member, Long> {
      // select * from Member m
      // inner join MemberDetail md
      // on m.member_id = md.member_id
      // where md.type = {type}
      List<Member> findByDetails_Pk_Type(String type); //언더바를 사용하여 JOIN 쿼리메서드 작성
  }
  ```
  
  사실 이전에 내가 했던 방식은 
  1. 데이터베이스 조회 쿼리를 비즈니스 로직까지 끌고 올 뿐만 아니라
  2. 불필요한 데이터까지 조회하고,
  3. 디스크에 여러 번 접근하게 되는 등

  여러 가지 문제점이 있었는데, JPA Repository에서 제공하는 쿼리 메서드를 사용하면 JOIN 쿼리를 단 한 줄의 코드로 간단하게 끝낼 수 있다는 장점이 있다. 

  > 물론 쿼리 메서드가 무조건적으로 좋은 방법이라고만은 할 수 없다. 세미나 때 팀장님께서 주신 의견에 따르면, 다음과 같은 이슈들을 생각해 볼 수 있다. 
  > 1. 쿼리 메서드가 너무 길어지고 복잡해지면 오히려 가독성이 떨어질 수 있다. 
  > 2. 간단한 조회 쿼리 수준에서는 문제가 되지 않을 수 있지만, 앞서 소개한 JOIN 쿼리 메서드와 같이 다소 직관적으로 이해하기 힘든 쿼리 메서드를 사용하려면 우선 팀원들의 JPA에 대한 이해도가 어느 정도 동일 선상에 있다는 것이 가정되어야 할 것이다. 


  ### 2) DTO Projection도 가능하다.

  ```java
  public interface CouponRepository extends JpaRepository<Coupon, Integer> {
      //select * from Coupon where payment_id = {paymentId}
      Optional<Coupon> findByPaymentId(Integer paymentId);
  }

  public class CouponDto {
      private Integer id;
      private String name;
      private int discountRate;
      private int minAmount;

    // Entity를 Dto로 매핑하는 용도의 생성자
      public CouponDto(Coupon entity) {
          this.id = entity.getId();
          this.name = entity.getCouponType().getName();
          this.discountRate = entity.getCouponType().getDiscountRate();
          this.minAmount = entity.getCouponType().getMinAmount();
      }
  }
  ```

  과거에 작성했었던 코드를 그대로 가지고 왔다. 쿠폰 테이블에서 쿠폰을 하나 조회해서 이를 클라이언트에게 넘겨주는 작업이 필요했었는데, 이 과정을 수행하기 위해서 다음의 로직을 따라야 했다.

  1. Repository의 메서드를 이용해 조건에 맞는 쿠폰 Entity를 조회해온다.
  2. 조회한 Coupon의 타입을 Entity에서 Dto로 바꿔준다.
  3. 클라이언트에 `CouponDto`를 넘겨준다.

  즉, Repository에서 찾아준 쿠폰의 타입은 Entity이므로, 클라이언트에 넘겨주기 위해서는 이 쿠폰을 Dto 타입으로 바꿔줘야 하고, 따라서 Coupon Entity를 Coupon Dto에 매핑하는 코드를 추가로 작성해줘야 하는 것이다.       

  그런데, JPA의 Repository는 DTO Projection을 지원하고 있었다. 간단히 말해서, Repository에서 조회한 데이터를 Entity 객체가 아니라, Dto 객체로 반환해주는 기능을 제공한다는 것이다.                     

  ```java
  public interface CouponRepository extends JpaRepository<Coupon, Integer> {
      Collection<CouponDto> findByPaymentId(Integer paymentId);
  }

  @RequiredArgsConstructor
  public class CouponDto {
      private final Integer id;
      private final String name;
      private final int discountRate;
      private final int minAmount;
  }
  ``` 

  `CouponDto`를 따로 정의해놓고, Repository 조회 메서드의 반환형을 `CouponDto`로 설정해주기만 하면, JPA Repository가 `CouponDto`의 생성자를 보고 알아서 Entity를 Dto에 매핑해주는 것이다.           

  이렇게 되면 비즈니스 로직 상에서 직접 Entity를 Dto에 매핑할 필요도 없고, Dto 클래스 내에 Entity를 Dto로 매핑하는 생성자를 정의할 필요도 없기 때문에 코드도 매우 깔끔해진다. 
