---

title:  "[minimal-mistakes] 카테고리(category) 만들기"
header:
  overlay_image: "https://images.unsplash.com/photo-1498050108023-c5249f4df085?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1744&q=80"
excerpt: "Golang"

categories: programming
tags:
  - [Blog, jekyll, Github]

toc_label: "목차"
toc: true
toc_sticky: true

# date: 2020-05-25
# last_modified_at: 2020-05-25
---


## 0️⃣시작

사이드바(SideBar)에 카테고리를 넣는 방법은 아래와 같습니다.  
1. navigation.yml에 Category 정의
2. post에 category 추가
3. page에 특정 category에 해당되는 post만 포함되도록 수정
4. Category와 page 연결
5. _config.yml을 수정하여 사이드바에 카테고리 표시

![](/images/2022-03-22-17-49-03.png)

## 1️⃣ navigation.yml 수정

아래와 같이 navigation.yml 하단에 추가합니다.  
url의 경우 각 카테고리에 맞게 각각 다르게 생성해야 합니다.


```
sidebar-category:
  - title: "카테고리"
    children:
      - title: "블로그"
        url: "/blog"
      - title: "카테고리B"
        url:  "/CategoryB"
      - title: "카테고리C"
        url:  "/CategoryC"
```
<div class="notice--info">
  <h4>❗ navigation.yml은 _data폴더에 위치합니다.</h4>
</div>


## 2️⃣ post 수정

_posts에 있는 Post문서(md파일)에 Category를 정의한다.

```
---
title:  "사이드바(SideBar) 카테고리 만들기"
categories:
  - blog
---
```



## 3️⃣ Page생성

root 디덱토리에 _pages폴더를 생성합니다. 이미 생성했다면 넘어갑니다.  
저는 C# 카테고리를 만들기 위해 category-blog.md파일을 생성하고 아래와 같이 수정하였습니다.  
- title: page에서 나타낼 제목을 설정합니다.
- layout: page에서 사용할 layout를 지정합니다. 
- permalink: page의 url 입니다. navigation.yml에 정의한 url를 입력합니다.

그리고 liquid for문을 통해 category중에 blog을 가진 post만 표시될 수 있도록 설정합니다.  

```html
---
title: "blog"
layout: archive
permalink: /blog
---

{% raw %}
{% assign posts = site.categories.blog %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
{% endraw %}
```
<div class="notice--info">
  <h4>❗ 다른 페이지를 추가 할 경우 title, permalink, "assign posts = site.categories.blog"를 카테고리에 맞게 수정합니다.  </h4>
</div>

## 4️⃣ _config.yml 수정
이제 만든 category를 sidebar에 등록해보겠습니다.  
저는 무조건 사이드바에 카테고리를 표시하게 할 것이므로 아래와 같이 수정하였습니다.  

```html
# Defaults
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: # true
      share: true
      related: true
      sidebar:                  # 추가한 부분
        nav: "sidebar-category" # 추가한 부분
```

<div class="notice--info">
  <h4>❗ 특정 post에서 sidebar을 끄고 싶은 경우 sidebar: false를 추가 합니다.</h4>
</div>


## 5️⃣ index.html수정
main화면에서 카테고리가 나타나지 않는다면, 아래와 같이 index.html에 sidebar 항목을 추가 합니다.

```
---
layout: archive
author_profile: true
sidebar:
    nav: "sidebar-category"
---
```

