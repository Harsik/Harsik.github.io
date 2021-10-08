---
layout: post
title: "깃허브 블로그(GitHub Blog) 만들기 5 - 태그 기능 구현"
date: 2020-01-01
categories: Github
tags: Jekyll Liquid Javascript
comments: true
---
<div style="display:none;">
태그 만들기
</div>
<h3>태그 만들기 계획</h3>
<br>
&nbsp;제가 만들고 싶은 태그 기능은 아래와 같습니다. 
<ul>
<li>각 포스트에 태그라는 포스트를 나타내는 푯말을 추가하는 것</li>
<li>그것을 포스트 내부와 그리고 외부, 리스트에서 표현하는 것</li>
<li>태그를 통해 게시물을 찾아갈 수 있도록 하는 것</li>
<li>태그를 검색하여 게시물을 찾아가는 것들을 가능하게 하는 것</li>
</ul>
&nbsp;이제 차례차례 하나씩 구현해보도록 하겠습니다.
<br><br>
<hr class="divider">
<h3>포스트 태그 추가</h3>
<br>
&nbsp;태그는 제가 생각하기에 주제 밑 부주제 혹은 포스트 날짜 옆에 붙여 넣는 것이 가장 적합하다고 생각합니다. 
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
&nbsp;page 그러니깐 우리 만들어 놓은 포스트에서 정보들을 받아 표시하고 태그를 가져와 반복적으로 입력하게 만들었습니다.
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
&nbsp;포스트에 tags를 추가 하였습니다. 화면으로 보면 아래와 같습니다.
<br><br>
![makeGithubBlog23](/files/makeGithubBlog/makeGithubBlog23.png)
<br><br>
&nbsp;필자가 원하는 위치에 있지만 단순히 글자만 있어 보기 좋지는 않기에 좀 더 보기 좋게 바꾸어 보겠습니다.

<hr class="divider">
<h3>스타일 개선</h3>
<br>
&nbsp;인스타 그램의 태그처럼 태그 앞에 #문자를 넣고 스타일을 변경하는 방향으로 가볼려고 합니다. 거기에 태그임을 나타내는 태그 아이콘이 있다면 더욱이 좋습니다. 아이콘은 구글 메테리얼 아이콘을 사용할 것이며 사용방법에 대해서 아래에 기술하겠습니다. 온라인으로 CSS 파일을 연결하기 위해서 아래의 헤더 파일에 아래의 코드를 추가하겠습니다.
{% highlight html %}
<link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
{% endhighlight %}
&nbsp;그리고 <b>[메테리얼 디자인 아이콘 페이지][materialIcon]</b>에 접속하여 필요로 하는 아이콘을 찾아 사용하면 됩니다.
<br><br>
![makeGithubBlog24](/files/makeGithubBlog/makeGithubBlog24.png)
<br><br>
&nbsp;위 사진대로 우리가 필요로 하는 태그 모양의 아이콘을 찾았습니다. 아이콘 밑에 사용방법에 대해 나와있으니 그대로 추가만 해주면 됩니다. 
<br><br>
&nbsp;추가한 결과로 아이콘이 너무 크게 나왔습니다. 아이콘의 크기를 줄이기 위해서 그리고 앞으로 스타일 변경을 위해서 기존에 사용하던 라이브러리 CSS 파일을 따로 빼내어 사용하도록 하겠습니다.

{% highlight css %}
/** Page Header */
.label { font-weight: initial; }
{% endhighlight %}

<br><br>
![makeGithubBlog25](/files/makeGithubBlog/makeGithubBlog25.png)
<br><br>
<hr class="divider">
<h3>외부 태그</h3>
<br>
&nbsp;기초적으로 태그를 다는 것과 스타일까지 적용시켰으니 이제 포스트 외부에 태그를 달고 태그 리스트를 만들어볼 것입니다. 그 전에 지킬 엔진의 최신 버전에서는 index.html가 아닌 index.md로 확장자를 변경해야 제대로 인덱스 파일이 인식하기에 파일명을 변경하길 바랍니다.
<br><br>
&nbsp;포스트 외부 즉 처음 접속 시 볼 수 있는 첫 화면과 카테고리 화면 등에 태그를 표시하려고 합니다. 아래의 코드를 보면서 설명하겠습니다. 
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
&nbsp;사이트 오브젝트에서 포스트 오브젝트를 리퀴드 반복문을 통해 포스트의 목록을 만들었는데 그것을 응용하여 포스트 오브젝트에서 태그 오브젝트를 반복문으로 나타내었습니다. 위에서 포스트에서 태그 만들 때 사용한 방법과 동일한 방법으로 태그를 만들었습니다. 또한 태그를 클릭할 경우 태그 리스트로 접속하도록 하이퍼링크를 추가하였습니다.
<br><br>
![makeGithubBlog26](/files/makeGithubBlog/makeGithubBlog26.png)
<br><br>
&nbsp;카테고리 페이지 또한 동일한 방식으로 해결하면 됩니다.
<hr class="divider">
<h3>태그 리스트</h3>
<br>
&nbsp;태그 리스트는 사이트가 가지고 있는 모든 태그들을 보여주고 각 태그마다 속해 있는 포스트명과 링크를 목록으로 보여주기 위해 만들 것입니다.
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
<hr class="divider">
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
&nbsp;포스트에서 태그들을 변수로 정의하고 리퀴드 필터를 통해 재가공되어집니다. <b>[리퀴드][liquidPage]</b> 홈페이지에서 태그와 필터에 대한 설명을 볼 수 있습니다. 
<br><br>
&nbsp;assign 태그를 통해 불러온 tags를 '|'문자로 재결합하고 if와 unless 태그를 통해 상황에 맞게 적용할 수 있도록 만듭니다. sort 필터는 재결합된 tags를 split필터로 분할할때 정렬된 변수값으로 만들어 줍니다. 이렇게 만든 tags를 반복문을 통해 tags 별로 포스트를 보여주는 리스트를 만들 수가 있습니다.
<br><br>
![makeGithubBlog27](/files/makeGithubBlog/makeGithubBlog27.png)
<br><br>
&nbsp;각 포스트명에는 포스트의 하이퍼링크가 걸려 있어 해당 포스트로 이동하게 됩니다.
<hr class="divider">
<h3>검색 기능 추가하기</h3>
<br>
검색창을 만들어서 자동완성된 게시물명이나 태그명을 통해 검색할 수 있는 기능을 만들려고 하였으나 자바스크립트를 통한 동적페이지 구성이 필요하다는 것을 알았습니다. 최대한 자바스크립트가 아닌 html나 지킬, 리퀴드로만 만들어 볼려고 했습니다만 그게 안된다면 어쩔 수 없습니다. 
<br><br>
그래서 저는 TipueSearch라는 라이브러리를 사용할 것입니다.
<br><br>
![makeGithubBlog30](/files/makeGithubBlog/makeGithubBlog30.png)
<br><br>
위 사진은 그 라이브러리를 사용한 모습으로 검색어에 해당하는 문구를 포스트명 뿐만 아니라 포스트 본문에서 찾아 해당하는 포스트를 보여줍니다. 저는 전에 참고했던 CALLICODER의 홈페이지와 비슷하게 만들어 볼려고 합니다.
<br><br>
![makeGithubBlog28](/files/makeGithubBlog/makeGithubBlog28.png)
<br><br>
위 사진에서 검색 아이콘을 클릭하면 아래의 사진처럼 검색칸이 뜨는 형식입니다.
<br><br>
![makeGithubBlog29](/files/makeGithubBlog/makeGithubBlog29.png)
<br><br>
그럼 우선적으로 라이브러리를 추가해야 합니다. <b>[TipueSearch][tipuesearch]</b> 페이지에서 라이브러리를 다운받읍시다.
<br><br>
![makeGithubBlog31](/files/makeGithubBlog/makeGithubBlog31.png)
<br><br>
다운받은 라이브러리에서 위 사진에 해당하는 파일을 프로젝트 내로 옮기고 _config.yml 파일에 환경변수를 추가해야합니다.
<h4>_config.yml</h4>
{% highlight text %}
#Disqus Comment
disqus_url: https://https-harsik-github-io.disqus.com/
disqus_id: aznm4736@gmail.com
{% endhighlight %}
그리고 검색 폼이 추가될만한 곳을 찾아 아래의 코드를 추가하여야 합니다.
<h4>header.html</h4>
{% highlight html %}
<!-- search box -->
		<form action="/search">
			<div class="tipue_search_box">
				<img src="/assets/tipuesearch/search.png" class="tipue_search_icon" />
				<input
					type="text"
					name="q"
					id="tipue_search_input"
					pattern=".{2,}"
					title="최소 2글자 이상"
					required
				/>
			</div>

			<div style="clear: both;"></div>
		</form>
{% endhighlight %}
search.html에 layout을 default로 수정하고 검색어 칸에 검색어를 입력하면 아래의 사진처럼 검색어 입력칸과 검색결과 창이 뜰 것입니다.
<br><br>
![makeGithubBlog32](/files/makeGithubBlog/makeGithubBlog32.png)
<br><br>
기능적인 부분이 완료되었으니 이제 스타일 부분을 고쳐볼 것입니다. 여러분들도 느꼈듯이 쓸대 없이 검색어 입력칸이 하나 더 생긴다는 점과 헤더에 위치해있는 입력칸이 보기 좋지는 않다는 점 그리고 굳이 보여줄 필요 없는 포스트 URL이 보여진다는 점이 고쳐야 할 점으로 느껴질 것입니다.
<h4>search.html</h4>
{% highlight html %}
---
title: Search
description: "Search this site"
layout: default
permalink: /search/
tipue_search_active: true
exclude_from_search: true
---
<!-- <form action="{{ page.url | relative_url }}">
  <div class="tipue_search_left"><img src="{{ "/assets/tipuesearch/search.png" | relative_url }}" class="tipue_search_icon"></div>
  <div class="tipue_search_right"><input type="text" name="q" id="tipue_search_input" pattern=".{3,}" title="At least 3 characters" required></div>
  <div style="clear: both;"></div>
</form> -->
<div id="tipue_search_content"></div>

<script>
$(document).ready(function() {
  $('#tipue_search_input').tipuesearch();
});
</script>
{% endhighlight %}
우선 본문에서 검색입력칸이 하나 더 나오는 이유는 서치페이지에서 폼 태그가 있기 때문입니다. 그렇기에 폼 태그를 주석처리하면 하나 더 나오는 문제가 해결됩니다. 그 다음 헤더에 있는 입력칸의 스타일을 변경할 차례입니다.

<h4>header.html</h4>
{% highlight html %}
<header class="site-header" role="banner">
	<div class="wrapper">
		<a class="site-title" rel="author" href="/">{{ site.title }}</a>
		<div style="display: flex;">
			<form class="searchButton" action="/search">
				<input
					class="searchBar"
					type="text"
					name="q"
					id="tipue_search_input"
					pattern=".{2,}"
					title="최소 2글자 이상"
					required
				/>
			</form>
			<button id="searchClose" class="searchClose">
				<svg
					height="24"
					viewBox="0 0 24 24"
					width="24"
					xmlns="http://www.w3.org/2000/svg"
				>
					<path
						d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"
					></path>
					<path d="M0 0h24v24H0z" fill="none"></path>
				</svg>
			</button>
		</div>
		<nav class="site-nav">
			<input type="checkbox" id="nav-trigger" class="nav-trigger" />
			<label for="nav-trigger">
				<span class="menu-icon">
					<svg viewBox="0 0 18 15" width="18px" height="15px">
						<path
							d="M18,1.484c0,0.82-0.665,1.484-1.484,1.484H1.484C0.665,2.969,0,2.304,0,1.484l0,0C0,0.665,0.665,0,1.484,0 h15.032C17.335,0,18,0.665,18,1.484L18,1.484z M18,7.516C18,8.335,17.335,9,16.516,9H1.484C0.665,9,0,8.335,0,7.516l0,0 c0-0.82,0.665-1.484,1.484-1.484h15.032C17.335,6.031,18,6.696,18,7.516L18,7.516z M18,13.516C18,14.335,17.335,15,16.516,15H1.484 C0.665,15,0,14.335,0,13.516l0,0c0-0.82,0.665-1.483,1.484-1.483h15.032C17.335,12.031,18,12.695,18,13.516L18,13.516z"
						/>
					</svg>
				</span>
			</label>
			<div id="headerTrigger" class="trigger">
				<a class="page-link" href="{{ site.baseurl }}/category/Linux">Linux</a>
				<a class="page-link" href="{{ site.baseurl }}/category/SpringBoot"
					>SpringBoot</a
				>
				<a class="page-link" href="{{ site.baseurl }}/category/Github"
					>Github</a
				>
				<a class="page-link" href="{{ site.baseurl }}/tags">Tags</a>
				<a class="page-link" href="{{ site.baseurl }}/about">About</a>
			</div>
		</nav>
		<!-- search box -->
		<img
			src="/assets/tipuesearch/search.png"
			id="searchIcon"
			class="searchIcon"
		/>
	</div>
</header>
<script>
	$('#searchIcon').click(function() {
		$('#tipue_search_input').show()
		$('#searchIcon').hide()
		$('#searchClose').show()
		$('#headerTrigger').hide()
	})
	$('#searchClose').click(function() {
		$('#tipue_search_input').hide()
		$('#searchIcon').show()
		$('#searchClose').hide()
		$('#headerTrigger').show()
	})
</script>
{% endhighlight %}
타이틀 옆으로 검색 아이콘과 입력칸을 넣고 제이쿼리를 이용하여 클릭 이벤트에 따른 상태 변화를 추가 하였습니다. 스타일을 다루기 위해 아래의 코드를 파일에 넣었습니다.
<h4>main.css</h4>
{% highlight css %}
.searchBar {
  display: none;
  background-color: #f4f4f4;
  width: 100%;
	padding: 1em;
	border: none;
	margin: 0.5em;
}

.searchIcon {
  padding: 15px;
  width: 24px;
  height: 24px;
}

.searchClose {
  display:none;
  border:none;
  background:none;
}

.searchButton {
  float: right;
}
{% endhighlight %}

<br><br>
![makeGithubBlog33](/files/makeGithubBlog/makeGithubBlog33.png)
<br><br>
![makeGithubBlog34](/files/makeGithubBlog/makeGithubBlog34.png)
<br><br>

이 다음 포스트 URL이 보이는 현상을 해결하려 했으나 라이브러리를 수정하는 단계까지 가야하기에 그냥 두기로 하겠습니다. 다음 포스트는 인디케이터를 추가한 페이징 기능을 추가하는 것을 다루어보도록 하겠습니다.



[tipuesearch]: https://github.com/jekylltools/jekyll-tipue-search
[liquidPage]: https://shopify.github.io/liquid/ 
[materialIcon]: https://material.io/resources/icons/?icon=local_offer&style=baseline