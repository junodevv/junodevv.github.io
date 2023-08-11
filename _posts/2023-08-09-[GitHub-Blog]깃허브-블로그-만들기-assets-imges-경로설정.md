---
title: "[GitHub Blog]깃허브 블로그 만들기 assets images 경로 설정"
category: GitHub Blog
tags: ['github', 'blog']
---
default의 이미지가 md로 작성된 파일에서는 보이지 않는 문제 해결

## 문제

깃허브 블로그를 만들던중 큰 문제에 봉착했다.

<img width="700" alt="Pasted Graphic" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/0a967e5d-ea4c-484b-aba3-320f4ef76d72">

이렇게 index.html 에서는 잘 보이던 프로필 이미지와 footer의 이미지가

<img width="700" alt="Pasted Graphic 2" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/58a769eb-e4eb-4ea0-a35e-7b03eaa27f5c">

게시글이나 카테고리의 상세페이지같은 md로 만들어진 페이지로 들어갈때는 보이지 않는 문제이다.

구글링을 해봐도 대부분 minimal-mistake 테마를 적용해서 만들었기 때문에 내가 원하는 해결책을 찾을 수 없었다.

그래서 내 블로그의 페이지 소스를 살펴보았다.

## 원인

나의 블로그 페이지 소스를 살펴본 결과 

- index.html

> <img width="500" alt="Pasted Graphic 3" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/7e960efe-4229-4eef-8db0-99a10609783b">
>
> 경로: https://junodevv.github.io/assets/images/githubicon.png


- _posts/2023-07-27-third

> <img width="500" alt="Pasted Graphic 5" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/47197c0a-76fd-4e9d-8d19-40da8017e1ae">
>
> 경로: https://junodevv.github.io/test/2023/07/27/assets/images/githubicon.png

이렇게 적용된 경로가 다르게 나타남을 알 수 있었다.

md으로 작성된 페이지는 https://junodevv.github.io 뒤에 카테고리명과 날짜가 경로에 포함됨을 알 수 있었다.

## 해결방법

구글링을 통해 찾아보다보니 해결책을 없었지만 상대경로 앞에 ./와 같은 표기법이 있다는걸 알게 되었고 

기존에 경로를 표시할 때 뭔가 조커?같이 *. 이런걸 활용할 수 있다는걸 알고 있었기 때문에 

./, ../ 를 막 넣어보다보니 

> <img width="400" alt="Pasted Graphic 6" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/63e6c2f6-1a59-4491-88f3-05a51a9d5452">
>
> <img width="571" alt="Pasted Graphic 7" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/a32e8a7c-e6a1-468c-97e6-5cf42c3e7671">
>
> 경로: http://127.0.0.1:4000/test/2023/07/assets/images/githubicon.png

 "../"를 사용하면 경로가 사진 처럼 1단계 상위 폴더의 위치를 가리킨다는 것을 알게 되었다.

 그렇다면 test/2023/07/27 를 모두 거쳐 상위폴더인 https://junodevv.github.io 로 올라가면 될 것이라는 생각이들었고

 <img width="610" alt="Pasted Graphic 8" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/06364d2e-eaad-44ef-bce5-5541f454146a">

 이렇게 해주니 해결되었다.

 찾아보니 "../" 를 사용하면 해당 폴더의 상위 폴더로 이동하고 그 폴더 안에서 해당 파일을 찾는다고 한다.

 test/2023/07/27/ 를 ../../../../ 를 통해 한 단계씩 상위 폴더로 이동해서 결국 _site 폴더 까지 이동하고 거기에서 

images 파일을 찾는 것이다.
