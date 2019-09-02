---
layout: post
title: "Spring Boot JPA"
date: 2019-08-30
categories:
---
<div style="display:none;">
SPring security 설명을 위한 사전 작업 중 하나로 jpa를 통한 db 유저 정보 등록 및 조회로 login logout을 구현 할 수 있도록 설명
초반 Spring Boot jpa에 대한 간략한 소개와 설명
중반 jpa 사용방법 h2
후반 jpa 응용 방법 mysql
<b><a href="https://rogerdudler.github.io/git-guide/index.ko.html">여기</a></b>
<br><br>
![gitDownload](/files/git/gitDownload.png)
<br><br>
</div>
우선 <b>JPA</b>가 뭐지라는 생각과 함께 고개를 기울이는 독자들을 위해서 간략히 설명하면서 시작하겠다. 

<br><br>
![JPA&ORM](/files/JPA&ORM.png)
<br><br>

<b>JPA</b>란
자바 <b>ORM</b>기술에 대한 API 표준 명세를 의미한다. 여기서 <b>ORM</b>란 JAVA 객체가 DB의 테이블이 되도록 매핑 시켜주는 것을 말한다. 즉 <b>Class => Table</b>이다. <b>JPA</b>를 사용하면 쿼리를 통한 조회 또한 메서드 호출로 변경하여 사용할 수 있다. 예를 들어 <b>SELECT * FROM user;</b>라는 쿼리는 <b>user.findAll()</b>가 같이 바꿔 쓸 수 있다는 것이다. 쿼리를 만들지 않고 메서드 호출만 해도 되니 생산성이 매우 높아진다. 다만 복잡한 쿼리는 메서드 호출로 감당할 수 없다는 문제점을 동반한다. <b>Oracle DB 피벗테이블</b> 같은 건 사용하지 못한다. 
<br><br>
필자는 이전까지 <b>Mybatis</b>나 <b>Ibatis</b>를 주요 사용했었다. 하지만 <b>Spring Boot</b>를 시작하고 <b>JPA</b>를 사용해 보니 앞에 것과 비교했을 때 매우 편리하며 그리고 속도차이도 거의 없는 기술이라는 것을 체감할 수 있었다.
<br><br>
이제 <b>JPA</b>를 구현한 프레임워크 중 하나인 <b>Hibernate</b>를 사용해 볼 것이다. 우선 사용하고자 하는 의존성패키지를 추가해야한다. 

<br><br>
![dependencies](/files/jpa/dependencies.png)
<br><br>

어디서 본 것 같다고 생각한다면 착각이 아니다. <b>Spring Boot Initiailizer</b>로 프로젝트를 처음 생성했을 때를 기억하는가? 처음 생성할 때 추가한 <b>Dependency</b>에 있던 것들이다. 고로 따로 수정할 사항은 없다. 
<br><br>
여기서 <b>Lombok</b>은 뭐길래 들어가 있지라는 생각을 할 수 있다. 그래서 <b>Lombok</b>에 대한 간략한 설명을 하겠다. User라는 Class 객체에서 <b>name</b>이라는 변수에 접근하기 위해서 일반적으로 Class 내에서 <b>getName, setName</b>이라는 메소드를 만들어 사용한다. 근데 이게 변수가 한 두개면 상관없는 데 변수가 늘어나는 대로 그것도 두배로 늘어나니 선언을 한건지 안한건지도 모르겠고 코드는 복잡하기만 하고 짜증이 난다. 줄여 말하자면 <b>가독성</b>이 떨어진다. 이런 시원치 않은 짓을 하지 않도록 어노테이션을 추가하여 해결한 것이 <b>Lombok</b>이다. 좀 더 보기 편하게 쓰자면
<br><br>

```java
public class User {
    private String name;
    public String getName(){
        return this.name;
    }
    public void setName(String name){
        this.name=name;
    }
}
```
<br><br>
->
<br><br>
```java
@Getter
@Setter
public class User {
    private String name;
}
```
