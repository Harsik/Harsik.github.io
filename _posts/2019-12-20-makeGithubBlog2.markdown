---
layout: post
title: "깃허브 블로그(GitHub Blog) 만들기 2 - 템플릿 수정하기"
date: 2019-12-20
categories: Github
tags: Jekyll Liquid
comments: true
---
<div style="display:none;">
</div>
<h3>템플릿 구조</h3>
<br>
&nbsp;저번 포스트에서 지킬이 제공하는 템플릿을 받아 웹사이트를 실행해 보았습니다. 이번 포스트에서는 그 템플릿이 어떤 구조로 되어 있으며 각 구조들이 어떤 역할을 하는지 알아보겠습니다.
<br><br>
![makeGithubBlog4](/files/makeGithubBlog/makeGithubBlog4.png)
<br><br>
&nbsp;위 사진이 템플릿의 디렉토리 구조입니다. 복잡하게 볼 것 없이 위 디렉토리에서 사용되어지는 것은 _config.yml, _posts정도입니다. jekyll-cache와 _site는 엔진 실행의 결과물입니다. 화면을 다듬을려면 좀 더 많은 파일들이 필요합니다.
<br><br>
![makeGithubBlog5](/files/makeGithubBlog/makeGithubBlog5.png)
<br><br>
&nbsp;위 사진에서 볼 수 있는 것처럼 메인화면을 구성하는 _layouts, 포스트화면을 담당하는 _includes, 모든 스타일을 담당하는 _sass, 메인화면 파일인 index.html이 있어야 합니다. 
<br><br>
<h4>index.html</h4>
{% raw %}
~~~html
---
layout: default
---
<div class="home">
  <h1 class="page-heading">{{ site.title }}</h1>
  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
        <h3><a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}<a></h3>
        <br>
      </li>
    {% endfor %}
  </ul>
</div>
~~~
{% endraw %}
<br><br>
<h4>_layouts/default.html</h4>
{% raw %}
~~~html
<!DOCTYPE html>
<html>
  <body>
    {% include header.html %}
    <div class="page-content">
      <div class="wrapper">
        {{ content }}
      </div>
    </div>
    {% include footer.html %}
  </body>
</html>
~~~
{% endraw %}
<br><br>
<h4>_layouts/post.html</h4>
{% raw %}
~~~html
---
layout: default
---
<div class="post">
  <header class="post-header">
     <h1 class="post-title p-name" itemprop="name headline">{{ page.title }}</h1>
     <p class="post-meta">Posted on {{ page.date | date: "%b %-d, %Y" }}
     </p>
  </header>
  <div class="post-content e-content" itemprop="articleBody">
    {{ content }}
  </div>
</div>
~~~
{% endraw %}
<br><br>
<h4>_includes/footer.html</h4>
{% raw %}
~~~html
<div class="footer center">
  Copyright &copy; <a href=https://Harsik.github.io target="_blank">Harsik</a> 2019<BR />
  Powered by <a href=http://jekyllrb.com target="_blank">Jekyll</a>
</div>
~~~
{% endraw %}
<br><br>
<h4>_includes/header.html</h4>
{% raw %}
~~~html
<div class="site-header">
    <nav class="site-nav">
      <a href="#" class="menu-icon">
        <i class="fa fa-navicon fa-lg"></i>
      </a>
      <div class="trigger">
          <a class="page-link" href="{{ site.baseurl }}/">Home</a>
          <a class="page-link" href="{{ site.baseurl }}/about">About</a>
      </div>
    </nav>
</div>
~~~
{% endraw %}
<br><br>
&nbsp;일단 위에 있는 파일들만 넣어 실행해보도록 하겠습니다.
<br><br>
![makeGithubBlog6](/files/makeGithubBlog/makeGithubBlog6.png)
<br><br>
&nbsp;스타일이 전혀 적용되지 않아서 정렬이나 색상이 표시되지 않았습니다. 스타일을 알아서 지정해도 좋지만 스타일을 구할 수 있는 방법이 있습니다.
<hr class="divider">
<h3>지킬 테마들</h3>
<br>
&nbsp;수 많은 사람들이 지킬을 사용하는 것처럼 지킬도 수 많은 테마들이 있습니다. 직접 찾아 나설 수도 있으며 <b><a href="http://jekyllthemes.org/">JekyllThemes</a></b>라는 사이트를 이용할 수도 있습니다. 다운받은 파일에서 _sass나 assets폴더만 가져다 내 지킬 폴더에 넣으면 지킬 엔진이 파일들을 해당 폴더 맞는 위치로 _site안에 넣어줍니다.
<br><br>
&nbsp;그럼 이제 테마 중 하나를 골라 적용 시켜봅시다. 저는 위 사이트에 있던 테마 중 'Texture'를 사용하겠습니다.
<br><br>
![makeGithubBlog7](/files/makeGithubBlog/makeGithubBlog7.png)
<br><br>
&nbsp;파일들을 옮기고 한 가지 작업이 더 남았습니다. 바로 default.html에 css파일을 링크하는 작업입니다. 
<h4>_layouts/default.html</h4>
{% raw %}
~~~html
<!DOCTYPE html>
<html>
  <head>
  <link rel="stylesheet" href="{{ "/assets/css/style.css" | prepend: site.baseurl }}">
  </head>
  <body>
    {% include header.html %}
    <div class="page-content">
      <div class="wrapper">
        {{ content }}
      </div>
    </div>
    {% include footer.html %}
  </body>
</html>
~~~
{% endraw %}
<br><br>
![makeGithubBlog9](/files/makeGithubBlog/makeGithubBlog9.png)
<br><br>
&nbsp;기본적으로 제공하는 테마를 사용해보겠습니다.
<br><br>
![makeGithubBlog8](/files/makeGithubBlog/makeGithubBlog8.png)
<br><br>