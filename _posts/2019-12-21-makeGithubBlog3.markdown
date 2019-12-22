---
layout: post
title: "깃허브 블로그(GitHub Blog) 만들기 (3/7)"
date: 2019-12-21
categories: Github
tags: 
---
<div style="display:none;">
</div>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>포스트 생성</h3>
<br>
저번 포스트에서는 전체적인 구조와 화면추가 그리고 테마의 적용까지 알아보았다. 이번 포스트는 블로그의 핵심 포스트에 대해 다루어 보도록 하겠다. 
<br><br>
포스트를 작성하기 위해서 우선 포스트파일이 어디에 있어서 엔진이 인식하고 컴파일 하는지를 알아야한다. 전에 구조에 대해 다루었을 때 그리고 지금 사용하고 있는 프로젝트에서도 _posts라는 폴더를 발견 할 수 있을 것이다. 이 안에 있어야만 한다.
<br><br>
단지 있기만 해서도 안된다. 파일의 이름은 항상 년-월-일-제목.md에 해당해야하며 이름 형태를 마추어주면 엔진이 이를 인식하여 _site내로 년/월/일/제목.html 형태의 파일구조를 만들어 사용하게 된다. 이렇게 생성할 수 있으며 이제 제대로 포스트를 만들기 위해서 파일 내부를 어떻게 작성해야하는지 알아보자. 
<br><br>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>포스트 작성</h3>
<br>
지킬 엔진이 포스트의 정보를 읽기 위해서 ---의 사이에 기록된 속성 값을 읽는다. 주로 layout, title, date등이며 경우에 따라 category, tags등을 추가하기도 한다. 그 기능들을 추가하는 것은 나중에 다루어 보고 지금은 각 요소에 대한 설명과 작성법에 대해 서술하겠다.
<br><br>
layout속성은 이 포스트가 표시될 파일 및 공간을 나타내며, post라 입력하면 layout내에 post.html라는 파일안에서 content로 출력되게 된다. title,date는 표시되게 될 파일안에서 포스트 정보값으로 활용되며 post.title로 리퀴드 언어에 의해 출력된다. 여기서 잠깐 리퀴드 언어에 대해 설명하겠다.
<br><br>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>리퀴드(Liquid)</h3>
<br>
리퀴드란 루비의 파생어로 지킬이 루비 젬에서 생성되었으며 루비언어를 사용하는데 그 중에서도 리퀴드라는 언어를 사용한다. 이건 마치 리엑트(React)라는 언어가 자바스크립트 언어지만 거기에 파생된 다른 언어인 것과 같다.
<br><br>
이 리퀴드 언어는 중괄호와 퍼센트를 써서 명령어나 변수를 표시할 수 있게한다. 예를 들어 저번 포스트에서도 보았듯이 for문을 사용한다던가 포스트의 제목을 나타내는 방식으로 활용된다. 이것은 포스트 내에서도 적용되기에 활용하는데 조금 까다롭다.  
<br><br>
하지만 활용하기에 따라 매우 쓰기 편해지기에 글을 쓰는게 적용적으로 쓰길 바라는 바다. 자세한 문법은 <b><a href="https://jekyllrb.com/docs/step-by-step/02-liquid/#use-liquid">여기</a></b>에 남겨두겠다.
<br><br>
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<h3>포스트 작성 예</h3>
<br>
백문이 불여일견. 하나 포스트를 만들어보자. _posts 폴더 내에 년-월-일-제목.md에 형식에 맞춘 파일을 만들고 ---에 포스트의 제목, 날짜 등을 넣고 본문을 만들면 아래와 같다.
<br><br>
![makeGithubBlog10](/files/makeGithubBlog/makeGithubBlog10.png)
<br><br>
![makeGithubBlog11](/files/makeGithubBlog/makeGithubBlog11.png)
<br><br>
홈 화면에서 최신순으로 포스트에 올라온 모습이다.
<br><br>
![makeGithubBlog12](/files/makeGithubBlog/makeGithubBlog12.png)
<br><br>
포스트가 작성되었다. 하지만 글만으로 포스트를 작성할 수는 없는 법. 다음으로 문장을 강조하거나 이미지를 추가하는 방법들에 대해 설명하겠다.
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">
<br><br>
<h3>포스트 작성 예</h3>
<br>
포스트 마크다운 문서이며 기본적으로 웹 문서이기 때문에 html태그들이 적용된다. `<b>`<b>굵게하거나</b> `<i>`<i>기울이거나</i> `<u>`<u>밑줄을 추가하는</u>것이 가능하다. 억음부호(```) 두개 사용하여 강조할 수도 있고 본문 내에 `[Jekyll docs][jekyll-docs] [jekyll-docs]: https://jekyllrb.com/docs/home`식으로 링크를 거는 것도 가능하다. 그 밖에 텍스트를 꾸미는 것은 다양하기에 <b>[테디노트][tedinote]</b>님의 링크를 걸어놓겠다.
<br><br>  
필자가 주로 이미지를 올리는 때 사용하는 방법은` ![이미지제목](/폴더경로/파일명)`으로 files라는 폴더를 만들고 그 안에 이미지 파일을 넣어 위에 같은 형태로 사용한다.<br>
`![makeGithubBlog12](/files/makeGithubBlog/makeGithubBlog12.png)`은 
<br><br>
![makeGithubBlog12](/files/makeGithubBlog/makeGithubBlog12.png)
<br><br>
이런식으로 표현된다.
<br><br>
그 다음 코드를 표현하고 싶은 때 `<pre><code>`태그를 사용하기도 하지만 억음부호 세개를 사용하여 
<br><br>
![makeGithubBlog13](/files/makeGithubBlog/makeGithubBlog13.png)
<br><br>
위와 같이 표현할 수도 있다. 리퀴드 언어를 활용하면 {% raw %}
{% highlight java %}{% endhighlight %}
{% endraw %}로 쓸수 있다. 

<h4>2019-12-21-mypost.md</h4>
{% raw %}
~~~html
---
layout: post
title:  "MyPost"
date:   2019-12-21
categories: jekyll update
---
Hello World!
<br>
<b>Hello World!</b>
<br>
<i>Hello World!</i>
<br>
<u>Hello World!</u>
<br>
<b>[테디노트][tedinote]</b>
<br>
![sunny](/files/sunny_stag.jpg)
<br>
{% highlight html %}
---
layout: default
---
<div class="home">
  <h1 class="page-heading">site.title</h1>
  <ul class="post-list">
      <li>
        <span class="post-meta">post-meta</span>
        <h3><a class="post-link">post-link<a></h3>
        <br>
      </li>
  </ul>
</div>
{% endhighlight %}

[tedinote]: https://teddylee777.github.io/jekyll/Jekyll-%EC%82%AC%EC%9A%A9%EC%9D%84-%EC%9C%84%ED%95%9C-markdown-%EB%AC%B8%EB%B2%95
~~~
{% endraw %}
<br><br>
![makeGithubBlog14](/files/makeGithubBlog/makeGithubBlog14.png)
<br><br>
이번 포스트는 여기까지 작성하도록 하겠으며 다음 포스트에서는 카테고리를 적용하는 방법에 대해 다루어 보도록하겠다.
<hr style="display:block !important; margin:25px 0; border:1px solid #c3c3c3">


[tedinote]: https://teddylee777.github.io/jekyll/Jekyll-%EC%82%AC%EC%9A%A9%EC%9D%84-%EC%9C%84%ED%95%9C-markdown-%EB%AC%B8%EB%B2%95
