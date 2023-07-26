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

## 5. 