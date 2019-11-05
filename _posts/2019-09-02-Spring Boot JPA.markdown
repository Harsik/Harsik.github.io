---
layout: post
title: "Spring Boot JPA"
date: 2019-09-02
categories:
tags: JPA Hibernate H2 Lombok
---
<div style="display:none;">
SPring security 설명을 위한 사전 작업 중 하나로 jpa를 통한 db 유저 정보 등록 및 조회로 login logout을 구현 할 수 있도록 설명
초반 Spring Boot jpa에 대한 간략한 소개와 설명
중반 jpa 사용방법 h2
후반 jpa 응용 방법 mysql
<br><br>
</div>
우선 <b>JPA</b>가 뭐지라는 생각과 함께 고개를 기울이는 독자들을 위해서 간략히 설명하면서 시작하겠다. 

<br><br>
![JPA&ORM](/files/jpa/JPA&ORM.png)
<br><br>

<b>JPA</b>란
자바 <b>ORM</b>기술에 대한 API 표준 명세를 의미한다. 여기서 <b>ORM</b>란 JAVA 객체가 DB의 테이블이 되도록 매핑 시켜주는 것을 말한다. 즉 <b>Class => Table</b>이다. <b>JPA</b>를 사용하면 쿼리를 통한 조회 또한 메서드 호출로 변경하여 사용할 수 있다. 예를 들어 <b>SELECT * FROM user;</b>라는 쿼리는 <b>user.findAll()</b>가 같이 바꿔 쓸 수 있다는 것이다. 쿼리를 만들지 않고 메서드 호출만 해도 되니 생산성이 매우 높아진다. 다만 복잡한 쿼리는 메서드 호출로 감당할 수 없다는 문제점을 동반한다. <b>Oracle DB 피벗테이블</b> 같은 건 사용하지 못한다. 
<br><br>
필자는 이전까지 <b>Mybatis</b>나 <b>Ibatis</b>를 주로 사용했었다. 하지만 <b>Spring Boot</b>를 시작하고 <b>JPA</b>를 사용해 보니 앞에 것과 비교했을 때 매우 편리하며 그리고 속도차이도 거의 없는 기술이라는 것을 체감할 수 있었다.
<br><br>
이제 <b>JPA</b>를 구현한 프레임워크 중 하나인 <b>Hibernate</b>를 사용해 볼 것이다. 우선 사용하고자 하는 의존성패키지를 추가해야한다. 

<br><br>
![dependencies](/files/jpa/dependencies.png)
<br><br>

어디서 본 것 같다고 생각한다면 착각이 아니다. <b>Spring Boot Initiailizer</b>로 프로젝트를 처음 생성했을 때를 기억하는가? 처음 생성할 때 추가한 <b>Dependency</b>에 있던 것들이다. 고로 따로 수정할 사항은 없다. 
<br><br>
여기서 <b>Lombok</b>은 뭐길래 들어가 있지라는 생각을 할 수 있다. 그래서 <b>Lombok</b>에 대한 간략한 설명을 하겠다. User라는 Class 객체에서 <b>name</b>이라는 변수에 접근하기 위해서 일반적으로 Class 내에서 <b>getName, setName</b>이라는 메소드를 만들어 사용한다. 
<br><br>근데 이게 변수가 한 두개면 상관없는 데 변수가 늘어나는 대로 그것도 두배로 늘어나니 선언을 한건지 안한건지도 모르겠고 코드는 복잡하기만 하고 짜증이 난다. 줄여 말하자면 <b>가독성</b>이 떨어진다. 이런 시원치 않은 짓을 하지 않도록 Annotation을 추가하여 해결한 것이 <b>Lombok</b>이다. 좀 더 보기 편하게 쓰자면
<br><br>

```java
public class Account {
    private String email;
    private String password;
    public String getEmail(){
        return this.email;
    }
    public void setEmail(String email){
        this.email=email;
    }
    public String getPassword(){
        return this.password;
    }
    public void setPassword(String password){
        this.password=password;
    }
}
```
<br><br>
->
<br><br>    
```java
@Getter
@Setter
public class Account {
    private String email;
    private String password;
}
```
이렇게 된다. 보기만 해도 지저분한 코드들이 싹 정리되었다. <b><a href="https://projectlombok.org/">Lombok 홈페이지</a></b>에서 지원되는 Annotation을 확인할 수 있으며 사용하는 방법에 대해서는 <b><a href="https://cheese10yun.github.io/lombok/">여기</a></b>를 참고하길 바란다. 
<br><br>   
이 다음 <b>JPA</b>를 사용하는 예제를 하나 만들 것이다. 우리가 목표로 하는 것은 <b>Rest API</b>를 이용한 백엔드 서버를 구축하는 것이기 때문에 기초적인 부분을 구현할려고 한다. 여기서 사용할 DB는 <b>H2</b> DB로 독자들을 따로 설치가 필요가 없다. 이것은 매우 가벼운 인메모리 DB이기 때문에 서버 실행과 동시에 설치되고 실행된다. 
<br><br>   
일단 파일 구조를 보면서 어떻게 만들어 갈 것인지 설명하겠다.
<br><br>
![FileTree](/files/jpa/FileTree.png)
<br><br>
위에서 부터 <b>AccountController</b>는 Rest API로 받은 요청을 처리하는 클래스이고, <b>Account</b>는 테이블과 매핑될 클래스이며, <b>AccountRepository</b>는 JpaRepository를 사용할 클래스고, <b>AccountService</b>는 controller에서 실행될 repository를 이용한 메소드가 있는 클래스이다. 설명이 너무 간단하여 이해하기 힘들 것이다. 차례차례 설명하겠다.
<br><br> 우리의 목표는 백엔드 서버 구축이다. 거기에 요청 받아 계정을 하나 만들고 그것을 DB에 저장하는 기능을 구현해 볼려고 한다. 테스트를 위해 프론트서버에서 통신이 들어온 것이 아닌 <b><a href="https://www.getpostman.com/">Postman</a></b>이라는 프로그램을 이용하여 요청을 보낼 것이다. localhost:8080에 아무요청이나 보내보아라.
<br><br>
![firstrequestfail](/files/jpa/firstrequestfail.png)
<br><br>
물론 아직 localhost:8080에 서버를 열지 않았기 때문에 위와 같은 화면이 나올 것이다. 자 우리는 요청을 하고 싶다. 그렇다면 서버는 이 요청을 받아드릴 부분이 필요하다. 그러기 때문에 우리는 요청을 컨트롤 할 수 있는 Controller가 필요하다. 
<br><br>
```java
@RestController
public class AccountController {

    @Autowired
    private AccountService accountService;
    
    @PostMapping("/saveAccount")
    public ResponseEntity<?> saveAccount(String email, String password) {
        accountService.saveAccount(email, password);
        return ResponseEntity.ok("");
    }

}
```
<br><br>
<b>@RestContorller</b>이 선언된 클래스에 '/'이하의 주소에서 오는 요청을 처리한다. 그 중 <b>Post</b> 요청은 <b>@PostMapping</b>이 선언된 메소드에서 처리하는데 지금 선언된 <b>"/saveAccount"</b>주소로 오는 요청을 처리하게 된다. <b>ResponseEntity</b>는 응답에 http 상태코드도 같이 보낼 수 있는 객체이다. 요청은 <b>AccountController</b>에서 받았으니 이제 그 처리과정을 밟을 차례이다. <b>AccountService</b>를 살펴보자.
<br><br>
```java
@Service
public class AccountService{

        @Autowired
        private AccountRepository accountRepository;

        public void saveAccount(String email, String password) {
                Account account = new Account(email, password);
                accountRepository.save(account);
        }

}
```
<br><br>
독자들은 왜 이 메소드를 <b>Controller</b>에다 선언하지 않았지?라는 생각이 들 수 있다. 독자들이 어떤 구조를 만들건 그건 독자들 자유이다. 필자는 목적에 따라 구분했을 뿐이다. 
<br><br><b>AccountService</b>에서 선언된 메소드를 보면 <b>Account</b> 객체를 선언하고 생성자를 통해 만든 객체를 <b>Repository</b>의 <b>save</b>메소드로 DB에 저장하는 것을 볼 수 있는다. 
<br><br>여기까지 오면 아 뭔 흐름인지는 알겠는데 <b>Account</b>가 어떤 구조로 되있길래 생성자를 통해 어떤 객체가 되었으며 <b>Repository</b> 또한 어떤 구조길래 <b>save</b> 메소드라는 것을 사용하여 DB에 저장하는 것이지라는 의문이 들 것이다. 차례차례 설명하겠다. 우선 <b>Account</b> 객체에 대해서다.
<br><br>
```java
@Entity
@Getter
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Table(name = "accounts")
public class Account {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Email
    private String email;

    private String password;

    @CreationTimestamp
    private LocalDateTime createdAt;

    @UpdateTimestamp
    private LocalDateTime updatedAt;

    @Builder
    public Account(String email, String password) {
        this.email = email;
        this.password = password;
    }
}
```
<br><br>
<b>@Getter나 @Setter, @Builder, @NoArgsConstructor</b>는 아까 전 보았던 Lombok의 Annotation이라 눈에 익겠지만 <b>@Entity와 @Table, @Id, @GeneratedValue</b>는 JPA의 Annotation이기 때문에 따로 설명하겠다. 
<br><br><b>@Entity</b>는 선언된 클래스가 테이블과 링크될 클래스임을 나타내는 Annotation이며, <b>@Table</b>은 어떤 테이블로 변환될지를 정하는 Annotation이다. 지금은 Table의 이름을 accounts로 정한 상태이다. <b>@Id</b>는 테이블의 인덱스성 기본키를 만드는 Annotation이며, <b>@GeneratedValue</b>가 튜플이 생성될 시 기본키를 하나씩 더해주는 역할을 한다.
<br><br><b>CreationTimestamp, UpdateTimestamp</b> 둘 다 <b>Hibernate</b>에서 제공하는 Annotation이다.으로 테이블에 새로운 객체가 저장될 시 생성시간, 수정시간을 기록해주는 Annotation이다. 이렇게 생성된 Class를 Repository에 등록하면 아래와 같이 된다.
<br><br>
```java
public interface AccountRepository extends JpaRepository<Account,Long>{
}
```
<br><br>
선언한 <b>Repository</b>에 extends로 JPA의 기능인 <b>JpaRepository</b>을 붙여 Account를 참조시키면 DB와 상호작용 할 수 있다. 위 accountRepository.save()의 save가 <b>JpaRepository</b>에서 제공하는 메소드인 것이다. 그 밖에 제공하는 다양한 메소드도 사용 할 수 있다. 자 이제 서버를 실행하고 요청을 보내보자.
<br><br>
![sendRequestTosaveAccount](/files/jpa/sendRequestTosaveAccount.png)
<br><br>
성공한 것 같은가? 아직 성공시 응답값을 넣지 않아 Body가 비어 있을 뿐 성공한 것이다. 하지만 이걸로는 성공한 건지 아닌 건지 잘 모르겠거니와 DB에 저장이 제대로 되었는지 확인할 길이 없기에 우리는 <b>H2 Console</b>를 통해 직접봐야한다.
<br><br>
![springh2consoletrueyml](/files/jpa/springh2consoletrueyml.png)
<br><br>
위와 같은 <b>application.yml</b> 혹은 <b>application.property</b>를 찾아 프로퍼티를 등록한다. 그리고 서버를 재 실행후 <b><a href="http://localhost:8080/h2-console">H2 Console</a></b>에 접속해 보자.
<br><br>
![h2console](/files/jpa/h2console.png)
<br><br>
위와 같은 화면이 뜰 것이다. 접속하면 Entity로 생성한 테이블이 보일 텐데 정면에 쿼리를 입력할 수 있으니 우리가 요청한 것이 적용됬는지 확인해보자.
<br><br>
![comfirmInputingRequest](/files/jpa/comfirmInputingRequest.png)
<br><br>
요청한 대로 잘 입력된 것을 확인할 수 있다. 확인해 본 결과 값을 보면 비밀번호가 저렇게 들어가 있으면 안된다는 생각이 들 것이다. 그러므로 다음 포스트에 우리는 <b>Spring security</b>라는 Framwork기술을 적용 시켜 보안에 문제가 없도록 할 것이다.
<div style="display:none;">
이 다음 할 것으로 가상머신의 소개와 CentOS소개 및 설치 Mysql 소개와 설치, 연결할까? -- 당장은 안필요할듯
아니면 Spring security 소개와 적용 할까? -- 적용 한다고 해도 테스트할 페이지를 만들어야되, themeleaf로 샘플이 있긴 하지
아니면 프론트 엔드 vuejs의 소개와 간단한 로그인 페이지 생성 할까?
Spring security 먼저 themeleaf로 로그인 페이지 만들고 그걸 vuejs로 전환하는 방식으로
</div>