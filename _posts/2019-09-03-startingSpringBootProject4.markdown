---
layout: post
title: "SpringBoot 프로젝트 시작하기 4 - 스프링 시큐리티"
date: 2019-09-03
categories: SpringBoot
tags: SpringBoot SpringSecurity Gradle
---
<div style="display:none;">
우선 spring security의 소개
간단한 웹 어플로 security 적용 확인 설명
프론트 엔드와 백엔드로 나뉘었을 때 문제점 기술
프론트 엔드에 해당하는 웹어플 만들기 
</div>
<hr class="divider">
<h3>스프링 시큐리티 소개</h3>
<br> 
&nbsp;우리는 웹 서비스는 물론 기타 다른 서비스를 이용하기 위해 대부분 아이디, 비밀번호를 사용한다. 그 서비스가 자체적으로 제공할 수도 있지만 스프링에서는 스프링 시큐리티라는 프레임워크를 제공한다. 
<br><br>
&nbsp;스프링 시큐리티가 어쩌고 하면서 세세한 부분에서 설명할 수 있겠지만 우리는 시간이 부족한 사람들이니 스프링 홈페이지에서 제공하는 <b><a href="https://spring.io/guides/gs/securing-web/">가이드</a></b>를 통해 간단한 웹어플리케이션을 만들면서 설명해보도록 하겠다.
<br><br>
![SpringSecurityGettheCode](/files/security/SpringSecurityGettheCode.png)
<br><br>
&nbsp;다운로드 버튼을 눌러 다운 받으면 가이드에서 제공하는 웹어플리케이션을 다운 받을 수 있다. 압축을 풀고 나면 아래와 같이 나오는데 그 중 initial 폴더로 작업하기 바란다. 혹시 막히거나 구체적인 답이 필요할 때는 complete폴더에서 구하기 바란다.
<br><br>
![initialFileTree](/files/security/initialFileTree.png)
<br><br>
&nbsp;친절하게도 메이븐과 그래들 둘다 지원되도록 만들어져 있다. 독자들은 좀 더 편한쪽을 선택해 진행하면 된다. 그대로 Application.java에 들어가 웹어플을 실행 시켜보자. 
<br><br>
<b><a href="https://http://localhost:8080/">웹서버</a></b>에 접속해 보면 간단하게 2페이지로만 구성되어 있다. 의존성 또한 아직 스프링 시큐리티가 없다. 본 가이드에서는 여기에 /hello로 &nbsp;접근을 제어하는 것을 스프링 시큐리티로 만들려고 하는 것이다. 
<br><br>
&nbsp;우선 의존성부터 추가해보자. 필자는 그래들이 더 편하기에 그래들로 진행하도록 하겠다.
<br><br>
![addDependencySecurity](/files/security/addDependencySecurity.png)
<br><br>
&nbsp;위와 같이 의존성 추가 후 업데이트를 해야 의존성이 적용된다.
<br><br>
```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/", "/home").permitAll()
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .loginPage("/login")
                .permitAll()
                .and()
            .logout()
                .permitAll();
    }

    @Bean
    @Override
    public UserDetailsService userDetailsService() {
        UserDetails user =
             User.withDefaultPasswordEncoder()
                .username("user")
                .password("password")
                .roles("USER")
                .build();

        return new InMemoryUserDetailsManager(user);
    }
}
```
<br><br>
&nbsp;@Bean과 @Configuration을 제외하면 모두 시큐리티의 어노테이션이다. 
<br><br>
&nbsp;Config Class에 WebSecurityConfigurerAdapter를 extend로 붙여 HttpSecurity객체를 통해 보안 설정을 정의하고 있다. 설정은 "/"와 "/home"에 대한 접근은 인증 없이 접근 가능하도록 하며 나머지 요청에 대해서는 인증을 요하도록 되어있다. 또한 loginPage와 logout에 대한 설정을 포함하고 있다. 
<br><br>
&nbsp;위에 방식으로 보면 헷갈릴 수 있는데 xml로 표현하면 이런식이다.
<br><br>
```xml
<http use-expressions="true"> 
<intercept-url pattern="/**" access="hasRole('USER')" /> 
<form-login login-page="/login" /> 
<logout logout-url="/logout" /> 
<csrf disabled="false"/> 
</http>
```
<br><br>
&nbsp;필자는 전자의 방식이 이해가 안되다가 후자의 방식으로 보고 전자가 왜 이런 형태를 띄게 됬는지 이해했다. 독자들 중에 필자와 같은 고민을 가지고 있었다면 이걸로 해결할 수 있길 바란다. 
<br><br>
&nbsp;그 다음 UserDetails은 스프링 시큐리티가 사용자의 정보를 담고 있는 객체이다. 기본적으로 제공하는 User 객체로 비밀번호 암호화기능이 있으며 사용자의 접근 제한을 설정할 수 있다. 스프링 시큐리티가 사용자 정보를 처리 하는 UserDetailsService 메소드에 @Override 선언을 하여 InMemoryUserDetailsManager을 반환하도록 하면 서버 메모리 상에 사용자의 아이디와 비밀번호를 저장하여 해당 아이디, 비밀번호로 인증 할 수 있게 된다. 그 다음 로그인 할 수 있도록 로그인 페이지가 필요하기에 로그인 페이지를 아래와 같이 만들겠다.
<br><br>
```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="https://www.thymeleaf.org" xmlns:sec="https://www.thymeleaf.org/thymeleaf-extras-springsecurity3">
    <head>
        <title>Spring Security Example</title>
    </head>
    <body>
        <h1>Welcome!</h1>
        
        <p>Click <a th:href="@{/hello}">here</a> to see a greeting.</p>
    </body>
</html>
```
<br><br>
&nbsp;모든 준비가 끝났으니 이제 웹어플을 실행하여 접속해 보자. 그리고 인증된 상태에서 접근 가능한 주소와 인증되지 않은 상태에서 접근 가능하지 않은 주소를 확인해 보아라.
<br><br>
![completeHome](/files/security/completeHome.png)
<br><br>
![completeLogin](/files/security/completeLogin.png)
<br><br>
&nbsp;사전에 코드상에 설정값으로 입력했던 user, password를 입력하면 로그인 할 수 있다.
<br><br>
![completeHello](/files/security/completeHello.png)
<br><br>
&nbsp;설정에서는 "/"와 "/home", "/login"에 접근하는 것은 permitall 즉 허용되어 있다. 인증하지 않아도 말이다. 하지만 그 외에는 인증을 필요로 하도록 설정하였기에 독자들은 한번 로그인 하기 전이나 로그아웃 후 "/hello"에 접근해 보아라. 바로 로그인화면으로 튕겨져 나올 것이다. 스프링 시큐리티가 제대로 적용되고 있는 것이다. 
<br><br>
&nbsp;어떻게 적용하는지 작동하는지에 대해서는 대략적으로 알았으나 이것으로 부족하다고 생각하면 <b><a href="https://spring.io/guides/topicals/spring-security-architecture">여기</a></b>나  <b><a href="https://sjh836.tistory.com/165">빨간색소년</a></b>님 블로그를 참고하길 바란다. 스프링 시큐리티가 실질적으로 어떻게 작동하는지 알 수 있다. 나중에 다시 심화적으로 SpringSecurity를 다뤄 보도록 하겠다.
<div style="display:none;">
</div>