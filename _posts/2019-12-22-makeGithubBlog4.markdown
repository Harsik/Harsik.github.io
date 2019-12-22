---
layout: post
title: "깃허브 블로그(GitHub Blog) 만들기 (4/7)"
date: 2019-12-22
categories: Github
tags: 
---
<div style="display:none;">
카테고리 만들기
</div>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>카테고리 만들기에 앞서</h3>
<br>
![makeGithubBlog8](/files/makeGithubBlog/makeGithubBlog8.png)
<br><br>
지금 화면이 스타일을 맞추기 전에 임시로 만들어 놓은 것이기 때문에 보기에 불편하고 중구난방적으로 요소들이 배치가 되어있다. 그래서 어느정도 스타일을 맞추기 위해 화면을 조정하기로 하였다.
<br><br>
![makeGithubBlog15](/files/makeGithubBlog/makeGithubBlog15.png)
<br><br>
위 사진은 처음 만들었던 템플릿의 모습이다. 참 깔끔하지 않는가. 지금은 템플릿을 고칠 수 없기 때문에 고칠 수 있도록 이 모습을 참고하여 화면들을 만들어 볼 것이다. 우선 화면을 헤더, 디폴트, 풋터순으로 구분하고 하나씩 만들어 보자. 우선 디폴트이다.
<br><br>
<h4>default.html</h4>
{% raw %}
~~~html
<!DOCTYPE html>
<html>
	{% include head.html %}
	<body>
		{% include header.html %}
		<main class="page-content" aria-label="Content">
			<div class="wrapper">
				<div class="page-content">
					<div class="wrapper">
						{{ content }}
					</div>
				</div>
			</div>
		</main>
		{% include footer.html %}
	</body>
</html>
~~~
{% endraw %}
<br><br>
head 태그에 해당하는 부분을 분리하여 head.html 파일로 관리할 것이다. 
<br><br>
<h4>head.html</h4>
{% raw %}
~~~html
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1"><!-- Begin Jekyll SEO tag v2.6.1 -->
<title>{% if page.title %}{{ page.title }}{% else %}{{ site.title }}{% endif %}</title>
<meta name="generator" content="Jekyll v4.0.0" />
<meta property="og:title" content="{% if page.title %}{{ page.title }}{% else %}{{ site.title }}{% endif %}" />
<meta property="og:locale" content="en_KR" />
<meta name="description" content="{{ site.description }}">
<meta property="og:description" content="{{ site.description }}">
<link rel="canonical" href="{{ page.url | replace:'index.html','' | prepend: site.baseurl | prepend: site.url }}">
<meta property="og:url" content="{% if page.baseurl %}{{ page.baseurl }}{% else %}{{ site.baseurl }}{% endif %}" />
<meta property="og:site_name" content="{% if page.site_name %}{{ page.site_name }}{% else %}{{ site.site_name }}{% endif %}" />
<script type="application/ld+json">
{"url":"http://localhost:4001/","headline":"Your awesome title","description":"Write an awesome description for your new site here. You can edit this line in _config.yml. It will appear in your document head meta (for Google search results) and in your feed.xml site description.","name":"Your awesome title","@type":"WebSite","@context":"https://schema.org"}</script>
<!-- End Jekyll SEO tag -->
<link rel="stylesheet" href="{{ "/assets/main.css" | prepend: site.baseurl }}" />
<link type="application/atom+xml" rel="alternate" href="{{ "/feed.xml" | prepend: site.baseurl }}" title="{% if page.title %}{{ page.title }}{% else %}{{ site.title }}{% endif %}" />
</head>
~~~
{% endraw %}
<br><br>
body 부분에서 header 또한 따로 나누겠다.
<br><br>
<h4>header.html</h4>
{% raw %}
~~~html
<header class="site-header" role="banner">
	<div class="wrapper">
		<a class="site-title" rel="author" href="/">{{ site.title }}</a>
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
			<div class="trigger">
				<a class="page-link" href="{{ site.baseurl }}/">Home</a>
				<a class="page-link" href="{{ site.baseurl }}/about">About</a>
			</div>
		</nav>
	</div>
</header>
~~~
{% endraw %}
<br><br>
다음은 본문 즉 main 태그 부분이다. 이 부분은 실질적으로 index.html가 담당하는 부분과 같기 때문에 따로 파일을 두지 않고 index.html 파일을 사용하겠다.
<br><br>
<h4>index.html</h4>
{% raw %}
~~~html
---
layout: default
---
<div class="home">
	<h2 class="post-list-heading">{{ site.title }}</h2>
	<ul class="post-list">
		{% for post in site.posts %}
		<li>
			<span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
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
다음 footer 부분이다.
<br><br>
<h4>footer.html</h4>
{% raw %}
~~~html
<footer class="site-footer h-card">
	<data class="u-url" href="/"></data>
	<div class="wrapper">
		<h2 class="footer-heading">{{ site.title }}</h2>
		<div class="footer-col-wrapper">
			<div class="footer-col footer-col-1">
				<ul class="contact-list">
					<li class="p-name">{{ site.title }}</li>
					<li>
						<a class="u-email" href="{{ site.email_address }}"
							>{{ site.email_address }}</a
						>
					</li>
				</ul>
			</div>
			<div class="footer-col footer-col-2">
				<ul class="social-media-list">
					<li>
						<a href="{{ site.github_address }}"
							><svg class="svg-icon">
								<use xlink:href="/assets/minima-social-icons.svg#github"></use>
							</svg>
							<span class="username">{{ site.github_username }}</span></a
						>
					</li>
					<!-- <li>
						<a href="https://www.twitter.com/jekyllrb"
							><svg class="svg-icon">
								<use xlink:href="/assets/minima-social-icons.svg#twitter"></use>
							</svg>
							<span class="username">jekyllrb</span></a
						>
					</li> -->
				</ul>
			</div>
			<div class="footer-col footer-col-3">
				<p>
					{{ site.description}}
				</p>
			</div>
		</div>
	</div>
</footer>
~~~
{% endraw %}
<br><br>
config 변수들을 적용하기 위해서 _config.yml을 조금 수정할 필요가 있다.
<br><br>
<h4>footer.html</h4>
{% highlight html %}
# Welcome to Jekyll!
#
# This config file is meant for settings that affect your whole blog, values
# which you are expected to set up once and rarely edit after that. If you find
# yourself editing this file very often, consider using Jekyll's data files
# feature for the data you need to update frequently.
#
# For technical reasons, this file is *NOT* reloaded automatically when you use
# 'bundle exec jekyll serve'. If you change this file, please restart the server process.
#
# If you need help with YAML syntax, here are some quick references for you: 
# https://learn-the-web.algonquindesign.ca/topics/markdown-yaml-cheat-sheet/#yaml
# https://learnxinyminutes.com/docs/yaml/
#
# Site settings
# These are used to personalize your new site. If you look in the HTML files,
# you will see them accessed via {{ site.title }}, {{ site.email }}, and so on.
# You can create any custom variable you would like, and they will be accessible
# in the templates via {{ site.myvariable }}.

title: Harsik's blog
email: aznm12@archivsoft.com
description: >- # this means to ignore newlines until "baseurl:"
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: "" # the subpath of your site, e.g. /blog
url: "" # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: jekyllrb
github_username:  Harsik
github_address: https://github.com/Harsik
# Build settings
theme: minima
plugins:
  - jekyll-feed

# Exclude from processing.
# The following items will not be processed, by default.
# Any item listed under the `exclude:` key here will be automatically added to
# the internal "default list".
#
# Excluded items can be processed by explicitly listing the directories or
# their entries' file path in the `include:` list.
#
# exclude:
#   - .sass-cache/
#   - .jekyll-cache/
#   - gemfiles/
#   - Gemfile
#   - Gemfile.lock
#   - node_modules/
#   - vendor/bundle/
#   - vendor/cache/
#   - vendor/gems/
#   - vendor/ruby/
{% endhighlight %}
<br><br>
![makeGithubBlog16](/files/makeGithubBlog/makeGithubBlog16.png)
<br><br>
이렇게 화면들을 바꿀 수 있는 준비가 되었다. 이 다음은 카테고리 기능을 추가하는 작업을 시작하겠다.
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">