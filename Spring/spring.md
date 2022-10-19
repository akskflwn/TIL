# 스프링 부트 핵심 가이드

# [01.스프링 부트란?](#01스프링-부트란)

# [02.스프링 DI](#02스프링-의존)

# 스프링 부트란?

스프링 프레임워크(Spring Framework)는 자바 기반의 애플리케이션으로 프레임워크로 엔터프라이즈급 애플리케이션을 개발하기 위한 다양한 기능을 제공한다. 스프링은 목적에 따라 다양한 프로젝트를 제공하는데, 그중하나가 Spring boot 이다.

**스프링 부트의 핵심 가치**

`애플리케이션 개발에 필요한 기반을 제공해서 개발자가 비즈니스 로직 구현에만 집중할 수 있게끔 하는것`

이같은 스프링을 효율적으로 사용할 수 있도록 스트링의 특징과 구조 등을 알아보자.

## 1.1제어 역전(loC)

스프링은 기존 자바 개발 방식과 다르게 동작한다.
스프링은 IoC를 적용한 환경에서는 사용할 객체를 직접 생성하지 않고 객체의 생명 주기관리를 외부에 위힘한다.

여기서 `외부`는 스프링 컨테이너 또는 IoC컨테이너를 의미한다.

객체의 관리를 컨테이너에 맡겨 제어권이 넘어간것을 `제어의 역전`이라고 부르며, 제어 역전을 통해 의존성 주입(`DI`), 관점 지향 프로그래밍(`AOP`) 등이 가능해진다.

## 1.2의존성 주입(DI)

의존성 주입 : 제어 역전의 방법중 하나. 사용할 객체를 직접 생성하지 않고 외부 컨테이너가 생성한 객체를 주입받아 사용하는 방식.

스프링에서 의존성 주입을 하는 방법.

- `생성자`를 통한 의존성 주입
- `필드 객체 선언`을 통한 의존성 주입
- `setter 메서드`를 통한 의존성 주입

스프링 공식 문서에서 권장하는 의존성 주입 방법은 생성자를 통해 의존성을 주입 받는 방식이다.
`다른 방식과는 다르게 생성자를 통해 의존성을 주입받는 방식`은 레퍼런스 객체 없이는 객체를 초기화 할 수 없게 설계할 수 있기 때문입니다.

생성자를 통한 의존성 주입

```java
@RestController
pulic class DIController{
    Myservice myService;

    @Autowired
    public DIcontroller(MyService myService){
        this.myService = myService;
    }

    @Getmapping("/di/hello")
    public String getHello(){
        return myService.getHello();
    }
}
```

## 1.3 관점 지향 프로그래밍(AOP)

관점지향 프로그래밍은 스프링의 아주 중요한 특징이다.

(간혹 AOP를 OOP의 대체 개념으로 오해하는 부분이 있는데 AOP는 OOp를 더욱 더 잘 사용하도록 돕는 개념이다)

AOP는 관점 기준으로 묶어 개발하는 방식을 의미한다.
여기서 관점이란 어떠한 기능을 `핵심기능` 과 `부가 기능` 으로 구분의 각각을 하나의 관점으로 보는 것을 의미한다.

`핵심기능`은 비즈니스 로직을 구현하는 과정에서 비즈니스 로직이 처리하려는 목적 기능을 말한다.

ex
클라이언트로 상품 정보 등록 요청을 받아 데이터베이스에 저장하고, 그 상품 정보를 조회하는 비즈니스 로직을 구현한다면. (1) 상품 정보를 데이터베이스에 저장하고, (2) 저장된 상품 정보 데이터를 보여주는 코드가 핵심 기능이다.

# 02스프링-의존

DI란 `Denpendency Injection` 의 약자로 우리말로는 `의존 주입` 이라고 한다.

이 단어의 의미를 이해하려면 먼저 '의존'이 뭔지 알아야 한다.
여기서 말하는 의존은 객체 간의 의존을 의미한다.
이해를 돕기해위해 회원 가입 처리 기능을 구현한 다음의 코드를 보자

```java
public class MemberRegisterService{

    private MemberDao memberDao = new MemberDao();

    public void regist(RegisterRequest req){
        //이메일로 회원 데이터 Member 조회
        Member member = memberDao.selectByEmail(req.getEmail());

        if(member != null){
            //같은 이메일을 가진 회원이 이미 존재 하면 익센션 발생
            throw new DuplicatedMemberException("dup email"+ req.getEmail());
        }

        // 같은 이메일을 가진 회원이 존재하지 않으면 DB에 삽입
        Member newMember = new Member(
            req.getEmail(), req.getPassword(), req.getName(),
            LocalDateTime.now());

        memberDao.insert(newMember);
    }
}
```

서로 다른 회원은 동일한 이메일 주소를 사용할 수 없다는 요구사항이 있다고 가정해보자.

이 제약사항을 처리하기 위해 MemberRegisterService 클래스는 MemberDao 객체의 selectByEmail() 메서드를 이용해서 동일한 이메일을 가진 회원 데이터가 존재하는지 확인한다.

만약 같은 이메일을 가진 회원 데이터가 존재한다면 위 코드처럼 익셉션을 발생시킨다.

같은 이메일을 가진 회원의 데이터가 존재하지 확인하기 위해 MemberDao 객체의

SelectByEmail() 메서드를 실행하고. 회원을 저장하기위해 insert() 메서드를 사용한다.

이렇게 한 클래스가 다른 클래스의 메서드를 실행할 때 이를 '의존'한다고 표현한다.
앞 코드에서는 `MemberRegisterService 클래스가 MemberDao 클래스에 의존한다` 고
표현할 수 있다.

```
의존은 변경에 의해 영향을 받는 관계를 의미한다. 에를 들어 MemberDao의 insert() 메서드의 이름을 insertMember()로 변경하면 이세머드를 사용하는 memberRegisterService 클래스의 소스 코드도 함께 변경된다. 이렇게 변경에 따른 영향이 전파되는 관계를 `의존`한다고 표현한다.
```

의존하는 대상이 있으면 그 대상을 구하는 방법이 필요하다. 가장 쉬운 방법은 의존 대상 객체를 직접 생성하는 것이다. 앞서 설펴본 MemberRegisterService 클래스도 다음 코드처럼 의존 대상인 MemberDao의 객체를 직접 생성해서 필드에 할당했다.

MemberRegisterService 클래스에서 의존하는 MemberDao 객체를 직접 생성하기 때문에
MemberRegisterService 객체를 생성하는 순간에 MemberDao 객체도 함께 생성된다.

클래스 내부에 직접 의존 객체를 생성하는 것이 쉽긴 하지만 유지보수 관점에서 문제점을 유발할 수 있다.
`때문에 스프링은 DI를 통해서 의존 처리`를 한다.

## 02.1 DI를 통한 의존 처리

`DI(Dependency Injection, 의존주입)`
는 의존하는 객체를 직접 생성하는 대신 의존 객체를 전달받는 방식을 사용한다.
MemberRegisterService 클래스에 DI 방식을 적용해보자.

```java
...
public class MemberResgisterService{
    private MemberDao memberDao;

    public MemberRegisterService(MemberDao memberDao){
        this.memberDao = memberDao;
    }
    ...
}
```

직접 의존 객체를 생성했던 코드와 달리 바뀐 코드는 의존 객체를 직접 생성하지 않는다.
생성자를 통해 MemberRegisterService가 의존 하고 있는 MemberDao 객체를 주입 받은 것이다.

의존 객체를 직접 구하지 않고 생성자를 통해서 전달받기 때문에 이 코드는 DI 패턴을 따르고있다.

DI를 적용한 결과 MemberRegisterService 클래스를 사용하는 코드는 객체를 생성할 때 생성자에서 MemverDao 객체를 전달해야 한다.

```java
MemberDao dao = new MemberDao();
MemberRegisterService svc = new MemberRegisterService(dao);
```

뭔가 더 복잡해보인다. 앞서 의존 객체를 직접 생성하는 방식과 달리 의존 객체를 주힙하는 방식은 객체를 생성하는 부분의 코드가 조금 더 길어졌다.

**`그냥 직접 의존 객체를 생성하면 되는 데 왜 굳이 생성자를 통해서 의존하는 객체를 주입하는 걸까?`**

바로 변경의 유리함이다.

## 02.2 DI와 의존 객체 변경의 유리함

의존 객체를 직접 생성하는 방식은 필드나 생성자에서 new 연산자를 이용해 객체를 생성한다.
회원 등록 기능을 제공하는 MemberRegisterService 클래스에서 다음 코드 처럼 의존 객체를 직접 생성할 수 있다.

```java
public class MemberRegisterService{
    private MemberDao dao = new MemberDao();
    ...
}
```

회원의 암호 변경 기능을 제공하는 ChangePasswordService 클래스도 다음과 같이 의존 객체를 직접 생성한다고 하자.

```java
public class ChangePasswordService{
    private MemberDao memberDao = new MemberDao();
}
```

MemberDao 클래스는 회원 데이터를 데이터 베이스에 저장한다고 가정해보자. 이 상태에서 회원 데이터의 빠른 조회를 위해 캐시를 적용해야 하는 상황이 발생했다. 그래서 MemberDao 클래스를 상속받는 CachedmemberDao 클래스르 만들었다.

```java
public class CahchedMemberDao extends MemberDao{
    ...
}

```

(캐시는 데이터 값을 볷사해 놓은 임시 장소를 가리킨다. 보통 조회 속도 향상을 위해 캐시를 사용함.)

캐시 기능을 적용한 CachedMemberDao를 사용하려면 MemberRegisterService 클래스와 ChangePasswordService 클래스의 코드를 다음과 같이 변경해야 한다

````java
public class MemberRegisterService{
    private MemberDao dao = new CachedMemberDao();
    ...
}
```java
public class MemberRegisterService{
    private MemberDao dao = new CachedMemberDao();
    ...
}
````

만약 MemberDao 객체가 필요한 클래스가 세가지라면 세 클래스 모두 동일하게 소스 코드를 변경해야 한다.

동일한 상황에서 DI를 사용하면 수정할 코드가 줄어든다.

```java
 MemberDao memberDao = new MemberDao();
 MemberRegisterService regSvc = new MemberRegisterService(memberDao);
 ChangePasswordService pwdSvc = new ChangePasswordService(memberDao);
```

변경후 ->

```java
 MemberDao memberDao = new CachedMemberDao();
 MemberRegisterService regSvc = new MemberRegisterService(memberDao);
 ChangePasswordService pwdSvc = new ChangePasswordService(memberDao);
```

DI를 사용하면 MemberDao 객체를 사용하는 클래스가 세 개여도 변경할 곳은 의존 주입 대상이 되는 객체를 생성하는 코드 한 곳뿐이다. 앞서 의존 객체를 직접 생성했던 방식에 비해 변경할 코드가 한 곳으로 집중되는 것을 알 수 있다.
