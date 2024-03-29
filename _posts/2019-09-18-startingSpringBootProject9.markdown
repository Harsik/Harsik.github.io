---
layout: post
title: "SpringBoot 프로젝트 시작하기 9 - 뷰 라이터와 사인업"
date: 2019-09-18
categories: SpringBoot
tags: Vue VueRouter Yarn
comments: true
---
<div style="display:none;">
Vue Router 설명과 적용
Yarn 설명과 설치
button components link
Vue App에서 Signup 기능 구현
</div>
<h3>뷰 라우터(Vue Router)란</h3>
<br>
&nbsp;<b><a href="https://router.vuejs.org/kr/">뷰 라우터(Vue Router)</a></b>란 Vue.js를 사용한 싱글 페이지 앱을 쉽게 만들 수 있도록 도와줍니다. 또한 사용자가 원하는 곳에다 원하는 방식으로 원하는 페이지를 보여지게 할 수 있으며 이해하기 쉬운 간단한 방법도 제공해 줍니다. 자세한 내용을 링크를 통해 알아보고 바로 실전으로 넘어가봅시다. 기존에 사용하던 프론트앱에다가 뷰 라우터를 적용시킬 것입니다.
<br><br>
&nbsp;우선 뷰 라우터를 사용하기 위해서 패키지 설치가 필요합니다. 라이브러리를 직접 다운로드 받아서 링크를 거는 방법도 있지만 더 편리한 방법인 npm install도 있습니다. 하지만 더 빠르고 성능 좋은 방법인 Yarn이 있습니다.
<br><br>
<hr class="divider">
<h3>Yarn 설치하기</h3>
<br>
&nbsp;<b><a href="https://yarnpkg.com/lang/en/">Yarn</a></b>은 예전 포스트에서도 나왔었지만 Yarn이란 NPM와 같은 패키지 매니져이며 공식 홈페이지나 다른 블로그에서도 NPM에 비해 장점이 많다고 소개하고 있습니다. 
그 중 인상 깊은 장점은 작업 병렬화를 통해 빠른 다운로드 및 설치가 가능하다는 점입니다. 여러분들에게 충분히 어필할 만한 매력이라고 생각하며 바로 설치해보겠습니다. 홈페이지에서 설치 마법사를 다운받아 설치하는 방법도 있지만 더 간단한 방법이 있습니다. 바로 npm을 통해 설치하는 방법입니다.
<br><br>
```
npm install --global yarn
```
<br><br>
&nbsp;node.js가 설치되 있다면 위 한줄로 설치 할 수 있습니다. 설치가 완료 되면 yarn을 사용하면 됩니다. 아까 미루었던 뷰 라우터 설치를 Yarn으로 해봅시다.
<br><br>
```
yarn install vue-router
```
<br><br>
&nbsp;설치가 완료 됬으면 현재 프로젝트에 적용시킬 차례입니다. 우선 화면에서 어느부분에 다른 페이지를 출력했으면 좋겠는지 결정하고 해당 위치에 <router-view> 태그를 넣어봅시다. 저의 경우 기존 Component가 있던 자리를 대체하였습니다.
<br><br>
```
 <v-content>
      <!-- <HelloWorld/> -->      
      <router-view></router-view>
  </v-content>
```
<br><br>
&nbsp;그 다음"<"router-view">"에 표시할 페이지들을 정의하고 그걸 목록으로 남겨야 합니다. 테스트를 위해 기존 Helloworld.vue를 복사해 Login.vue와 Signup.vue로 만들어 봅시다. 그리고 목록을 만들 차례입니다.
<br><br>
```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import HelloWorld from '../components/HelloWorld'
import Login from '../components/Login'
import Signup from '../components/Signup'

Vue.use(VueRouter)
export default new VueRouter({
	mode: 'history',
	routes: [
		{
			path: '/',
			name: 'HelloWorld',
			component: HelloWorld
		},
		{
			path: '/Login',
			name: 'Login',
			component: Login
		},
		{
			path: '/Signup',
			name: 'Signup',
			component: Signup
		},
		{
			path: '*',
			redirect: '/'
		}
	]
})
```
<br><br> 
각 컴포넌트들의 위치와 주소, 이름을 정의 하였습니다. Vue.use()를 통해 
VueRouter를 플러그인으로 추가합니다.
<br><br>
```javascript
import Vue from 'vue'
import App from './App.vue'
import vuetify from './plugins/vuetify'
import router from './router'

Vue.config.productionTip = false

new Vue({
	vuetify,
	router,
	render: h => h(App)
}).$mount('#app')

```
<br><br> 
&nbsp;작성한 index를 main.js에 추가하여 선언하면 끝납니다. index에서 정의했던 주소를 찾아가보면 해당 컴포넌트가 화면에 표시되는 것을 확인 할 수 있습니다. 
<br><br>
![VueRouterLogin](/files/vuetify/VueRouterLogin.png)
<br><br>
<hr class="divider">
<h3>v-toolbar</h3>
<br>
&nbsp;하지만 일일히 주소를 치면서 들어가는건 매우 불편합니다. 로그인 화면을 열어주는 버튼이 필요합니다. 저번 포스트에서 로그인 버튼을 만들 때처럼 버튼을 만들 것이지만 좀 다른 버튼을 만들 것입니다. 뷰티파이 홈페이지에서 버튼을 검색했던 것처럼 toolbar를 검색해 봅시다.
<br><br>
![Toolbar](/files/vuetify/Toolbar.png)
<br><br> 
&nbsp;여기에 우측 LINK1처럼 로그인 버튼으로 만들 것입니다. v-btn의 to 프로퍼티가 주소를 입력하는 부분입니다. 스크립트를 참고하여 기존의 프로젝트에 추가 시켜봅시다. 
<br><br>
![addToLoginButton](/files/vuetify/addToLoginButton.png)
<br><br>
&nbsp;클릭해보면 로그인 화면으로 이동할 것입니다. 뷰 라우터에 대해 설치 및 기초응용까지 해보았습니다. 자신감이 좀 붙었다면 이제 사인업 기능을 구현하고 버튼으로 달고 페이지 만들고 해봅시다.
<br><br>
<hr class="divider">
<h3>사인업 기능 구현하기</h3>
<br>
&nbsp;여러분들 중에서는 로그인 화면에도 아이디와 비밀번호 입력 칸이 있는데 로그인 화면에다가 사인업 넣어도 되지 않느냐라고 생각하는 사람이 있을 수 있습니다. 하지만 저처럼 좀 더 도전적인 것을 원한다면 페이지를 따로 만들고 이메일 입력칸에 가입시 기존에 있는 이메일인지 체크하거나, Fetch 중이면 로딩바 같은 시각적인 도형을 추가한다던가 해보시길 바랍니다. 그러한 이유에서 사인업 페이지를 따로 만들려고 하는 것입니다. 로그인처럼 사인업 버튼을 만들고 임시로 만들었던 사인업 파일을 적절히 수정하면 됩니다.
<br><br>
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
          <v-btn @click="onSignup">Signup</v-btn>
        </v-col>
        {{ response }}
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
import { signup } from './APIUtils'

export default {
  data: () => ({
    response: '',
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
  }),
  methods: {
    onSignup () {
        localStorage.email = this.email
        const signupRequest = {
          email: this.email,
          password: this.password
        }
        signup(signupRequest)
          .then(response => {
            this.response = response
          })
    }
  }
}
</script>
```
<br><br> 

&nbsp;가입이 성공했다는 메세지를 출력하기 위해 화면상에 변수를 출력하도록 하였습니다.

<br><br>
![SignupSuccess1](/files/vuetify/SignupSuccess1.png)
<br><br>
<br><br>
![SignupSuccess1](/files/vuetify/SignupSuccess2.png)
<br><br>
 
&nbsp;워크벤치에서도 아이디가 추가된 것을 확인 할 수 있습니다.











