# How To Write Jekyll

## 0. jekyll에 대해서

1. 빌드(Build)와 배포(Deploy)
Build 란 어플리케이션이 동작할 수 있도록 컴파일 또는 변환하는 작업이다.

예를 들어 java가 웹어플리케이션을 만들떄 War 파일로 만들어 지는 과정이 Build 이다.

jekyll에서는 HTML(<-MD), CSS(<-SCSS), Javascript 파일이 _site폴더에 생성되는 과정을 Build 라고 한다.



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

3. 작성자 페이지

그리고 작성자 페이지를 만들어서 아까 블로그 만들듯이 만들면 되는데 나는 이게 필요없으니 내 블로그에서는 생략할것이지만

일단해보자

author.html을 생성하면 **site.authors**로 사용할 수 있대

ex) staff.html

```html
---
layout: default
title: Staff
---
<h1>Staff</h1>

<ul>
    {% for author in site.authors %}
    <li>
        <h2>{{ author.name }}</h2>
        <h3>{{ author.position}}</h3>
        <p>{{author.content | markdownify }}</p>
    </li>
    {% endfor %}
</ul>

<!-- 
    markdownify = 콘텐츠가 마크다운이라서 필터링을 한다? {{content}}레이아웃에서 사용하여 출력할때 자동으로 발생
    _site안에 생성된 authors를 참조하는듯, 
    _site에 기존에 적놓은 md나 scss를  html이나 css 으로 변환해서 놓고 그것을 가지고 사이트를 구성하는 거같음
 -->
```

4. 네비게이션에 작성자 html 추가해주기

```yml
...
- name: Staff
  link: /staff.html
```

5. 페이지 출력

기본적으로 컬렉션은 문서페이지를 출력하지 않는다. 

출력하고 싶은 경우각 작성자가 자신의 페이지를 갖도록 하므로 컬렉션의 구성을 조정한다.??

_config.yml -> Output: true 추가

```yml
collections:
  authors: 
    output: true
```

이렇게 하면 출력페이지여 연결을 할 수 있다. author.url을 사용할 수 있게됨

```html
...
<h2><a href="{{ author.url }}">{{ author.name }}</a></h2>
...
 <!-- 
    author.url은 _config.yml에서 output: true 옵션을 추가 함으로써 사용가능해진 url이다.
  -->
```

_layouts/author.html ( author 레이아웃 생성 )

```html
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}
```

6. 머리말 기본값 설정하기 

머리말 기본값을 설정하면 각각의 html의 머리말에 설정하지 않아도된다.

_config.yml

```yml
collections:
  authors: 
    output: true

# 머리말 기본값 설정
defaults:
  - scope:
      path: ""
      type: "authors"
    values:
      layout: "author"
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "post"
  - scope:
      path: ""
    values:
      layout: "default"
```

이렇게 하면 각 html에 머리말에 지정하지 않아도 default로 지정할 수 있다.
범위와 타입을 설정? -> 차차 알아가도록하자

7. 작성자의 게시글 나열( 해당 작성자가 작성한글 모아보기 생성)

_layout/author.html

```html
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}

<h2>Posts</h2>
<ul>
    {% assign filtered_posts = site.posts | where: 'author', page.name %}
    {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
<!-- 
	assign = 변수 선언함수같은거 ex) 선언: {%assign 변수명 = 넣을_값 %}
								사용: {{ 변수명 }}
	그니까 filtered_posts에 site.posts를 넣는데 조건이 'author'가 page.name 인 site.posts 인것
 -->
```

8. 저자 페이지 링크

작성자 페이지에 연결하기 

_layouts/post.html -> 변경

```html
---
layout: default
---
<h1>{{ page.title }}</h1>
<p>
    {{ page.date | date_to_string }}
    {% assign author = site.authors | where: 'name', page.author | first %}
    {% if author %}
     - <a href="{{ author.url }}">{{ author.name }}</a>
    {% endif %}
</p>

{{ content }}
```

## 10. 배포

1. Gemfile

Gemfile이 있어서 jekyll이나 gem의 버전이 다른 환경에서도 일관되게 유지될 수 있다.

Gemfile에 직접 " gem 'jekyll' " 같이 라이브러리? 이름을 작성해놓고 

```
bundle install 
```
터미널에서 위 명령어를 실행하면 gem을 설치하고 Gemfile.lock 을 생성하여 현재 gem버전을 잠근다.

gemfile이 변경된 경우
```
bundle update
```
를 실행하여  업데이트 할 수 있다.

2. 플러그인

- jekyll-sitemap 

-> 검색엔진이 내 웹사이트를 찾을 수 있도록 내 웹사이트의 크롤링 및 색인을 돕는 페이지 목록 같은 것.

-> 웹 크롤링 로봇이 이용할 수 있는 웹사이트의 접근 가능한 페이지의 목록

- jekyll-feed 

-> 게시물에 대한 RSS 피드를 만든다

-> RSS? Rich Site Summary? Real Simple Syndication?

-> RSS = 사이트피드, 새 글들을 뽑아서 하나의 파일로 만들어 놓은 것 -> 구글, 네이버 등의 검색엔진에 이 RSS를 전달 해주면 검색목록에 내 새글들을 제공해준다.

[RSS 참고사이트](https://4343282.tistory.com/164)

- jekyll-seo-tag

-> 검색엔진 최적화를 위해 사용되는 메타 태그들

-> SEO = Search-Engine Optimization : 메타 태그를 이용해서 웹페이지 관련 설명을 추가하거나 URL에 검색어가 될 수 있는 단어를 노출해서 검색 엔진의 검색결과의 상위에 노출되는 확률을 높이는 것

[참고사이트: 검색엔진에 블로그 등록하기](https://gmlwjd9405.github.io/2017/10/21/include-blog-in-a-NaverSearchEngine.html)

위의 3가지 플러그인 추가하는 법

Gemfile 에 설치할 플러그인들 추가

```ruby
source "https://rubygems.org"

gem "jekyll"

group :jekyll_plugins do
    gem 'jekyll-sitemap'
    gem 'jekyll-feed'
    gem 'jekyll-seo-tag'
end
```

_config.yml에 플러그인 항목 추가

```yml
...
# 플러그인 항목
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-seo-tag
```

이후 bundle update 실행

이제 sitemap은 따로 설정할 필요없이 빌드할때 알아서 사이트맵을 생성하고

feed와 seo-tag는 태그를 추가해 줘야한다.

_layouts/default.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>{{ page.title }}</title>
<link rel="stylesheet" href="/assets/css/styles.css"> 
{% feed_meta %} ****
{% seo %} ****
</head>
<body>
	{% include navigation.html %}
	{{ content }}
</body>
</html>
```

이렇게 작성하고 http://localhost:4000 에서 페이지 소스를 확인해보면
```html
<link type="application/atom+xml" rel="alternate" href="http://localhost:4000/feed.xml" />
<!-- Begin Jekyll SEO tag v2.8.0 -->
<title>junodevv Blog</title>
<meta name="generator" content="Jekyll v4.3.2" />
<meta property="og:title" content="junodevv Blog" />
<meta property="og:locale" content="en_US" />
<link rel="canonical" href="http://localhost:4000/" />
<meta property="og:url" content="http://localhost:4000/" />
<meta property="og:type" content="website" />
<meta name="twitter:card" content="summary" />
<meta property="twitter:title" content="junodevv Blog" />
<script type="application/ld+json">
{"@context":"https://schema.org","@type":"WebSite","headline":"junodevv Blog","url":"http://localhost:4000/"}</script>
<!-- End Jekyll SEO tag -->
```
이게 추가된걸 확인할 수 있었다.

3. Environments

아무에게나 보여주고 싶지는 않지만 개발과정에서는 보고싶은 출력결과가 있다면 그걸 Environments를 통해서 설정할수 있다고 하는거 같은데 

예를 들면 분석 스크립트(analytics script)

ex)
```
JEKYLL_ENV=production bundle exec jekyll build
```
이걸 사용하면 jekyll.environment라는 liquid 를 사용할 수 있다.

그리고 JEKYLL_ENV에서 접속했을때만 분석 스크립트를 출력하려면

```html
{% if jekyll.environment == "production" %}
	<script src="my-analytics-script.js"></script>
{% endif %}
```
환경이 production일때만 분석 스크립트를 보여주는 코드인가봐 default에 써주면 되나?

4. 전개

마지막 단계가 _site를 production 서버로 가져오는 거라는데 이게 몬솔

더 나은 방법은 CI 또는 타사의 프로그램을 써서 자동화 하라는데