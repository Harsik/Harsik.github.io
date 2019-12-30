---
layout: post
title: "SpringBoot 프로젝트 시작하기 1 - VSCode설치와 시작"
date: 2019-08-28
categories: SpringBoot
tags: VSCode SpringBoot
---
<hr class="divider">
<h3>동기와 글이 지향하는 방향</h3>
<br>
&nbsp;글을 쓰는건 정말 힘든일이다. 지금 당장 내가 쓰고 싶은 글에서 무엇부터 시작해야할지도 모르지 않는가. 하지만 지금 쓰다보면 글자가 쌓여 글이 되고 글이 쌓여 블로그가 되어 바닷모래를 모아 성을 만드는 것처럼 내가 쓰고 싶은 글이 될 수 있을 것이다. 
<br><br>
![모래성 이미지](/files/sandCastle.jpeg)
<br><br>
&nbsp;어쩌면 내가 참고 했던 사이트는 이것이것이며 이걸 참고 하면 될 것이다라고 넘어 가면 편할거라고도 생각하지만 그러면 내게 남는게 뭐가 있을까 내것이 되는건 몇 없을 것이다. 
<br><br>
&nbsp;이제부터 내가 제작했던 토이 프로젝트에 대해 설명할 것이다. 
<hr class="divider">
<h3>토이 프로젝트</h3>
<br>
&nbsp;이 프로젝트는 5월 초부터 7월 초까지 약 2달간 제작하였으며 백엔드 프로젝트와 프론트엔드 프로젝트를 동시 진행하였다.
<br><br>
&nbsp;백엔드 프로젝트는 스프링부트를 사용해 제작하였으며 프론트엔드 프로젝트는 Vue.js를 사용하여 제작하였다. 이 프로젝트를 만들게된 계기는 팀장님께 다음 프로젝트에 대비하기 위해 다루어야 할 도구와 언어를 익히기 위해서이다. 
<br><br>
&nbsp;그렇다면 백엔드를 만들기 위한 환경설정을 무엇을 해야 하는가. 우선 구현하고자 하는 기능을 어떤 기술로 적용시킬 지를 정해야한다. 아래에 차례대로 설명하겠지만 간략하게 말하자면 전체적인 프레임워크는 스프링부트를 사용할 것이며, 로그인, 로그아웃과 같은 보안 기능은 스프링시큐리티로 DB간 상호작용은 JPA, Hibernate, DB는 MYSQL을 사용하며, 빌드를 위한 프로젝트 관리 도구는 Maven이 아닌 Gradle를 사용해 보기로 하였다. 빌드 후 구동을 위한 WAS는 Tomcat이 아닌 undertow를 사용한다.
<br><br>
&nbsp;이전까지 나는 사용한 IDE는 이클립스를 제외하곤 전무했으나 이제 실행하는 것만 5분이 걸리는 툴을 사용하기 싫어졌다. 그러다가 <b><a href="https://jojoldu.tistory.com">기억보다 기록을</a></b>이란 블로그를 참고하기 시작하면서 인텔리제이나 VSCode와 같은 도구를 사용하기 시작했고 지금은 거의 이클립스를 대체할 수 있게 되었다. 그리고 위 블로그에서 나는 스프링부트에 대해 많이 배울 수 있었다. 블로그에서는 인텔리제이를 사용하나 내가 이 프로젝트를 만드는데 사용한 것은 VSCode이니 이것에 대해 간단한 설명과 설치방법 그리고 사용방법에 대해 알려주도록 하겠다.
<hr class="divider">
<h3>VSCode 설치하기</h3>
<br>
&nbsp;VSCode에 간단하게 설명하자면 MS사에서 만든 확장성 있는 텍스트 에디터 즉 메모장 같은 것이다. 그래서 IDE는 아니지만 가지고 있는 확장성으로 인해 IDE처럼 사용할 수 있다. <b><a href="https://code.visualstudio.com/">여기</a></b>에 홈페이지 링크가 있다. 홈페이지에 접속하면 아래와 같이 다운로드 버튼이 있다. 버튼만 누르면 다운로드는 시작된다. 그리고 설치하면 된다.
<br><br>
![VSCodeDownloadPage](/files/VSCode/VSCodeDownloadPage.png)
<br><br>
그리고 아래와 같이 설치과정을 밟으면 된다.
<br><br>
![InstallVSCode1](/files/VSCode/InstallVSCode1.png)
<br><br>
![InstallVSCode2](/files/VSCode/InstallVSCode2.png)
<br><br>
![InstallVSCode3](/files/VSCode/InstallVSCode3.png)
<br><br>
![InstallVSCode4](/files/VSCode/InstallVSCode4.png)
<br><br>
&nbsp;위 사항을 모두 체크하는 것을 추천한다. 탐색기에서 우클릭 시 Open with Code가 나오는데 이게 꽤 편리한 기능이기 때문이다.
<br><br>
![InstallVSCode5](/files/VSCode/InstallVSCode5.png)
<br><br>
![InstallVSCode6](/files/VSCode/InstallVSCode6.png)
<br><br>
![InstallVSCode7](/files/VSCode/InstallVSCode7.png)
<br><br>
&nbsp;자 모든 설치 과정을 마치고 실행하면 아래와 같은 화면이 뜰 것이다.
<br><br>
![InstallVSCodeComplete](/files/VSCode/InstallVSCodeComplete.png)
<br><br>
<hr class="divider">
<h3>VSCode로 스프링부트 시작하기</h3>
<br>
&nbsp;이제 스프링부트를 위한 확장 프로그램을 설치할 차례이다. <b><a href="https://code.visualstudio.com/docs/java/java-spring-boot">여기</a></b> 홈페이지에서 스프링부트를 사용하기 위한 방법이 적혀있다. 정리하자면 아래와 같다.
<br><br>
<h2>미리 준비할 것</h2>
<ul>
<li>Java Development Kit (JDK), version 1.8.</li>
<li>Apache Maven, version 3.0 or later.</li>
<li>Java Extension Pack</li>
</ul>
<br><br>
<h2>설치할 확장프로그램</h2>
<ul>
<li>Spring Boot Tools</li>
<li>Spring Initializr</li>
<li>Spring Boot Dashboard</li>
</ul>

&nbsp;아래의 그림을 참고하여 설치하며 install 버튼을 누르면 설치가 진행 된다.
<br><br>
![VSCodeSpringBootExtension](/files/VSCode/VSCodeSpringBootExtension.png)
<br><br>

&nbsp;그리고 간단한 프로젝트를 만들어 main으로 가보면 
<br><br>
![SpringBootRun](/files/VSCode/SpringBootRun.png)
<br><br>

&nbsp;확장 프로그램으로 인해 프로젝트를 간단하게 실행 할 수 있도록 버튼이 추가되는 것을 볼 수 있다. 




