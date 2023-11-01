---
title: "[GitHub Blog]깃허브 블로그 만들기 - tags"
category: GitHub Blog
tags: github blog tags 태그
---
깃허브 블로그에 태그(tags) 추가하기

### 구현하고 싶은 것
> 1. [게시글을 들어 갔을 때 tag 내용들이 보이게 하기](#1-게시글에-들어갔을때-tag-내용-보이게-하기)
> 
> 2. [네비게이션의 Post항목에 들어 갔을때 게시물의 제목 옆에 tag가 보이게 하기](#2-네비게이션의-post항목에-들어-갔을때-게시물의-제목-옆에-tag가-보이게-하기)
>
> 3. [tags 를 모아놓은 페이지를 생성하기(tags.html)](#3-tags-를-모아놓은-페이지-생성하기-tagshtml)
>
> 4. [main 화면에서 Category 아래에 tags 목록을 생성하기](#4-main-화면에서-category-아래에-tags-목록을-생성하기)

3과 4는 순서가 무관하다. 

-------

## 1. 게시글에 들어갔을때 tag 내용 보이게 하기

기존 내 게시글은 이렇다.
>
><img width="702" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/539f8cec-7cfe-4db7-98b7-e18d29a22aa1">

나는 날짜 밑에 tags 를 추가 하고자 한다.

- 먼저 각 게시글의 머릿말(front-matter)에 tags 라는 속성?을 추가 해준다.

> <img width="434" alt="Pasted Graphic 2" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/1ea41af8-209f-494e-9342-f4a0f8ecabb1">
>
> 빨간줄과 같은 형식으로 tag들을 지정 해주면 된다.
> 공백을 기준으로 태그들을 구분하고 있다.

- 게시글을 보여주는 레이아웃인 _layout/post.html 을 수정해주어야 한다.

> <img width="557" alt="Pasted Graphic 1" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/cc349bf2-62dd-4e2d-abf9-34752a375d43">
>
> 위 사진에서 빨간 부분의 코드를 추가 해줬고 노란밑줄로 그어진 부분의 코드로 태그 이미지를 추가해 줬다.
>
> 만약 page에 tags가 지정 되어있다면 for문을 통해 그 태그들을 출력하는 코드이다.

- 결과

><img width="533" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c7b62dcc-2e40-43e5-9ae5-7621a6c211ef">
> 
> 게시글 속에 tag가 잘 생성되었다.

---------
## 2. 네비게이션의 Post항목에 들어 갔을때 게시물의 제목 옆에 tag가 보이게 하기

- 이번엔 junodevv.github.io/post.html(블로그 root 파일 속의 post.html)에서 코드를 수정해준다.

기존 코드와 화면

> <img width="503" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/dc2b60e6-ebf2-458a-b62f-e0e8cb4717c8">
>
> <img width="707" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/3d54ce21-832a-48a1-b523-e600bcc7cce3">

변경된 코드와 화면

><img width="595" alt="Pasted Graphic 3" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/619e1961-91d3-4d63-9bd1-84c8c413f083">
>
><img width="651" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/92282842-5640-43a8-a487-65934518d31c">

--------

## 3. tags 를 모아놓은 페이지 생성하기 (tags.html)

tag들과 해당tag를 포함하고 있는 게시글을 나열하는 tags.html을 만들고자 한다.

먼저 모든 post의 tag들을 추출하고 그걸 전역변수처럼 만들기 위해서 _includes/tag.html을 만들어 tag라는 변수로 사용할 수 있도록 만들었다.

참고로 나중에 [4번](#4-main-화면에서-category-아래에-tags-목록을-생성하는-것)에서 aside 부분에서도 이 변수를 사용하기 위해 전역 변수처럼 만들었다.

- _includes/tag.html

><img width="889" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/783d7cbb-6d38-4cd6-9b8f-199d9a0f194a">
>
>위의 코드는 [reference](#reference)의 [깃허브 블로그 만들기5 - 태그 기능 구현](https://harsik.github.io/github/2020/01/01/makeGithubBlog5.html) 를 참고했다.
> 
> 참고 liquid문법 [문법 참고사이트1](https://shopify.github.io/liquid/), [문법 참고사이트2](https://heekangpark.github.io/jekyll/06-liquid)
>
>- join : 배열을 문자열로 바꾸기
>
>- contains : 좌항의 String Array에 우항의 String이 원소로 있는지. 있으면 true, 없으면 false.
>
>- split: 문자열을 배열로 만들기
>
>- sort: 배열 정렬하기(대문자 > 소문자, a > z)
>
>- unless: if 의 반대, ~하지 않으면
>
>- slugify: 문자열을 소문자 URL로 변환 해주는 필터

위 파일을 생성함으로써 다른 파일에서

&#123;% include tag.html %&#125; 를 사용하여 tag라는 변수를 사용할 수 있게 된다.

- 이번엔 (root)junodevv.github.io/tags.html 파일을 만들어 위의 tag 변수를 사용해 tags 페이지를 만들었다.

- (root)junodevv.github.io/tags.html 코드와 구현된 화면

><img width="622" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/a42ed350-4d35-43db-b7d3-9ba09b311915">
>
> 해당 코드도 [reference](#reference)의 [깃허브 블로그 만들기5 - 태그 기능 구현](https://harsik.github.io/github/2020/01/01/makeGithubBlog5.html) 를 참고했다.
>
><img width="493" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/78979c8d-97e1-43be-aac8-7041ad4186ae">

--------

## 4. main 화면에서 Category 아래에 tags 목록을 생성하기

> <img width="271" alt="Pasted Graphic 4" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/f187a148-18dd-443d-91a2-03c9d9a77162">
>
여기서 빨간 부분에 모든 tags들의 리스트를 추가하고자 한다.

3번 과정에서 tag라는 변수를 전역으로 사용할 수 있도록 만들었으니 _includes/main.html에 적용하여 나의 default.html의 aside 부분에 tags의 list를 추가할 수 있다.

- _includes/main.html 코드와 구현된 화면

> <img width="769" alt="Pasted Graphic 5" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c11bc764-94cd-4ce9-b620-c81f83f3a18c">
>
> 참고로 여기서 노란부분은 ~(root)/tags.html의 href="#&#123;&#123;tag &#124; slugify&#125;&#125;" 와 연결되게 한 부분이다.
>
> <img width="164" alt="Pasted Graphic 6" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/4c85ea2c-4261-4e3e-8c82-1933a6b4c617">

# 끝



--------
### reference
[깃허브 블로그 만들기5 - 태그 기능 구현](https://harsik.github.io/github/2020/01/01/makeGithubBlog5.html)

[liquid문법, 전역변수처럼 사용하는 법](https://seungwubaek.github.io/front-end/liquid/function_with_jekyll/)