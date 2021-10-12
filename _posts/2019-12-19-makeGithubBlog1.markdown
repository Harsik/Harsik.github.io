---
layout: post
title: "깃허브 블로그(GitHub Blog) 만들기 1 - 지킬 설치하기"
date: 2019-12-19
categories: Github
tags: 지킬
comments: true
---
<div style="display:none;">
</div> 
<h3>깃허브 블로그</h3>
<br>
![makeGithubBlog1](/files/makeGithubBlog/makeGithubBlog1.png)
<br><br>
&nbsp;우선 깃허브 블로그가 무엇인지부터 설명하려 합니다. 깃허브 블로그란 이름에서도 알 수 있다시피 깃허브에서 제공하는 블로그 서비스를 말합니다. 하지만 우리가 기존에 접하던 네이버 블로그나 다음 블로그 혹은 구글의 블로거(Blogger)와는 다릅니다. 앞에 것들은 오직 블로그 서비스만을 위한 기능들을 제공하기 때문에 정해진 목록, 포스트, 댓글 등만을 제공하여 스타일이 거의 고정되어 있고 내부 시스템이 어떻게 작동하고 있는지는 보여주지 않습니다.
<br><br>
&nbsp;반면에 깃허브 블로그는 깃허브란 저장 공간과 지킬이란 웹사이트 엔진을 제공하기 때문에 사용자가 사이트의 토대부터 만들 수 있습니다. 한편으론 매력적으로 느낄 수 있으며 다른 한편으론 매우 불편하게 느낄 수 있습니다. 하지만 깃허브 블로그를 사용하고 만들면서 생긴 고생과 노력은 보람있다고 할 수 있습니다. 
<br><br>
&nbsp; 시간이 부족한 사람들을 위해 깃허브 블로그를 만들 수 있는 방법을 간단히 풀어 소개할려고 합니다.
<hr class="divider">
<h3>루비젬 설치</h3>
<br>
&nbsp;<b><a href="https://jekyllrb-ko.github.io/">지킬 홈페이지</a></b>에 들어가보면 평범한 텍스트 파일을 정적 웹사이트 또는 블로그로 변신시켜 보세요.라는 글과 빠른 시작 방법에 대해 알려줍니다.
<br><br>
![makeGithubBlog2](/files/makeGithubBlog/makeGithubBlog2.png)
<br><br>
&nbsp;사용자들은 터미널에 위 그림에 있는 명령어들을 입력하기만 하면 됩니다. 하지만 지금은 준비가 아직 덜 되었기에 위 명령어를 입력해도 명령어를 인식할 수 없다는 오류가 뜰 것입니다. gem이라는 명령어 사용하기 위해서 루비젬을 우선 설치해야 합니다. 
<br><br>
&nbsp;윈도우를 기준으로 <b><a href="https://rubyinstaller.org/">루비 인스톨러</a></b>사이트에서 파일을 다운 받은 후 설치해야 하며 다른 운영체제라면 <b><a href="https://www.ruby-lang.org/ko/documentation/installation/">루비 홈페이지</a></b>를 참고하길 바랍니다. 루비 설치파일에는 루비젬이 같이 있기에 설치 후 gem명령어를 사용 할 수 있습니다.
<hr class="divider">
<h3>지킬 번들러 설치</h3>
<br>
&nbsp;이제 준비가 끝났으니 터미널에 'gem install bundler jekyll' 입력하여 지킬 번들러를 설치해 봅시다. 
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
&nbsp;다음 명령어는 jekyll new my-awesome-site입니다. gem을 통해 설치된 지킬을 자체 명령어를 통해 웹사이트 템플릿을 제공할 수 있습니다.
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
&nbsp;그 다음은 웹서버를 실행하기 위해서 위에 나온것처럼 bundler exec jekyll serve를 입력해봅시다.
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
&nbsp;서버 어드레스 주소를 브라우저를 통해 접근하면 만들어진 사이트 화면이 보일겁니다.
<br><br>
![makeGithubBlog3](/files/makeGithubBlog/makeGithubBlog3.png)
<br><br>
&nbsp;제대로 만들어 졌다고 생각하면 깃허브에 [깃허브아이디].github.io라는 이름의 레파지토리를 생성하여 업로드 해서 https://[깃허브아이디].github.io/에 접속하면 방금 만든 웹사이트가 보일겁니다. 템플릿을 통해 포스트를 추가하거나 태그와 카테고리 기능을 추가하는 등은 다음 포스트부터 설명하도록 하겠습니다. 
