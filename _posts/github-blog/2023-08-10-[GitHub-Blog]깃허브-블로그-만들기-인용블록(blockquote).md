---
title: "[GitHub Blog]깃허브 블로그 만들기 - 인용블록(blockquote)"
category: GitHub Blog
tags: github blog blockquote 인용블록
---
마크다운형식으로 작성된 post에서 인용블록(blockquote)이 생기지 않는 문제 해결

## 문제

원래 마크다운 언어에서 \> 를 사용하면 인용 블록이 생긴다.
><img width="542" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/275fd9d4-1ae3-404f-bd33-3d6d4433f9a8">

하지만 내 블로그 게시글에는 그게 나타나지 않는 현상이 발생했다.

> <img width="638" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/e8471161-f6c8-443c-b132-0c300a017fe3">
>
> 위 사진은 \> 를 사용하여 작성한 부분인데 아무런 인용블록의 표시가 없음을 알 수 있다.

## 원인

인용블록이 나오는 다른 사이트에서 페이지소스를 살펴보니(velog를 참고했다.) 

> <img width="484" alt="Pasted Graphic 11" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/e822f138-07d3-441f-b620-44ef07143849">
>
> blockquote 라는 테그를 사용하여 인용블록을 표현함을 알 수 있었다.

> <img width="301" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/ba4eaea9-19fc-43c1-9193-b3c039cf7c33">
>
> 그리고 그 테그에 대한 css를 추가하여 사용 하고 있음을 확인했다.

나는 blockquote 테그에 대한 css를 추가해 주지 않아서 기본 css가 적용되어 있기 때문에 인용블록이 적용되지 않은 것 처럼 보였던 것이다.

## 해결방법

해결방법은 간단하다 blockquote 테그에 대한 css를 추가해주면 된다.

> _sass/main.scss
>
> ![Pasted Graphic 15](https://github.com/junodevv/junodevv.github.io/assets/126752196/eaedc92f-66d4-4843-aabb-b3a8c80929e7)
>
> blockquote 테그에 대한 css 를 추가해줬다.

><img width="712" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/f9b77aaf-679a-4e4b-9892-cc63db0764b6">
>
> 인용 블록에 css가 적용되어 정상적으로 표현됨을 확인 할 수 있다.

# 끝
