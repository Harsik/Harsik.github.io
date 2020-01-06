---
layout: post
title: "깃허브 블로그(GitHub Blog) 만들기 6 - 페이지네이터 구현"
date: 2020-01-06
categories: Github
tags: Jekyll Liquid Javascript
---
<div style="display:none;">
페이징 만들기
</div>
<hr class="divider">
<h3>페이징 만들기 계획</h3>
<br>
&nbsp;언젠가 만들어야지 생각만 하던 기능을 차례차례 만들기 시작하기 감회가 새롭다. 처음 이 블로그를 만들었을 때 포스트가 몇 없어서 페이징 기능을 만들 필요도 카테고리를 만들 필요도 없었는데 이제는 둘다 만들고 있다. 계획이라면 포스트 리스트 밑에 인디케이터를 추가하여 페이징 기능을 강화시키는 것이다. 그럼 본격적으로 시작해보자.
<hr class="divider">
<h3>페이지네이터 추가</h3>
<br>
&nbsp;많은 블로그 들이 포스트를 여러페이지로 나누어 사용하기에 지킬 엔진 또한 그 기능을 지원하는 플러그인을 제공한다. 우리는 <b>[jekyll-paginate][jekyll-paginate]</b>라는 플러그 인을 사용할 것이다. 플러그인을 사용하기 위해서 홈페이지에서는 jekyll 3에 대한 내용이 적혀 있어 jekyll 4와 다른 점이 있는지 확인햇지만 동일한 방법을 사용하면 된다. 아래의 파일에 각각 필요로 하는 내용을 추가하면된다.
<h4>Gemfile</h4>
{% highlight text %}
group :jekyll_plugins do
  gem "jekyll-paginate"
end
{% endhighlight %}
<h4>_config.yml</h4>
{% highlight text %}
plugins:
  - jekyll-paginate
paginate: 5
paginate_path: "/posts/page/:num"
{% endhighlight %}
<br><br>
&nbsp;그리고 포스트 리스트를 표시할 파일로 index.md을 사용했었는데 페이지네이터 플러그인을 사용하기 위해서는 다시 확장자 html로 변경해야 한다. 아래의 코드를 참고하여 index.html의 코드를 변경하길 바란다.
<br><br>
<h4>index.html</h4>
{% raw %}
~~~html
---
layout: default
---
<div class="home">
	<h2 class="post-list-heading">최근 게시물</h2>
	<ul class="post-list">
		{% for post in paginator.posts %}
		<li>
			<span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}
			{% if post.tags %} 
			<i class="material-icons svg-icon">local_offer</i>
			{% for tag in post.tags %}
			<a class="label" href="{{ '/tags' | prepend: site.baseurl }}">#{{ tag }}</a> &nbsp; 
			{% endfor %} 
			{% endif %}
			</span>
			<h3>
				<a class="post-link" href="{{ post.url | prepend: site.baseurl }}"
					>{{ post.title }}</a
				>
			</h3>
		</li>
		{% endfor %}
	</ul>
</div>
{% include pagination.html %}				
~~~
{% endraw %}
<br><br>
&nbsp;페이지를 넘기거나 찾는 부분은 따로 파일로 빼내었다. 지금은 단순하지만 이 뒤로 좀 더 코드가 길어지기에 지저분해 보일 수 있기 때문이다. 아래의 코드를 참고하여 페이지네이션 파일을 작성할 수 있다.
<br><br>
<h4>pagination.html</h4>
{% raw %}
~~~html
<!-- Pagination links -->
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path }}" class="previous">Previous</a>
  {% else %}
    <span class="previous">Previous</span>
  {% endif %}
  <span class="page_number ">Page: {{ paginator.page }} of {{ paginator.total_pages }}</span>
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path }}" class="next">Next</a>
  {% else %}
    <span class="next ">Next</span>
  {% endif %}
</div>		
~~~
{% endraw %}
<br><br>
결과 화면은 아래와 같다.
<br><br>
![makeGithubBlog35](/files/makeGithubBlog/makeGithubBlog35.png)
<br><br>
페이지네이션 파일에 나와있는 것처럼 인디케이터가 형성되었다. 넥스트를 누르면 다음 페이지가 나온다.
<br><br>
![makeGithubBlog36](/files/makeGithubBlog/makeGithubBlog36.png)
<br><br>
다음 페이지로 넘어간뒤 프리비어스가 활성화 된 모습이다. 플러그인으로 어렵게만 느껴졌던 페이징 기능이 간단하게 구현됬다. 인덱스 파일뿐만 아니라 카테고리도 포스트 리스트가 있기 때문에 카테고리 또한 인덱스 파일처럼 수정해야 한다. 인덱스 파일처럼 수정할려고 하는데 카테고리에서 적용하기에 상상이상으로 복잡하다. 좀 길어질 테니 다음 소주제로 넘어가겠다.
<hr class="divider">
<h3>페이지네이터 버전업</h3>
<br>
기존에 사용하던 페이지네이터는 버전이 낮아 심플한 기능만을 지원하는 플러그인이다. 그렇기에 카테고리 리스트에 페이징을 추가하기에 적합하지 않다. 그렇다면 버전 업된 플러그인을 설치해보자.
<h4>Gemfile</h4>
{% highlight text %}
group :jekyll_plugins do
  gem "jekyll-paginate-v2"
end
{% endhighlight %}
<h4>_config.yml</h4>
{% highlight text %}
gems:
    - jekyll-paginate-v2
pagination:
    enabled: true
    per_page: 5
    limit: 0
    sort_field: "date"
    sort_reverse: true
{% endhighlight %}
<br><br>
그리고 카테고리 파일 내용을 변경해야한다. 새로운 플러그인에 맞는 페이지 변수들을 설정해줘야 한다.
<br><br>
<h4>/category/SpringBoot.md</h4>
{% raw %}
~~~html
---
layout: category
title: SpringBoot
pagination:
    enabled: true
    category: SpringBoot
    per_page: 5
---	
~~~
{% endraw %}
<br><br>
<h4>/_layouts/category.html</h4>
{% raw %}
~~~html
---
layout: default
---
<div class="home">
	<h2 class="post-list-heading">{{ page.pagination.category }}</h2>
	<ul class="post-list">
{% for post in paginator.posts %}
    <li>
			<span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}
			{% if post.tags %} 
			<i class="material-icons svg-icon">local_offer</i>
			{% for tag in post.tags %}
			<a class="label" href="{{ '/tags' | prepend: site.baseurl }}">#{{ tag }}</a> &nbsp;  
			{% endfor %} 
			{% endif %}</span>
			<h3>
				<a class="post-link" href="{{ post.url | prepend: site.baseurl }}"
					>{{ post.title }}</a
				>
			</h3>
		</li>
{% endfor %}
	</ul>
</div>
{% include pagination.html %}
~~~
{% endraw %}
<br><br>
<h4>pagination.html</h4>
{% raw %}
~~~html
<!-- Pagination links -->
{% if page.pagination.category %}
{% assign category = page.pagination.category | prepend: '/category/' | default: site.baseurl %}
{% else %}
{% assign category = site.baseurl %}
  {% endif %}
{% if paginator.total_pages > 1 %}
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">&laquo; Prev</a>
  {% else %}
    <span>&laquo; Prev</span>
  {% endif %}
  {% for page in (1..paginator.total_pages) %}
    {% if page == paginator.page %}
      <em>{{ page }}</em>
    {% elsif page == 1 %}
      <a href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">{{ page }}</a>
    {% else %}
      <a href="{{ site.paginate_path | prepend: category | replace: '//', '/' | replace: ':num', page }}">{{ page }}</a>
    {% endif %}
  {% endfor %}
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Next &raquo;</a>
  {% else %}
    <span>Next &raquo;</span>
  {% endif %}
</div>
{% endif %}
~~~
{% endraw %}
<br><br>
카테고리 게시물 리스트에서 출력되는 페이지네이터의 변수들이 다르기 때문에 조금 수정할 필요가 있다. 결과는 아래와 같다.
<br><br>
![makeGithubBlog37](/files/makeGithubBlog/makeGithubBlog37.png)
<br><br>
인디케이터에 할당되는 링크가 메인화면과 카테고리 서로 다른 것을 볼 수 있다.
<br><br>
![makeGithubBlog38](/files/makeGithubBlog/makeGithubBlog38.png)
<br><br>


[jekyll-paginate]: https://jekyllrb-ko.github.io/docs/pagination/