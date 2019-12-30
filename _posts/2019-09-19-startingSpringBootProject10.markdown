---
layout: post
title: "SpringBoot 프로젝트 시작하기 10 - 로그아웃 구현하기"
date: 2019-09-19
categories: SpringBoot
tags: Vuetify JWT
---
<div style="display:none;">
프론트에서 v-toolbar-items로 로그인 전 후 버튼 및 메뉴 생성
로그인 후 메뉴에서 프로파일 및 로그아웃 추가
로그인 후 Inspire 화면 추가 및 이동
첫 Welcome 화면과 Inspire으로 이동할 수 있도록 Navi 메뉴 생성 

백엔드에서 Profile 기능 추가
RDMS에서 관계를 JPA로 적용시키는 방법 
AuthController 수정
AccountController에 Profile service 제작 및 등록 
</div>
<hr class="divider">
<h3>로그인 후 페이지</h3>
<br>
&nbsp;언제까지 로그인을 확인하기 위해서 화면에 로그로 남기기보다는 만족감을 얻고 싶어서 로그인 후 나를 반겨줄 페이지를 하나 추가할려고 한다. 
<br><br>
```javascript
<template>
  <v-layout align-center justify-center>
    <v-flex>
      <img src="../assets/logo.svg" alt="Vuetify.js" class="mb-5" />
      <blockquote class="text-xs-center">
        &#8220;First, solve the problem. Then, write the code.&#8221;
        <footer>
          <small>
            <em>&mdash;John Johnson</em>
          </small>
        </footer>
      </blockquote>
    </v-flex>
  </v-layout>
</template>

<script>
</script>
<style scoped>
  img {
    width: 10%;
    height: 10%;
    margin-left: auto;
    margin-right: auto;
    display: block;
  }
</style>
```
<br><br> 

&nbsp;이 위에 있는 것과 같은 파일을 생성하여 만들어도 되나 독자들이 하나 만들어보길 권장한다. 뷰 파일을 만들고 router에서 목록을 추가하여 로그인 후 주소를 찾아가게 만들면 된다. onLogin 메소드에서 fetch가 끝난다음 함수에서 this.$router.push('/InspireView')를 추가하자.

<br><br>
```javascript
 onLogin () {
      this.isLoading = true
      localStorage.email = this.email
      let loginRequest = {
        email: this.email,
        password: this.password
      }
      login(loginRequest)
        .then(response => {
          localStorage.accessToken = response.accessToken
          this.$router.push('/InspireView')
        })
    }
```
<br><br>

&nbsp;추가하면 vue-router가 해당 주소에 컴포넌트를 불러 보여준다. 그러므로 로그인 후 InspireView 화면을 볼 수 있다. 

<br><br>
![InspireView](/files/vuetify/InspireView.png)
<br><br>

&nbsp;이 사진을 보여준 것은 다름이 아니라 로그인 후 화면을 어떻게 구성할지에 대해서 설명하기 위해서이다. 위 사진을 보면 우측 위 점3개의 버튼이 보일 것이다. 구글을 크롬에 대한 서브 메뉴를 보여줄 때 저 버튼을 사용한다. 우리도 그와 비슷하게 하기 위해서 로그인을 하면 기존의 버튼들을 교체하고 새로나온 버튼에게 새 기능을 추가 할 것이다. 

<br><br>
<hr class="divider">
<h3>서브 메뉴</h3>
<br>
&nbsp;말로 하면 어려우니 바로 실전으로 들어가겠다. 우선 우리가 가장 먼저 해야하는 일을 정의하면 로그인 성공 시 버튼을 전환하는 작업이다. 일단 원하는 버튼을 추가하자.

<br><br>
```javascript
<v-btn icon>
  <v-icon large>more_vert</v-icon>
</v-btn>
```
<br><br>

&nbsp;구글에서 사용하는 아이콘을 메테리얼 아이콘이라고 한다. <b><a href="https://materialdesignicons.com/">링크</a></b>를 남기니 필요한 것을 찾아 낼 수 있길 바란다. 그 다음 각 버튼이 로그인에 따라 보이거나 안보이게 해야 하기 때문에 로그인 상태를 저장할 수 있는 변수를 만들어야 하며, 그 변수 값을 이용하여 기능을 구현해야한다.

<br><br>
```javascript
<v-toolbar-items v-if="isAuthenticated">
      <v-btn icon>
           <v-icon large>mdi-dots-vertical</v-icon>
     </v-btn>
</v-toolbar-items>
```
<br><br>

&nbsp;뷰의 v-if 기능을 이용하면 간단하게 구현할 수 있다. 위 isAuthenticated값을 로그인에 따라 다르게 해보자. 그 다음 새로 만든 위 아이콘에 메뉴 기능을 추가할 것이다. 메뉴를 추가하기 위해 뷰티파이에서 제공하는 v-menu 컴포넌트를 사용할 것이다.

<br><br>
```javascript
   <v-toolbar-items v-if="isAuthenticated">
          <v-layout align-center justify-space-between>
            <v-menu bottom left offset-y>
              <v-btn slot="activator" icon>
                <v-icon large>mdi-dots-vertical</v-icon>
              </v-btn>
              <v-list>
                <v-list-tile v-for="(subMenu, i) in subMenus" :key="i" :to="subMenu.to">
                  <v-list-tile-title>{{ subMenu.title }}</v-list-tile-title>
                </v-list-tile>
              </v-list>
            </v-menu>
          </v-layout>
        </v-toolbar-items>
```
<br><br>

```javascript
    <v-content>    
      <router-view
        @sendAuthentication="setAuthentication"
      >
              </router-view>
    </v-content>
    
```
<br><br>

```javascript
<script>

export default {
  name: 'App',
  data: () => ({
    isAuthenticated: false,    
    subMenus: [
      { title: 'Profile', to: '/Profile' },
      { title: 'Logout', to: '/Logout' }
    ]
  }),
  methods: {
    setAuthentication (bool) {
      this.isAuthenticated = bool
    }
  }
};
</script>
    
```
<br><br>
&nbsp;필자는 메뉴에 프로파일과 로그아웃를 넣어놨다. 이글 뒤로 두 메뉴에 대한 기능을 구현하기 위해서이다. 그리고 뷰라우터에서 뷰 파일간 통신을 위해 @sendAuthentication="setAuthentication"를 선언하여 하위 컴포넌트에도 setAuthentication메소드를 사용할 수 있도록 하였다. 이렇게 함으로써 Login.vue에서 상위 App.vue의 변수 값을 변경 할 수 있도록 메소드를 제공할 수 있는 것이다. 프로파일를 만들기 전에 우선 로그아웃 기능부터 만들도록 하겠다.    
<br><br>
<hr class="divider">
<h3>로그아웃 기능 구현하기</h3>
<br>
&nbsp;기존 세션-쿠키 방식에서는 로그아웃이란 세션 혹은 쿠키를 없애면 사용자 접속한 세션이 없거나 쿠키가 유효하지 않게 되어 로그아웃으로 처리되게 된다. 하지만 JWT 방식은 세션이 없다. 서버로 다른 클라이언트에서 같은 아이디가 로그인한다고 해서 서버가 이미 로그인 한 아이디임을 알 수 없다. 발급된 토큰을 기준으로 서버 접근이 가능한지 아닌지만 판별한다. 만약 우리가 그래도 로그아웃을 만들고 싶다면 로그아웃 프로세스를 만들고 JWT를 지워 서버에 접근할 수 없도록 해야한다.
<br><br>
![sessionjwtlogout](/files/security/sessionjwtlogout.png)
<br><br>
&nbsp;로그아웃을 위한 뷰 페이지를 하나 만들고 /Logout으로 접근시 로그아웃 프로세스가 작동하도록 만들것이다.

<br><br>

```javascript
<template>
</template>
<script>
export default {
  mounted () {
    this.logout()
  },
  methods: {
    logout () {
      localStorage.accessToken = null
      this.$emit('sendAuthentication', false)
      this.$router.push('/login')
    }
  }
}
</script>
    
```
<br><br>

&nbsp;뷰의 라이프 서클 중 하나인 mounted는 화면이 표시되기 시작한 시점을 말하며 좀 더 자세한 부분은 <b><a href="https://medium.com/witinweb/vue-js-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-7780cdd97dd4">링크</a></b>를 참고하길 바란다. 위 과정으로 jwt는 없어지며 로그인 상태값이 변경되고 로그인 화면으로 넘어오게 된다. 다음은 프로파일 화면을 구성할 차례이다. 하지만 프로파일을 구현하는데 앱과 서버 둘다 설명하면서 하기에 매우 길어질 것 같아 여기까지만 쓰도록 하겠다.
<br><br>