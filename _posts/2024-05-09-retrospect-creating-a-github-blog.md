---
title: "[회고] 깃허브 블로그 제작"
excerpt: "깃허브로 블로그를 만들어 보자!"

categories: # 카테고리 설정
  - 회고
tags: # 포스트 태그
  - [회고, 블로그제작]

permalink: /회고/[회고]-깃허브-블로그-제작/ # 포스트 URL

toc: true # 우측에 본문 목차 네비게이션 생성
toc_sticky: true # 본문 목차 네비게이션 고정 여부

date: 2024-05-09 # 작성 날짜
last_modified_at: 2024-05-09 # 최종 수정 날짜
---

### 📌 블로그 제작
---
여러 블로그 플랫폼을 거쳤지만, 결국 깃허브 블로그를 제작하게 되었다.  
물론 자체 제작은 아니고 [*choiiis*님의 블로그](https://github.com/choiiis/minimal-mistakes-choiiis-customized)를 fork해서 만들었다.  
  
블로그 설정은 choiiis님이 작성하신 README 파일만 보고도 쉽게 따라 할 수 있었다.   
  
약간 헤멨던 부분은 **댓글 기능**이었는데,  
블로그 댓글용 repo를 private로 팠더니 뜨지 않아서 **public**으로 바꿔주니 금방 해결했다.  

### 📌 update README.md
---
벨로그는 블로그 포스트를 **velog-readme-stats**를 통해 깃허브에서 보여줬다.  
하지만 깃허브 블로그는 불가능해서 다른 방법을 찾아보던 중 [*솜씨좋은장씨*님 블로그](https://somjang.tistory.com/entry/GitHub-%EB%82%B4-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EC%B5%9C%EC%8B%A0%EA%B8%80-%EA%B9%83%ED%97%99-%ED%94%84%EB%A1%9C%ED%95%84%EC%97%90-%EC%9E%90%EB%8F%99%EC%9C%BC%EB%A1%9C-%ED%91%9C%EC%8B%9C%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-feat-Python-GitHub-Actions)를 발견했다.    
자세한 설명은 위 블로그의 글을 참고하면 되고, 아래는 간단한 설명이다.  

1. **blog repo에 feed.xml 추가**
```xml
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.title | xml_escape }}</title>
    <description>{{ site.description | xml_escape }}</description>
    <link>{{ site.url }}{{ site.baseurl }}/</link>
    <atom:link href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" rel="self" type="application/rss+xml"/>
    <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
    <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
    <generator>Jekyll v{{ jekyll.version }}</generator>
    {% for post in site.posts limit:30 %}
      <item>
        <title>{{ post.title | xml_escape }}</title>
        <description>{{ post.content | xml_escape }}</description>
        <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
        <link>{{ post.url | prepend: site.baseurl | prepend: site.url }}</link>
        <guid isPermaLink="true">{{ post.url | prepend: site.baseurl | prepend: site.url }}</guid>
        {% for tag in post.tags %}
        <category>{{ tag | xml_escape }}</category>
        {% endfor %}
        {% for cat in post.categories %}
        <category>{{ cat | xml_escape }}</category>
        {% endfor %}
      </item>
    {% endfor %}
  </channel>
</rss>
```
 
2. **username repo에 update_readme.py 추가**
```python
import feedparser

somjang_blog_rss_url = "https://blogname.github.io/feed.xml"
rss_feed = feedparser.parse(somjang_blog_rss_url)

MAX_POST_NUM = 10

latest_blog_post_list = ""

MAX_POST_NUM = 10

for idx, feed in enumerate(rss_feed['entries']):
    if idx > MAX_POST_NUM:
        break
    feed_date = feed['published_parsed']
    latest_blog_post_list += f"[{feed_date.tm_year}.{feed_date.tm_mon:02}.{feed_date.tm_mday:02} - {feed['title']}]({feed['link']}) <br>\n"
    
markdown_text = """
기본 내용
"""

readme_text = f"{markdown_text}{latest_blog_post_list}"

with open("README.md", 'w', encoding='utf-8') as f:
    f.write(readme_text)
```

3. **github action 추가**  
- username repo 메뉴 중 Actions에서 **python application workflow**를 추가
- 아래와 같이 코드를 수정
```yml
# This workflow will install Python dependencies, run tests and lint with a single version of Python
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python

name: Python application

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: "0 0 */1 * *"

permissions:
  contents: write

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 3.12
      uses: actions/setup-python@v2
      with:
        python-version: 3.12
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install feedparser
    - name: Run Update Python Script
      run: |
        python update_readme.py
    - name: Update README.md file
      run: |
        git pull
        git add .
        git diff
        git config --local user.email "user@email.com"
        git config --local user.name "username"
        git commit -m "Update README.md"
        git push
```

### 📌 결과
---
**Main page**
![blog main](/assets/images/posts_img/retrospect-creating-a-github-blog/git_blog_home.png)
  
**Post page**
![blog post](/assets/images/posts_img/retrospect-creating-a-github-blog/git_blog_post.png)
  
**About page**
![blog about](/assets/images/posts_img/retrospect-creating-a-github-blog/git_blog_about.png)

**깃허브 프로필 update**
![blog about](/assets/images/posts_img/retrospect-creating-a-github-blog/github_profile_update.png)

### Reference
---
- [choiiis/minimal-mistakes-choiiis-customized](https://github.com/choiiis/minimal-mistakes-choiiis-customized)
- [[GitHub] 내 블로그 최신글 깃헙 프로필에 자동으로 표시하는 방법! ( feat. Python / GitHub Actions )](https://somjang.tistory.com/entry/GitHub-%EB%82%B4-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EC%B5%9C%EC%8B%A0%EA%B8%80-%EA%B9%83%ED%97%99-%ED%94%84%EB%A1%9C%ED%95%84%EC%97%90-%EC%9E%90%EB%8F%99%EC%9C%BC%EB%A1%9C-%ED%91%9C%EC%8B%9C%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-feat-Python-GitHub-Actions)
- [블로그에 RSS, JSON Feed 추가하기](https://bepyan.github.io/blog/nextjs-blog/7-rss)
