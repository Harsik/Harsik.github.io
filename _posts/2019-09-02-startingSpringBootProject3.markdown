---
layout: post
title: "SpringBoot 프로젝트 시작하기 3 - 스프링부트 JPA"
date: 2019-09-02
categories: SpringBoot
tags: JPA Hibernate H2 Lombok
comments: true
---
<div style="display:none;">
SPring security 설명을 위한 사전 작업 중 하나로 jpa를 통한 db 유저 정보 등록 및 조회로 login logout을 구현 할 수 있도록 설명
초반 Spring Boot jpa에 대한 간략한 소개와 설명
중반 jpa 사용방법 h2
후반 jpa 응용 방법 mysql
<br><br>
</div>
<h3>JPA란</h3>
<br>
&nbsp;우선 JPA가 뭔지 간략히 설명하도록 하겠습니다. 
<br><br>
![JPA&ORM](/files/jpa/JPA&ORM.png)
<br><br>
&nbsp;JPA란 자바 ORM기술에 대한 API 표준 명세를 의미합니다. 여기서 ORM란 JAVA 객체가 DB의 테이블이 되도록 매핑( Class => Table ) 시켜주는 것을 말합니다. JPA를 사용하면 쿼리를 통한 조회 또한 메서드 호출로 변경하여 사용할 수 있습니다. 
예를 들어 <i>SELECT * FROM user;</i>라는 쿼리는 <i>user.findAll()</i>와 같이 메소드로 바꿔 쓸 수 있다는 것입니다. 
쿼리를 만들지 않고 메서드 호출만 해도 되니 생산성이 매우 높아집니다. 
다만 복잡한 쿼리는 메서드 호출로 감당할 수 없다는 문제점을 동반하게 됩니다. 
<br><br>
&nbsp;저는 이전까지 마이바티스를 주로 사용했었습니다. 하지만 스프링부트를 시작하고 JPA를 사용해 보니 앞에 것과 비교했을 때 매우 편리하며 그리고 속도차이도 거의 없는 기술이라는 것을 체감할 수 있었습니다.
<br><br>
&nbsp;이제 JPA를 구현한 프레임워크 중 하나인 하이버네이트를 사용해 볼 것입니다. 우선 사용하고자 하는 의존성패키지를 추가해야 합니다. 
<br><br>
![dependencies](/files/jpa/dependencies.png)
<br><br>
&nbsp;스프링부트 이니셜라이져로 프로젝트를 처음 생성했을 때를 기억해보면 위 디펜더시들은 처음 생성할 때 추가한 디펜더시에 있던 것들입니다. 따로 수정할 사항은 없습니다. 
<hr class="divider">
<h3>롬북이란</h3>
<br>
&nbsp;롬북에 대한 간략한 설명을 하겠습니다. User라는 Class 객체에서 name이라는 변수에 접근하기 위해서 일반적으로 Class 내에서 getName, setName이라는 메소드를 만들어 사용합니다. private로 보안을 지키면서 사용하기 위해서입니다.
<br><br>
&nbsp;근데 이게 변수가 한 두개면 상관없는 데 변수가 늘어나는 대로 get,set 메소드도 두배로 늘어나니 선언을 한건지 안한건지 헷갈립다. 이러한 문제를 어노테이션을 추가하여 해결한 것이 롬북입니다.
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
<h1>-></h1> 
```java
@Getter
@Setter
public class Account {
    private String email;
    private String password;
}
```
&nbsp;롬북을 사용하면 이렇게 됩니다. 보기만 해도 지저분한 코드들이 싹 정리되었습니다. <b><a href="https://projectlombok.org/">Lombok 홈페이지</a></b>에서 지원되는 어노테이션을 확인할 수 있으며 사용하는 방법에 대해서는 <b><a href="https://cheese10yun.github.io/lombok/">여기</a></b>를 참고하길 바랍니다. 
<hr class="divider">
<h3>JPA 예제 만들기</h3>
<br> 
&nbsp;이 다음 JPA를 사용하는 예제를 하나 만들 것입니다. 제가 목표로 하는 것은 Rest API를 이용한 백엔드 서버를 구축하는 것이기 때문에 기초적인 부분을 구현하려고 합니다. 여기서 사용할 DB는 H2 DB로 매우 가벼운 인메모리 DB이기 때문에 독자들을 따로 설치가 필요가 없습니다. 서버 실행과 동시에 설치되고 실행됩니다. 일단 파일 구조를 보면서 어떻게 만들어 갈 것인지 설명하겠습니다.
<br><br>
![FileTree](/files/jpa/FileTree.png)
<br><br>
&nbsp;위에서 부터 AccountController는 Rest API로 받은 요청을 처리하는 클래스이고, Account는 테이블과 매핑될 클래스이며, AccountRepository는 JpaRepository를 사용할 클래스고, AccountService는 controller에서 실행될 repository를 이용한 메소드가 있는 클래스입니다.
<br><br> 
&nbsp;백엔드 서버 구축을 위해 거기에 요청 받아 계정을 하나 만들고 그것을 DB에 저장하는 기능을 구현해 볼려고 합니다. 테스트를 위해 프론트서버에서 통신이 들어온 것이 아닌 <b><a href="https://www.getpostman.com/">Postman</a></b>이라는 프로그램을 이용하여 요청을 보낼 것입니다. localhost:8080에 아무요청이나 보내보겠습니다.
<br><br>
![firstrequestfail](/files/jpa/firstrequestfail.png)
<br><br>
&nbsp;아직 localhost:8080에 서버를 열지 않았기 때문에 위와 같은 화면이 나옵니다. 서버는 이 요청을 받아드릴 부분이 필요합니다. 그러기 때문에 우리는 요청을 컨트롤 할 수 있는 콘트롤러가 필요합니다. 
<hr class="divider">
<h3>컨트롤러, 서비스</h3>
<br> 
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
&nbsp;@RestContorller이 선언된 클래스에 '/'이하의 주소에서 오는 요청을 처리합니다. 그 중 포스트 요청은 @PostMapping이 선언된 메소드에서 처리하는데 지금 선언된 "/saveAccount"주소로 오는 요청을 처리하게 됩니다. ResponseEntity는 응답에 http 상태코드도 같이 보낼 수 있는 객체입니다. 요청은 AccountController에서 받았으니 이제 그 처리과정을 밟을 차례입니다. AccountService를 살펴봅시다.
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
&nbsp;AccountService에서 선언된 메소드를 보면 Account 객체를 선언하고 생성자를 통해 만든 객체를 Repository의 save메소드로 DB에 저장하는 것을 볼 수 있습니다. 
<hr class="divider">
<h3>레파지토리, Account 객체</h3>
<br> 
&nbsp;우선 Account 객체부터 설명하겠습니다.
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
&nbsp;@Getter나 @Setter, @Builder, @NoArgsConstructor는 아까 전 보았던 롬북의 어노테이션이라 눈에 익겠지만 @Entity와 @Table, @Id, @GeneratedValue</b>는 JPA의 어노테이션이기 때문에 따로 설명하겠습니다. 
<br><br>
&nbsp;@Entity는 선언된 클래스가 테이블과 링크될 클래스임을 나타내는 어노테이션이며, @Table은 어떤 테이블로 변환될지를 정하는 어노테이션입니다. 지금은 Table의 이름을 accounts로 정한 상태입니다. @Id는 테이블의 인덱스성 기본키를 만드는 어노테이션이며, <b>@GeneratedValue</b>가 튜플이 생성될 시 기본키를 하나씩 더해주는 역할을 합니다.
<br><br>
&nbsp;CreationTimestamp, UpdateTimestamp 둘 다 하이버네이트에서 제공하는 어노테이션입니다.으로 테이블에 새로운 객체가 저장될 시 생성시간, 수정시간을 기록해주는 어노테이션입니다. 이렇게 생성된 Class를 Repository에 등록하면 아래와 같이 됩니다.
<br><br>
```java
public interface AccountRepository extends JpaRepository<Account,Long>{
}
```
<br><br>
&nbsp;선언한 Repository에 extends로 JPA의 기능인 JpaRepository을 붙여 Account를 참조시키면 DB와 상호작용 할 수 있습니다. 위 accountRepository.save()의 save가 JpaRepository에서 제공하는 메소드입니다. 그 밖에 제공하는 다양한 메소드도 사용 할 수 있습니다. 자 이제 서버를 실행하고 요청을 보내보겠습니다.
<br><br>
![sendRequestTosaveAccount](/files/jpa/sendRequestTosaveAccount.png)
<br><br>
&nbsp;아직 응답값을 넣지 않아 Body가 비어 성공적으로 응답합니다. 하지만 이걸로는 성공한 건지 아닌 건지 잘 모르겠거니와 DB에 저장이 제대로 되었는지 확인할 길이 없기에 H2 콘솔을 통해 직접 확인해보겠습니다.
<hr class="divider">
<h3>H2 콘솔</h3>
<br> 
![springh2consoletrueyml](/files/jpa/springh2consoletrueyml.png)
<br><br>
&nbsp;위와 같은 application.yml 혹은 application.property를 찾아 프로퍼티를 등록합니다. 그리고 서버를 재 실행후 <b><a href="http://localhost:8080/h2-console">H2 Console</a></b>에 접속해 봅시다.
<br><br>
![h2console](/files/jpa/h2console.png)
<br><br>
&nbsp;위와 같은 화면이 뜰 것입니다. 접속하면 Entity로 생성한 테이블이 보일 텐데 정면에 쿼리를 입력할 수 있으니 우리가 요청한 것이 적용됬는지 확인해볼 수 있습니다.
<br><br>
![comfirmInputingRequest](/files/jpa/comfirmInputingRequest.png)
<br><br>
&nbsp;요청한 대로 잘 입력된 것을 확인할 수 있습니다. 확인해 본 결과 값을 보면 비밀번호가 저렇게 들어가 있으면 안된다는 생각이 들 것입니다. 다음 포스트에 우리는 스프링 시큐리티라는 프레임워크기술을 적용 시켜 보안에 문제가 없도록 해보겠습니다.
<div style="display:none;">
이 다음 할 것으로 가상머신의 소개와 CentOS소개 및 설치 Mysql 소개와 설치, 연결할까? -- 당장은 안필요할듯
아니면 Spring security 소개와 적용 할까? -- 적용 한다고 해도 테스트할 페이지를 만들어야되, themeleaf로 샘플이 있긴 하지
아니면 프론트 엔드 vuejs의 소개와 간단한 로그인 페이지 생성 할까?
Spring security 먼저 themeleaf로 로그인 페이지 만들고 그걸 vuejs로 전환하는 방식으로
</div>