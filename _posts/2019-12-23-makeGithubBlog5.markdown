---
layout: post
title: "깃허브 블로그(GitHub Blog) 만들기 (5/7)"
date: 2019-12-23
categories: Github jekyll
tags: jekyll Jekyll Jekyll
---
<div style="display:none;">
태그 만들기
</div>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>태그 만들기 계획</h3>
<br>
&nbsp;태그를 어떻게 만들까? 일단 태그 기능에 대한 정의가 필요하다. 필자가 만들고 싶은 태그 기능은 
<ul>
<li>각 포스트에 태그라는 포스트를 나타내는 푯말을 추가하는 것</li>
<li>그것을 포스트 내부와 그리고 외부, 리스트에서 표현하는 것</li>
<li>태그를 통해 게시물을 찾아갈 수 있도록 하는 것</li>
<li>태그를 검색하여 게시물을 찾아가는 것들을 가능하게 하는 것</li>
</ul>
&nbsp;이상이다. 차례차례 하나씩 구현해보자.
<br><br>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>포스트 푯말 추가</h3>
<br>
&nbsp;푯말 아니 태그가 편하니 태그로 통일하여 말하겠다. 이 태그는 필자가 여러 게시물들을 참고하건데 주제 밑 부주제 혹은 포스트 날짜 옆에 붙여 넣는 것이 가장 적합하다고 생각한다. 
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
       <a class="label">#{{ tag }}</a> &nbsp;
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
&nbsp;page 그러니깐 우리 만들어 놓은 포스트에서 정보들을 받아 표시하고 태그를 가져와 반복적으로 입력하게 만들었다.
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
&nbsp;포스트에 tags를 추가 하였다. 화면으로 보면 아래와 같다.
<br><br>
![makeGithubBlog23](/files/makeGithubBlog/makeGithubBlog23.png)
<br><br>
&nbsp;필자가 원하는 위치에 있지만 단순히 글자만 있어 보기 좋지는 않다. 좀 더 보기 좋게 바꾸어 보자.

<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>스타일 개선</h3>
<br>
&nbsp;인스타 그램의 태그처럼 태그 앞에 #문자를 넣고 스타일을 변경하는 방향으로 가볼려고 한다. 거기에 태그임을 나타내는 태그 아이콘이 있다면 더욱이 좋다. 아이콘은 구글 메테리얼 아이콘을 사용할 것이며 사용방법에 대해서 아래에 기술하겠다. 온라인으로 CSS 파일을 연결하기 위해서 아래의 헤더 파일에 아래의 코드를 추가한다.
{% highlight html %}
<link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
{% endhighlight %}
&nbsp;그리고 <b>[메테리얼 디자인 아이콘 페이지][materialIcon]</b>에 접속하여 필요로 하는 아이콘을 찾으면 된다.
<br><br>
![makeGithubBlog24](/files/makeGithubBlog/makeGithubBlog24.png)
<br><br>
&nbsp;위 사진대로 우리가 필요로 하는 태그 모양의 아이콘을 찾았다. 아이콘 밑에 사용방법에 대해 나와있으니 그대로 추가만 해주면 된다. 
<br><br>
&nbsp;추가한 결과로 아이콘이 너무 크게 나왔다. 아이콘의 크기를 줄이기 위해서 그리고 앞으로 스타일 변경을 위해서 기존에 사용하던 라이브러리 CSS 파일을 따로 빼내어 사용하도록 하겠다.

{% highlight css %}
/** Page Header */
.label { font-weight: initial; }
{% endhighlight %}

<br><br>
![makeGithubBlog25](/files/makeGithubBlog/makeGithubBlog25.png)
<br><br>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>외부 태그</h3>
<br>
&nbsp;기초적으로 태그를 다는 것과 스타일까지 적용시켰으니 이제 포스트 외부에 태그를 달고 태그 리스트를 만들어볼 것이다. 그 전에 지킬 엔진의 최신 버전에서는 index.html가 아닌 index.md로 확장자를 변경해야 제대로 인덱스 파일이 인식하기에 파일명을 변경하길 바란다.
<br><br>
&nbsp;포스트 외부 즉 처음 접속 시 볼 수 있는 첫 화면과 카테고리 화면 등에 태그를 표시하려고 한다. 아래의 코드를 보면서 설명하겠다. 
<br><br>
<h4>index.md</h4>
{% raw %}
~~~html
---
layout: default
---
<div class="home">
	<h2 class="post-list-heading">최근 게시물</h2>
	<ul class="post-list">
		{% for post in site.posts %}
		<li>
			<span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
			{% if post.tags %} 
			<i class="material-icons svg-icon">local_offer</i>
			{% for tag in post.tags %}
			<a class="label" href="{{ '/tags' | prepend: site.baseurl }}">#{{ tag }}</a> &nbsp; 
			{% endfor %} 
			{% endif %}
			<h3>
				<a class="post-link" href="{{ post.url | prepend: site.baseurl }}"
					>{{ post.title }}</a
				>
			</h3>
		</li>
		{% endfor %}
	</ul>
</div>
~~~
{% endraw %}
<br><br>
&nbsp;사이트 오브젝트에서 포스트 오브젝트를 리퀴드의 반복문을 통해 포스트의 목록을 만들었었는데 그것을 응용하여 포스트 오브젝트에서 태그 오브젝트를 반복문으로 나타내었다. 위에서 포스트에서 태그 만들 때 사용한 방법과 동일한 방법으로 태그를 만들었다. 또한 태그를 클릭할 경우 태그 리스트로 접속하도록 하이퍼링크를 추가하였다.
<br><br>
![makeGithubBlog26](/files/makeGithubBlog/makeGithubBlog26.png)
<br><br>
&nbsp;카테고리 페이지 또한 동일한 방식으로 해결하면 된다.
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>태그 리스트</h3>
<br>
&nbsp;태그 리스트는 사이트가 가지고 있는 모든 태그들을 보여주고 각 태그마다 속해 있는 포스트명과 링크를 목록으로 보여주기 위해 만들 것이다. 아래 코드를 보면서 설명을 시작하겠다.
<br><br>
<h4>tags.md</h4>
{% raw %}
~~~html
---
layout: default
title: Tags
permalink: /tags/
---
{% assign rawtags = "" %}
{% for post in site.posts %}
{% assign ttags = post.tags | join:'|' | append:'|' %}
{% assign rawtags = rawtags | append:ttags %}
{% endfor %}
{% assign rawtags = rawtags | split:'|' | sort %}
{% assign tags = "" %}
{% for tag in rawtags %}
{% if tag != "" %}
{% if tags == "" %}
{% assign tags = tag | split:'|' %}
{% endif %}
{% unless tags contains tag %}
{% assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' %}
{% endunless %}
{% endif %}
{% endfor %}
{% for tag in tags %}
<a class="label label-success" href="#{{ tag | slugify }}" >#{{ tag }}</a> &nbsp;
{% endfor %}
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
{% for tag in tags %}
<h4 id="{{ tag | slugify }}">{{ tag }}</h4>
<ul>
  {% for post in site.posts %}
  {% if post.tags contains tag %}
  <li>
      <a href="{{ post.url | prepend: site.baseurl }}">
        {{ post.title }}
      </a>
  </li>
  {% endif %}
  {% endfor %}
</ul>
{% endfor %}
~~~
{% endraw %}
<br><br>
포스트에서 태그들을 변수로 정의하고 리퀴드 필터를 통해 재가공되어진다. <b>[리퀴드][liquidPage]</b> 홈페이지에서 태그와 필터에 대한 설명을 볼 수 있다. 
<br><br>
assign 태그를 통해 불러온 tags를 '|'문자로 재결합하고 if와 unless 태그를 통해 상황에 맞게 적용할 수 있도록 만든다. sort 필터는 재결합된 tags를 split필터로 분할할때 정렬된 변수값으로 만들어 준다. 이렇게 만든 tags를 반복문을 통해 tags 별로 포스트를 보여주는 리스트를 만들 수가 있다.
<br><br>
![makeGithubBlog27](/files/makeGithubBlog/makeGithubBlog27.png)
<br><br>
각 포스트명에는 포스트의 하이퍼링크가 걸려 있어 해당 포스트로 이동하게 된다.
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">









[liquidPage]: https://shopify.github.io/liquid/ 
[materialIcon]: https://material.io/resources/icons/?icon=local_offer&style=baseline