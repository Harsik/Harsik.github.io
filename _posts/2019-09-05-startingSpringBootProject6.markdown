---
layout: post
title: "SpringBoot 프로젝트 시작하기 6 - Vue 어플"
date: 2019-09-05
categories: SpringBoot
tags: Javascript Vue Vuetify FetchAPi
comments: true
---
<div style="display:none;">
프론트 엔드에 해당하는 웹어플 만들기 
웹어플에서 백서버로 로그인 시도해보기
웹어플 로그인 요청 시, 프론트 엔드와 백엔드로 나뉘었을 때 문제점 기술
</div>
<h3>App 구성하기</h3>
<br>
&nbsp;자 이제 본격적으로 어플리케이션을 구성해보겠습니다
<br><br>
지난 포스트에서 Rest API를 이용한 백엔드 서버를 구축하고 JPA를 활용하여 Account를 DB에 저장하였습니다. 그 당시 요청을 보내기 위하여 포스트맨을 사용했지만 오늘은 지금부터 만들 어플로 요청을 보낼 것입니다. 
<hr class="divider">
<h3>폼 구성하기</h3>
<br>
&nbsp;그렇다면 어플로 무엇을 해야 할지, 어떤 화면을 만들어야 할 지 감이 올 것입니다. 독자들이 어떤 화면을 만들건 자유이지만 당장 떠오르는 것은 로그인 폼 화면을 만드는 것입니다. 목적은 정해졌으니 어떻게 만드는 지만 정하면 됩니다.
<br><br>
![searchForm](/files/vuetify/searchForm.png)
<br><br>
&nbsp;뷰티파이 홈페이지에서 검색해 보면 위와 같은 결과가 나옵니다.
<br><br>
![basicForm](/files/vuetify/basicForm.png)
<br><br>
&nbsp;<b><></b>를 눌러보면 해당 코드가 나옵니다. 이걸 고스란히 Helloworld.vue 옮겨 보겠습니다. 그리고 우리가 필요한 형태로 바꾸어 보겠습니다.
```javascript
<template>
  <v-form>
    <v-container>
      <v-row>
        <v-col cols="12" md="4">
          <v-text-field v-model="email" :rules="emailRules" label="E-mail" required></v-text-field>
        </v-col>

        <v-col cols="12" md="4">
          <v-text-field
            v-model="password"
            :rules="passwordRules"
            :counter="8"
            type="password"
            label="Password"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="4">
          <v-btn @click="onLogin">Login</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
  export default {
    data: () => ({
      valid: false,
      firstname: '',
      password: '',
      passwordRules: [
        v => !!v || 'password is required',
        v => v.length >= 8 || 'password must be at least 8 characters',
      ],
      email: '',
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid',
      ],
    })
  }
</script>
```
&nbsp;코드를 변경하면 아래와 같이 화면이 만들어질 것입니다. 
<br><br>
![loginForm](/files/vuetify/loginForm.png)
<br><br>
<hr class="divider">
<h3>버튼 추가하기</h3>
<br>
&nbsp;이메일과 비밀번호를 입력할수 있지만 아직 로그인 버튼이 아직 없습니다.
<br><br>
![loginForm](/files/vuetify/button.png)
<br><br>
&nbsp;검색결과로 나온 버튼 코드를 추가해보고 페이지에서 클릭해 보면 아무런 작동을 하지 않습니다. 아직 우리가 버튼을 눌렀을 시 작동할 메소드를 정하지 않았기 때문입니다. 로그인 버튼 태크에 @click="onLogin"를 추가하고 script 내에 method: {}를 추가 시키고 핵심적인 부분 즉 서버와 통신을 구현하면 됩니다. 구현하는 방법에서 등장하는 것이 바로 Fetch API입니다.
<br><br>
<hr class="divider">
<h3>Fetch API</h3>
<br>
&nbsp;Fetch API는 ES6에서 등장한 방식으로 프로미스와 함께 에이젝스를 대체할 수 있는 API입니다. ES6란 ECMA Script 6의 줄임말로 자바스크립트와 같은 스크립트 언어들을 표준화하기 위한 규격이라 할 수 있습니다. 버전 6에 이르러서 나온 Fetch API는 비동기 처리 특징을 그대로 가져와 강화 시킨 프로미스를 사용합니다. 
<br><br>
&nbsp;프로미스에 대한 자세한 설명은 <b><a href="https://joshua1988.github.io/web-development/javascript/promise-for-beginners/">여기</a></b>를 참고하길 바랍니다. 잘 이해가 되지 않는 분들은 에이젝스라고 생각해도 다르지 않습니다. 바로 지금 필요한 기능을 구현함으로써 배워보겠습니다.
<br><br>
```javascript
   onLogin () {
      fetch('http://localhost:8080/saveAccount', {
        method: 'POST',
        body: JSON.stringify({ 'email': this.email, 'password': this.password }), // data can be `string` or {object}!
        headers: { 'Content-Type': 'application/json' }
        }).then(response => {
        response.json()
      })
    }
```
<br><br>
&nbsp; 이미 이 웹 어플리케이션을 실행하는 주소가 <u>localhost:8080</u>입니다. 백엔드 서버 또한 <u>localhost:8080</u>입니다. vue-cli는 웹어플 실행시 8080가 사용중이면 다른 포트번호로 실행 시켜준다. 먼저 서버를 실행하고 웹어플을 실행 시켜보겠습니다.
<br><br>
&nbsp;둘다 잘 실행 됬다면 이제 요청을 받을 준비를 하겠습니다. 저번 시간엔 서버가 잘 받도록 Postman에서 파라미터를 잘 맞춰보았습니다. 이번엔 앱에서 오는 요청을 잘 처리 하도록 서버가 맞춰보도록 하겠습니다.
<br><br>
```java
    @PostMapping("/saveAccount")
    public ResponseEntity<?> saveAccount(@RequestBody LoginRequest loginRequest) {
        accountService.saveAccount(loginRequest);
        return ResponseEntity.ok("");
    }
```
<br><br>
&nbsp;앱에서 오는 요청은 JSON이므로 자바에서 이를 처리할 작업이 필요합니다. @RequestBody는 요청으로 오는 JSON 바디를 자바 객체화 시켜줍니다. 
<br><br>
```java
@Getter
@Setter
@Builder
@AllArgsConstructor
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class LoginRequest {

    @NotBlank
    private String email;

    @NotBlank
    private String password;

}
```
<br><br>
&nbsp;객체화된 JSON은 위에 선언된 Class로 변환되며 service 또한 변환된 것과 호환 되도록 변경해야 합니다.
<br><br>
```java
     public void saveAccount(LoginRequest loginRequest) {
                Account account = new Account(loginRequest.getEmail(), loginRequest.getPassword());
                accountRepository.save(account);
        }
```
<br><br>
&nbsp;이렇게 해도 아직 안됩니다. 왜냐하면 SOP(Same-Origin Policy, 동일 출처 정책)이 있기 때문입니다.
<br><br>
<hr class="divider">
<h3>SOP란</h3>
<br>
&nbsp;브라우저에서 보안상의 이유로 프로토콜, 포트, 호스트가 같은 동일 Origin에서만 데이터 전송이 가능하도록 하는 정책입니다. 이 것 때문에 아래 같은 짜증나는 상황에 처한 경우가 있습니다.
<br><br>
![noCorsError](/files/vuetify/noCorsError.png)
<br><br>
&nbsp;그로인해 스프링 프레임워크에서 지원하게 된 어노테이션이 있으니 그것이 바로 @CrossOrigin입니다. 요청을 받는 메소드 위에 @CrossOrigin를 선언해 보겠습니다. 그리고 웹 서버 실행 후 어플에서 요청을 보내 보겠습니다. 별 문제가 없다면 H2 콘솔에서 계정이 제대로 저장된 것이 보일 것입니다. 
<br><br>
![selectFromAccounts](/files/vuetify/selectFromAccounts.png)
<br><br>
&nbsp;저장된 것을 토대로 불러와서 비교하고 어설픈 로그인 기능도 만들 수 있습니다. 앞으로 스프링 시큐리티를 적용할 예정이므로 이번 시간은 로그인 기능 구현의 초입이라고 볼 수 있습니다. 다음 포스트는 좀 더 심화적인 부분을 다루어 보도록 하겠습니다.
<div style="display:none;">
</div>