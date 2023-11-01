---
title: "[GitHub Blog]깃허브 블로그 만들기 - 코드블록(codeblock)"
category: GitHub Blog
tags: github blog codeblock 코드블록
---
liquid문법을 코드블록에 써도 렌더링 되는 현상해결 및 언어별 스타일 적용

깃허브 블로그로 포스팅을 할 때 코드블록을 통해 liquid 문법을 있는 그대로 출력하고 싶은데 jekyll에서 렌더링을 한 채로 출력해서 불편함이 있다. 

그래서 이 문제를 해결하고자 한다.

그리고 ```(백택)을 이용한 코드블록사용과 언어별 highlight 가 적용되지 않아 불편함이있기에 이 또한 해결하고자 한다.

>1. [jekyll - raw-endraw](#1-jekyll---raw-endraw)
>
>2. [markdown - pre/code](#2-markdown---precode)
>
>3. [언어강조 - highlight 언어 - endhighlight](#3-언어강조---highlight-언어---endhighlight)
>
>4. [reference](#reference)

-------
## 1. Jekyll - raw-endraw

먼저 jekyll 언어는 liquid문법인 raw-endraw 문을 통해서 있는 그대로 출력할 수 있다.

- <b class="text-gray">raw-endraw 사용 안했을때</b>

> 코드: <img width="104" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/9673d42d-ea7c-4405-8e0b-bfb9dd02ae4d">
>
> 결과: {{page.title}}
 
=> page.title이 렌더링되어 front-matter에서 설정한 이 게시글의 title이 출력됨을 확인할 수 있다.

 - <b class="text-red">raw-endraw 사용 했을때</b>

> 코드: <img width="270" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/067f8d88-9fe6-4948-a456-19ba6cea540a">
>
> 결과: {% raw %} {{page.title}} {% endraw %}

=> page.title이 렌더링 되지 않고 글자 그대로 출력되었음을 확인할 수 있다.

------

## 2. Markdown - pre/code(```)

``` <pre><code> ```는 마크다운파일을 작성할떄 사용하는 ```(백택)과 동일한 태그이다.

코드블록을 표현할 때 사용한다.

백택을 사용하여 작성하고 페이지 요소를 살펴보면 

```
<pre>
    <code> 
        이렇게 작성되어있음을 살펴볼 수 있다.
    </code>
</pre>
```

------

## 3. 언어강조 - highlight 언어 - endhighlight

나는 블로그를 만들때 테마를 사용하지 않았기 때문에 코드블록을 사용할때 언어 강조(highlight)가 되지 않는다.

>코드
>
><img width="344" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/5d6aa8ef-8c41-46a5-9b4b-dc18f8165380">

>결과
>
><img width="362" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/9c2aa33c-e087-423d-9a23-18bfc541d109">


이를 해결하기 위해 먼저 kramdown rauge 라는 gem 파일을 설치해야한다.


> gem install kramdown rouge

설치 성공 후 _config.yml 파일에서

>highlighther: rouge 

를 추가해준다.

그래도 아직 색이 적용되지 않는데 highlighter 테마가 없기 때문이다.

이 테마는 터미널에서 아래 명령어로 받을 수 있다.

> rougify style 테마명 > 파일경로

나는 "github" 테마를 _sass/highlight.scss 에 다운받았고

ex) rougify style github > _sass/highlight.scss 

<img width="174" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/d1812bce-4fb2-4427-9063-269fa25c2aff">
<img width="219" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/6a96abd8-8c16-4251-b206-0ef2b76a24fa">

assets/css/styles.scss 에 ``` @import "highlight"; ``` 를 추가하여 css를 적용했다. 

적용 결과
```java
public class{
    public static void main(string[] args){
        int a = 10;
    }
}
```

적용 전 결과와 달리 코드블록 주변에 약간 회색의 블록이 생기고 글자별로 색상이 바뀌는 것을 확인할 수 있다.

# 끝

--------
### reference

[깃 블로그에서 코드블록 만들기, ```를 사용하지 않는 코드블록](https://hhj6212.github.io/blog/2020/08/22/Jekyll-highlight-codeblock.html)

[마크다운 코드블록 지원 언어 표](https://computer-science-student.tistory.com/366)

[코드블록 하이라이터 설치 및 테마 적용](https://junia3.github.io/blog/codeblock)

[코드블록 하이라이터 테마 설치 및 테마 예시보여주는 사이트](https://mingzz1.github.io/development/github-blog/2020/06/16/jekyll-rougify_theme.html/)

[깃허브 page 공식 docs](https://docs.github.com/ko/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll#syntax-highlighting)