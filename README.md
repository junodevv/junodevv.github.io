# junodevv.github.io
----------

# How To Write Jekyll

## 1. Jekyll 의 구조
|-- _data (페이지 구성요소, Ex member.yml -> site.data.members 로 사용)

|-- _includes (여러 페이지에서 사용되는 html)

|-- _layouts (뼈대를 구성하는 html)

|-- _sass (scss 집합?)

|-- _config.yml (페이지 속성파일 - 환경설정 정보)

|-- index.html ('홈' 페이지)

|-- Gemfile (패키지 구성파일)

|-- etc.

[공식 참고 사이트](https://jekyllrb-ko.github.io/docs/structure/)

## 2. Liquid(오브젝트, 태그, 필터)
* 오브젝트 => {{ 변수명 }}
* 태그 = {% 논리연산 %} => if문 등 작성하는 곳
* 필터 = | => {{hi | capitalize}} = HI 가 출력됨

## 3. 머리말 
```jekyll
---
title: junodevv Bolg Home
---
...

{{ page.title }} = junodevv Bolg Home

```

## 4. 레이아웃(기본 레이아웃, 이걸 머리말에 넣으면 이 형식으로 출력됨 ) 
1. _layouts/default.html  생성 후 

2. index.html 에 레이아웃을 적용

```jekyll
---
layout: default
---
..
..
내용 
```
3. about.md 에도 적용 -> http://localhost:4000/about.html

```jekyll
---
layout: default
title: about
---
내용솰라솰라 
```


## 5. 네비게이션
Include 태그를 이용하여 _include폴더에 저장된 정적자원들을 사용가능하다

사용법

1. 네비게이션을 위한 파일 _includes/navigation.html 에 저장

```jekyll
<nav>
	<a href="/">Home</a>
	<a href="/about.html">About</a>
</nav>
```

2. 현재 페이지 표시( page.url 변수활용)
 
```jekyll
<a href="/" 
		{%if page.url == "/"%}style="color: blue;" {%endif%}>
			Home
</a>
<a href="/about.html"
		{%if page.url == "/about.html" %} style="color: blue;" {%endif%}>
			About
</a>
```
이러면 네비게이션에서 현재페이지의 색이 변한다.

## 6. 데이터(_data) 
jekyll은 _data 디렉토리의 YAML, JSON, CSV 파일에서 데이터들을 읽어올수 있다. 

이런 데이터 파일들을 활용해서 소스코드와 컨텐츠를 분리하여 유지관리를 쉽게 한다.

사용법(yml) 

1. navigation.yml 생성

```yml
- name: Home
  link: /
- name: About
  link: /about.html
```

이렇게 만들고

2. navigation.html에서의 활용

```jekyll
<nav>
	{% for item in site.data.navigation %}
		<a href="{{ item.link }}" 
			{% if page.url == item.link %}style="color: blue;" {% endif %}>
				{{ item.name }}
		</a>
	{% endfor %}
</nav>
```

이렇게 만들면 위에서 작성한 navigation과 같은 결과가 나옴

## 7. 에셋
에셋 구조

|-- assets

|	|-- css

|	|-- images

|	|-- js

Sass

-> jekyll에서 사용할 수 있는 CSS확장 기능 

sass 사용법

1. assets/css/style.scss 생성

```scss
---
---
// 비어있는 머리말 = 이 파일을 처리하라고 jekyll에게 알려주는 것
// _sass/main.scss 파일을 참조 하라는 의미
@import "main";
```
이렇게 만든다. 여기서 위의 비어 있는 머리말은 이 파일을 처리하라고 jekyll에게 알려주는것이고

import 는 _sass/main.scss파일을 참조하라는 의미이다.

이 과정을 거치면서 jekyll은 scss파일을 css파일로 생성한다. 

2. _sass/main.scss 생성

```scss
.current{
    color: green;
}
```

이제 main을 import한 style를 통해서 current를 사용할 수 있다.

3. 위에서 만든 scss들 적용

_layouts/default.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>{{ page.title }}</title>
<link rel="stylesheet" href="/assets/css/styles.css"> ******
<!-- jekyll이 scss를 css로 생성하기 때문에 css로 참조하면 된다. -->
</head>
<body>
	{% include navigation.html %}
	{{ content }}
</body>
</html>
```

여기서 jekyll이 scss파일을 css로 생성하기 때문에 참조 주소는 .css가 된다.

_includes/navigation.html

```html
<nav>
	{% for item in site.data.navigation %}
		<a href="{{ item.link }}" 
			{% if page.url == item.link %}class="current"{% endif %}>
				{{ item.name }}
		</a>
	{% endfor %}
</nav>
```

현재 페이지의 색상 설정을 assets/css/styles.scss에서 import한 _sass/main.scss파일을 통해서 설정했다.

## 8. 블로깅
데이터 베이스 없이 어떻게 블로그를 가지는가

1. 게시물(_posts)

게시물은 _posts라는 폴더 아래에 " _post/2023-07-27-bananas.md"(날짜-제목.확장자)이런형식으로 작성된다.

post 작성법 ex)

```md
---
layout: post
author: jill
---

first post 입니당
```

2. post 레이아웃 설정

_layouts/post.html

```html
---
layout: default
---
<h1>{{ page.title }}</h1>
<p>{{ page.date | date_to_string }} - {{ page.author }}</p>

{{ content }}
<!--    
	page.title 해당 페이지 의 머리말에서 설정한 title
    page.date 해당 페이지 파일이름에 작성된 날짜
    page.author 해당 페이지 머리말에서 설정한 author
-->
```

이제 각 post 들에 머리말에서 layout: post를 설정하면 이 레이아웃대로 나온다.

3. 게시물을 나열할 페이지 생성 (blog.html)
```html
---
layout: default
title: Blog
---
<h1> Latest Posts</h1>

<ul>
    {% for post in site.posts %}
        <li>
            <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
            <p>{{ post.excerpt }}</p>
        </li>
    {% endfor %}
</ul>
<!-- 
    post.url의 경로는 jekyll에 의해 자동으로 설정됨
    post.title은 게시물 파일이름에서 가져오고, 원하면 title머리말에서 재정의 할 수 있음
    post.excerpt는 콘텐츠의 첫번째 단락
-->
```

4. navigation.yml blog.html에 추가 

```yml
...
- name: Blog
  link: /blog.html
```

여기까지 하면 navigation을 통해 blog.html로 연결되고 bolg.html에는 post로 연결되는 list들이 있다.

## 9. 컬렉션
컬렉션은 작성자를 구체화하고 광고문구? 게시물이 있는 고유한 페이지를 갖게 해주는 것??

게시물과 유사하다.

1. 구성

컬렉션을 설정하기 위해선 jekyll에게 알려야하는데 그 것이 이루어지는  파일이 바로 **_config.yml** 이다.

루트 디렉토리에 생성한다. 

_config.yml

```yml
collections:
authors: 
```

_config.yml을 재구성 했을때는 서버를 다시 시작해 줘야 한다. ( ctrl+C -> jekyll serve)

2. 작성자 추가

_config.yml 내부의 collections 는 루트 폴더에 정의하면된다. 예를 들어서 작성자 여러명을 정의해 두고 싶으면
_authors 폴더를 루트 디렉토리에 생성하여 그 폴더 아래에 작성자들을 정의 해둔다

ex) _author/juno.md

```md
---
name: juno
position: backend-engineer
---
내용 솰라솰라
```
... ted 등등 여러명 추가

그리고 작성자 페이지를 만들어서 아까 블로그 만들듯이 만들면 되는데 나는 이게 필요없으니 내 블로그에서는 생략할것이지만

일단해보자

ex) author.html

```html
---
layout: default
title: Staff
---
<h1>Staff</h1>
<ul>
	{% for author in site.authors %}	
</ul>
```




