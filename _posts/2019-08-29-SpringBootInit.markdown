---
layout: post
title: "Spring Boot 시작하기"
date: 2019-08-29
categories:
tags: SpringBoot
---
이번 글은 <b>Spring Boot</b>의 시작하는 방법에 대해서 적어볼려고 한다. 
<br><br>
Spring 재단은 처음 프로젝트를 생성할때 편의를 제공해 주는 도구로 <b><a href="https://start.spring.io/">Spring Boot Initialzer</a></b>라는 것을 제공한다. 
<br><br>
![SpringInitializer](/files/SpringInitializer.png)
<br><br>
일단 간단한 웹 어플리케이션부터 만들어 보자. <b>Dependency</b>는 아래에 그림과 같이 추가하며 추후 Gradle을 통해 차차 늘려나갈 예정이다.
<br><br>
![SpringDependencies](/files/SpringDependencies.png)
<br><br>
물론 간단한 웹 앱을 만드는 데 저리 많은 <b>Dependency</b>는 필요 없지만 앞으로 추가할 것들을 미리 추가한 것이다. 이제 하단에 <b>Generate the project</b> 버튼을 눌러 생성된 프로젝트 파일을 다운로드 하자. 
<br><br>
압축을 풀고 안에 소스를 확인해 보면 실행만 가능한 상태인데 거기에 <b>RestController</b>를 넣어 제대로 실행되고 있는지 확인할 것이다. 아래와 같이 폴더와 자바파일을 추가한다.
<br><br>
![addWebController](/files/addWebController.png)
<br><br>
그리고 메인클래스에서 Run버튼을 눌려 실행하면 Spring Boot가 실행된다는 문구가 터미널 혹은 출력으로 나올 것이다.
<br><br>
![SpringBootRun](/files/SpringBootRun.png)
<br><br>
특이한 일이 없으면 <b><a href="http://localhost:8080/hello">localhost:8080/hello</a></b>에 접속하여 <b>RestController</b>에 넣어놨던 코드가 정상 실행된 것을 확인 할 수 있다.
<br><br>
![HelloWorld](/files/HelloWorld.png)
<br><br>

