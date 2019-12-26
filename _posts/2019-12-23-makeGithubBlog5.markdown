---
layout: post
title: "깃허브 블로그(GitHub Blog) 만들기 (5/7)"
date: 2019-12-23
categories: Github
tags: 
---
<div style="display:none;">
태그 만들기
</div>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>태그 만들기 계획</h3>
<br>
태그를 어떻게 만들까? 일단 태그 기능에 대한 정의가 필요하다. 필자가 만들고 싶은 태그 기능은 
<ul>
<li>각 포스트에 태그라는 포스트를 나타내는 푯말을 추가하는 것</li>
<li>그것을 포스트 내부와 그리고 외부, 리스트에서 표현하는 것</li>
<li>태그를 통해 게시물을 찾아갈 수 있도록 하는 것</li>
<li>태그를 검색하여 게시물을 찾아가는 것들을 가능하게 하는 것</li>
</ul>
이상이다. 차례차례 하나씩 구현해보자.
<br><br>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>포스트 푯말 추가</h3>
<br>
푯말 아니 태그가 편하니 태그로 통일하여 말하겠다. 이 태그는 필자가 여러 게시물들을 참고하건데 주제 밑 부주제 혹은 포스트 날짜 옆에 붙여 넣는 것이 가장 적합하다고 생각한다. 
<br><br>
<h4>post.html</h4>
{% raw %}
~~~html
---
layout: default
---
<div class="post">
  <header class="post-header">
     <h1 class="post-title p-name" itemprop="name headline">{{ page.title }}</h1>
     <p class="post-meta">Posted on {{ page.date | date: "%b %-d, %Y" }}
       {% if page.tags %} 
       {% for tag in page.tags %}
       <a class="label">{{ tag }}</a> &nbsp;
       {% endfor %}
       {% endif %}
      </p>
  </header>
  <div class="post-content e-content" itemprop="articleBody">
    {{ content }}
  </div>
</div>
~~~
{% endraw %}
<br><br>
page 그러니깐 우리 만들어 놓은 post에서 정보들을 받아 표시하는 데 그 중 태그를 가져와 반복적으로 입력하게 만들었다.
<br><br>
{% highlight html %}
---
layout: post
title:  "MyPost"
date:   2019-12-21
categories:
tags: post demo test
---
{% endhighlight %}
<br><br>
포스트에 tags를 추가 하였다. 화면으로 보면 아래와 같다.
<br><br>
![makeGithubBlog23](/files/makeGithubBlog/makeGithubBlog23.png)
<br><br>


<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">