---
layout: post
title: "SpringBoot 프로젝트 시작하기 5 - Vue.js"
date: 2019-09-04
categories: SpringBoot
tags: Javascript Vue Vuetify NPM
---
<div style="display:none;">
프론트 엔드에 해당하는 웹어플 만들기 
웹어플 로그인 요청 시, 프론트 엔드와 백엔드로 나뉘었을 때 문제점 기술
</div>
<hr class="divider">
<h3>웹은 진화하고 있다</h3>
<br> 
<br><br>
![javascript](/files/vuetify/javascript.png)
<br><br>
&nbsp;필자가 자바스크립트 처음 접했을 때가 대학교 2학년 때였는데 그 때만 해도 자바스크립트는 HTML 문서를 동적으로 움직이기 위한 도구에 불과했다. 하지만 지금에 와서 보면 엄청난 진보가 눈에 띌 정도이다. 자바스크립트는 단지 웹에 한정되지 않고 네트워크와 서버, DB와 모바일에까지 도달하는 막대한 영역을 가지게 되었다. 우리는 그 중 일부인 Vue.js를 다루어볼려고 한다.
<hr class="divider">
<h3>Vue.js란</h3>
<br> 
&nbsp;간단히 설명하면 웹 UI 프레임워크로 페이지를 구축하는 프론트엔드 언어이다. 즉 화면을 만드는 언어이다. 이와 비슷한 역할을 하는 것이 React.js, Angular.js 등이 있다. 우리가 흔히 홈페이지에서 볼 수 있는 왼쪽에 메뉴가 있고 누르면 팝업되며 어떤 버튼들로 기능을 작동시키는 그런 모든 것들을 할 수 있다. 
<br><br>
&nbsp;스프링 시큐리티 포스트에서는 페이지를 만드는 데 HTML을 사용했으며 스프링부트에서 제공하는 타임리프를 통해 웹 문서를 조작했다. 서버에서는 저장되어 있는 문서들을 스프링 MVC 프레임워크를 사용하여 주소에 따라 화면에 나타내어 줬었지만 이번엔 그 것에서 벗어나 백엔드 서버와 프론트엔드 서버를 따로 구축하고 화면만 동작하는 웹 어플리케이션을 만들 것이다. 
<br><br>
&nbsp;Vue.js를 사용하지만 이 것만 사용하는 건 밋밋하고 예쁘지가 않다. 그렇기에 CSS 프레임워크를 더해 사용할 것이다. CSS 프레임워크에도 다양한 종류가 있다. vuetify, element, keen ui 등등이 있는데, 우리는 그 중 뷰티파이(Vuetify)를 사용할 것이다. 
<br><br>
![VuetifyLayout](/files/vuetify/VuetifyLayout.png)
<hr class="divider">
<h3>뷰티파이란</h3>
<br> 
&nbsp;뷰티파이는 메터리얼 디자인(material design)을 사용하는 CSS 프레임워크로 구글스러운 느낌을 준다. <b><a href="https://vuetifyjs.com/ko/">홈페이지</a></b>가 매우 잘 되어 있으며 가이드도 충실하고 한글도 지원한다. 그 밖에 템플릿도 제공하는데, 처음 시작할 때 많은 도움을 받을 수 있다. 
<br><br>
&nbsp;우선 홈페이지에 나와 있는대로 따라해 보겠다. 
```
$ yarn global add @vue/cli
// OR
$ npm install @vue/cli -g
```
&nbsp;위 문장을 보고 도대체 어떻게 하는지 감이 오지 않는 사람들이 있다. 쉘 및 터미널 환경에 익숙지 않은 사람들인데 필자도 얼마 전까지 그랬다. 하지만 사용하기 어렵지 않으니 지금부터 해보자. 
<hr class="divider">
<h3>NPM 시작하기</h3>
<br> 
&nbsp;일단 NPM이란 것이 필요하다. NPM이란 node.js package manager로 생산된 패키지들을 관리해 주는 매니저 프로그램이라 볼 수 있다. NPM을 사용하기 위해서 우선 <b><a href="https://www.npmjs.com/get-npm">여기</a></b>를 참고하길 바란다. 여기서 node.js를 설치 할 수 있는데 윈도우에서는 설치 마법사로 설치 가능하다. 설치 과정이 복잡하지 않으며 next, install하면 설치가 완료된다. 설치를 하면 NPM도 같이 사용할 수가 있다. yarn도 설치 할 수 있지만 필수적이지는 않다. 
<br><br>
&nbsp;이제 설치도 완료 했겠다. 터미널에서 명령어를 쳐야되는데 VSCode는 자체 터미널 기능을 제공하기에 <i>ctrl + `</i>으로 터미널을 열어 명령어를 쳐보자. 
<br><br>
![npmInstallVuecli](/files/vuetify/npmInstallVuecli.png)
<br><br>
&nbsp;꽤 편리 하지 않은가? NPM을 통해 vue cli라는 패키지가 설치 되었다. 이제 vue cli 명령어로 my-app이라는 vue templete을 만들어보자.
```
$ vue create my-app
$ cd my-app
```
vue create 명령어를 실행 시 선택지가 나올텐데 Default로 고르면 된다. 설치가 완료되면 <i>cd my-app</i>으로 my-app 디렉토리에 들어가 아래의 명령어를 실행하면 웹어플리케이션이 실행된다.
```
$ yarn serve
// OR
$ npm run serve
```
<br><br>
![vueCreateApp](/files/vuetify/vueCreateApp.png)
<br><br>
&nbsp;하지만 아직 뷰티파이가 적용된 건 아니다. <i>ctrl + c</i>를 눌러 서버를 끄고 아래의 코드를 입력해 적용시켜보자. 선택지에서 똑같이 Default로. 
<br><br>
![serialworkFinish](/files/vuetify/serialworkFinish.png)
<br><br>
```
$ vue add vuetify
```
&nbsp;설치가 끝나고 다시 실행하면 아래와 같이 뷰티파이가 적용된 것을 볼 수 있다.
<br><br>
![addVuetifyToApp](/files/vuetify/addVuetifyToApp.png)
<br><br>
&nbsp;생각보다 글이 길어져서 컴포넌트를 적용한 어플리케이션을 만드는 것은 다음 포스트로 미루겠다.
<div style="display:none;">
중요 문구에 굵게, 명령어에 이텔릭, 머릿말 활용하기, 슬슬 포스트가 많아진다. 카테고리 만들기
</div>