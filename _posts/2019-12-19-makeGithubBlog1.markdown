---
layout: post
title: "깃허브 블로그(GitHub Blog) 만들기 (1/7)"
date: 2019-12-19
categories: Github
tags: Jekyll
---
<div style="display:none;">
</div>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>동기와 글이 지향하는 방향</h3>
<br>
이 포스트에 시작은 다소 뜬끔 없지만 글을 어떻게 써야되는지 고민하는 것부터 시작한다. 어떻게 하면 좀 더 찾기 쉽고 읽기 쉬우며 이해가 쉬운 3便을 충족시킬 수 있을까 생각한다. 방금 전에 3便도 3가지 쉬운점을 요약 표현하기 위해 적었으나 어쩌면 findable, readable, understandable 표현하는 것이 나을 수도 있다고 생각한다. 
<br><br>
이런 사소한 부분부터 글의 전체적인 틀을 어떻게 짜는지 고민한다. 예를 들면 글을 작성하면 주제 본문으로 끝 마칠 수 있지만 소주제를 추가하고 소본문을 추가하여 각각을 모아 본문을 구성하는 방식으로 만들 수도 있다. 하지만 글을 작성하는 목적을 정해야 글의 방향을 정할 수 있다. 
<br><br>
필자는 이 블로그를 내 기술과 지식을 남기고 전하기 위해 만들었으므로 정보 전달이 주 목적이 된다. 그러므로 글 또한 주 목적이 같다. 그렇게 생각하면 뉴스나 신문에서 작성하는 글이 내가 작성하는 글과 방향이 같다고 할 수 있다. 그것들 또한 정보 전달이 주 목적이기 때문이기에 내가 작성하는 글들은 뉴스나 신문의 형태를 토대로 만들어야 할 것이다. 그리고 이 글을 읽는 독자들은 서술하는 기술들을 거의 처음 접하는 사람들로 생각하고 글을 작성할 생각이다. 서론이 길었으니 바로 시작하겠다. 
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>깃허브 블로그</h3>
<br>
![makeGithubBlog1](/files/makeGithubBlog/makeGithubBlog1.png)
<br><br>
우선 깃허브 블로그가 무엇인지부터 설명하려 한다. 깃허브 블로그란 이름에서도 알 수 있다시피 깃허브에서 제공하는 블로그 서비스를 말한다. 하지만 우리가 기존에 접하던 네이버 블로그나 다음 블로그 혹은 구글의 블로거(Blogger)와는 사뭇다르다. 그것들은 오직 블로그 서비스만을 위한 기능들을 제공하기 때문에 정해진 목록, 포스트, 댓글 등만을 제공하여 스타일이 거의 고정되어 있고 내부 시스템이 어떻게 작동하고 있는지는 보여주지 않는다.
<br><br>
하지만 깃허브 블로그는 깃허브란 저장 공간과 지킬이란 웹사이트 엔진만을 제공하기 때문에 사용자가 사이트의 토대부터 만들 수 있다. 한편으론 매력적으로 느낄 수 있으며 다른 한편으론 매우 불편하게 느낄 수 있다. 단지 글을 올리고 저장만된다면 상관없다고 생각하는 사람에게는 그럴 수 있다. 하지만 필자가 깃허브 블로그를 사용하고 만들면서 겪어왔던 고생과 수고 그리고 노력을 쏟아부어 느낄 수 있던 보람은 말이 이루어 말할 수 없다. 
<br><br>
하지만 늘 시간이 부족한 사람들을 위해 이번글은 아주 간단하게 깃허브 블로그를 만들 수 있는 방법을 소개하겠다. 그렇기에 깃허브가 무엇이며 지킬엔진이 무엇인지는 설명하지 않겠다.
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>루비젬 설치</h3>
<br>
<b><a href="https://jekyllrb-ko.github.io/">지킬 홈페이지</a></b>에 들어가보면 평범한 텍스트 파일을 정적 웹사이트 또는 블로그로 변신시켜 보세요.라는 글과 빠른 시작 방법에 대해 알려준다. 
<br><br>
![makeGithubBlog2](/files/makeGithubBlog/makeGithubBlog2.png)
<br><br>
우리는 단지 터미널에 위 그림에 있는 명령어들을 입력하기만 하면 된다. 하지만 지금은 준비가 아직 덜 되었기에 위 명령어를 입력해도 명령어를 인식할 수 없다는 오류가 뜰 것이다. gem은 루비젬의 명령어이기 때문에 루비젬을 우선 설치해야 한다. 
<br><br>
윈도우를 기준으로 <b><a href="https://rubyinstaller.org/">루비 인스톨러</a></b>사이트에서 파일을 다운 받은 후 설치해야 하며 다른 운영체제라면 <b><a href="https://www.ruby-lang.org/ko/documentation/installation/">루비 홈페이지</a></b>를 참고하길 바란다. 루비를 설치파일에는 루비젬이 같이 있기에 설치 후 gem명령어를 사용 할 수 있다.
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>지킬 번들러 설치</h3>
<br>
이제 준비가 끝났으니 터미널에 gem install bundler jekyll 입력하여 지킬 번들러를 설치해 보자. 
<br><br>
![makeGithubBlog2](/files/makeGithubBlog/makeGithubBlog2.png)
<br><br>
<pre>
Microsoft Windows [Version 10.0.18362.535]
(c) 2019 Microsoft Corporation. All rights reserved.

C:\Users\Achivsoft\Documents\githubblog>gem install bundler jekyll
Fetching: bundler-2.1.2.gem (100%)
Successfully installed bundler-2.1.2
Parsing documentation for bundler-2.1.2
Installing ri documentation for bundler-2.1.2
Done installing documentation for bundler after 32 seconds

ㆍ
ㆍ
ㆍ

Successfully installed jekyll-4.0.0
Parsing documentation for jekyll-4.0.0
Done installing documentation for jekyll after 1 seconds
2 gems installed
</pre>
<br><br>
다음은 jekyll new my-awesome-site이다. gem을 통해 설치된 jekyll을 자체 명령어를 통해 웹사이트 템플릿을 제공할 수 있다.
<pre>
C:\Users\Achivsoft\Documents\githubblog>jekyll new my-awesome-site
Running bundle install in C:/Users/Achivsoft/Documents/githubblog/my-awesome-site... 
  Bundler: Fetching gem metadata from https://rubygems.org/...........
  Bundler: Fetching gem metadata from https://rubygems.org/.
  Bundler: Resolving dependencies...

ㆍ
ㆍ
ㆍ

  Bundler:
  Bundler: For more info see:
  Bundler: https://github.com/svenfuchs/i18n/releases/tag/v1.1.0
New jekyll site installed in C:/Users/Achivsoft/Documents/githubblog/my-awesome-site.
</pre>
<br><br>
그 다음은 웹서버를 실행하는 일이다. 위에 나온것처럼 bundler exec jekyll serve를 입력하여 실행해보자.
<pre>
C:\Users\Achivsoft\Documents\githubblog>cd my-awesome-site

C:\Users\Achivsoft\Documents\githubblog\my-awesome-site>bundler exec jekyll serve
Configuration file: C:/Users/Achivsoft/Documents/githubblog/my-awesome-site/_config.yml
            Source: C:/Users/Achivsoft/Documents/githubblog/my-awesome-site
       Destination: C:/Users/Achivsoft/Documents/githubblog/my-awesome-site/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 3.362 seconds.
 Auto-regeneration: enabled for 'C:/Users/Achivsoft/Documents/githubblog/my-awesome-site'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
[2019-12-20 11:17:02] ERROR `/favicon.ico' not found.
</pre>
<br><br>
서버 어드레스 주소를 브라우저를 통해 접근하면 만들어진 사이트 화면이 보일 것이다.
<br><br>
![makeGithubBlog3](/files/makeGithubBlog/makeGithubBlog3.png)
<br><br>
제대로 만들어 졌다고 생각하면 깃허브에 [깃허브아이디].github.io라는 이름의 레파지토리를 생성하여 업로드 해보아라. 그리고 https://[깃허브아이디].github.io/에 접속하면 방금 만든 웹사이트가 보일 것이다. 간단한 템플릿을 만드는 것은 여기까지이다. 템플릿을 통해 포스트를 추가하거나 태그와 카테고리 기능을 추가하는 등은 다음 포스트부터 다룰 것이다. 
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">