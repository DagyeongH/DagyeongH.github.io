---
title: "[회고] 깃허브 블로그 제작기"
excerpt: "깃허브로 블로그를 만들어 보자!"

categories: # 카테고리 설정
  - Retrospective
tags: # 포스트 태그
  - [회고, 블로그제작]

permalink: /retrospective/[회고]-깃허브-블로그-제작기/ # 포스트 URL

toc: true # 우측에 본문 목차 네비게이션 생성
toc_sticky: true # 본문 목차 네비게이션 고정 여부

date: 2024-05-08 # 작성 날짜
last_modified_at: 2024-05-08 # 최종 수정 날짜
---

### 📌 블로그 제작
여러 블로그 플랫폼을 거쳤지만, 결국 깃허브 블로그를 제작하게 되었다.  
물론 자체 제작은 아니고 [*choiiis*님 블로그](https://github.com/choiiis/minimal-mistakes-choiiis-customized)를 fork해서 만들었다.  
  
블로그 설정은 choiiis님이 작성하신 README 파일만 보고도 쉽게 따라 할 수 있었다.   
  
약간 헤멨던 부분은 **댓글 기능**이었는데,  
블로그 댓글용 repo를 private로 팠더니 뜨지 않아서 **public**으로 바꿔주니 금방 해결했다.  

파일을 업로드 후 포스팅이 안되는 문제가 발생했다.  
블로그 설정을 처음부터 다시 해보고, 카테고리 이름도 영어로 바꿔봤지만 해결하지 못했다.  
이후, 다른 블로그를 통해 포스팅 날짜가 **utc 기준**이라는 것을 알게됐고,  
**하루 전 날짜**로 변경하니 포스팅 성공했다!     
앞으로 새벽에 글을 쓸 일이 있다면 전 날짜로 작성해야겠다.  

### 📌 update README.md   
벨로그는 블로그 포스트를 **velog-readme-stats**를 통해 깃허브에서 보여줬다.  
하지만 깃허브 블로그는 불가능해서 다른 방법을 찾아보던 중 [*솜씨좋은장씨*님 블로그](https://somjang.tistory.com/entry/GitHub-%EB%82%B4-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EC%B5%9C%EC%8B%A0%EA%B8%80-%EA%B9%83%ED%97%99-%ED%94%84%EB%A1%9C%ED%95%84%EC%97%90-%EC%9E%90%EB%8F%99%EC%9C%BC%EB%A1%9C-%ED%91%9C%EC%8B%9C%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-feat-Python-GitHub-Actions)를 발견했다.    

자세한 설명은 위 블로그의 글을 참고하면 되고, 아래는 간단한 설명이다.  

1️⃣ **blog repo에 feed.xml 추가**     
 
2️⃣ **username repo에 update_readme.py 추가**   

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

3️⃣ **github action 추가**    
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
**Main page**   
![blog main](/assets/images/posts_img/retrospect-creating-a-github-blog/git_blog_home.png)  
  
**Post page**  
![blog post](/assets/images/posts_img/retrospect-creating-a-github-blog/git_blog_post.png)  
  
**About page**  
![blog about](/assets/images/posts_img/retrospect-creating-a-github-blog/git_blog_about.png)  

**깃허브 프로필 update**  
![blog about](/assets/images/posts_img/retrospect-creating-a-github-blog/github_profile_update.png)  

### Reference  
- [choiiis/minimal-mistakes-choiiis-customized](https://github.com/choiiis/minimal-mistakes-choiiis-customized)   
- [[GitHub] 내 블로그 최신글 깃헙 프로필에 자동으로 표시하는 방법! ( feat. Python / GitHub Actions )](https://somjang.tistory.com/entry/GitHub-%EB%82%B4-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EC%B5%9C%EC%8B%A0%EA%B8%80-%EA%B9%83%ED%97%99-%ED%94%84%EB%A1%9C%ED%95%84%EC%97%90-%EC%9E%90%EB%8F%99%EC%9C%BC%EB%A1%9C-%ED%91%9C%EC%8B%9C%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-feat-Python-GitHub-Actions)   
- [블로그에 RSS, JSON Feed 추가하기](https://bepyan.github.io/blog/nextjs-blog/7-rss)   
- [해결 깃허브 pages 블로그 만들기 중 _posts 파일 업로드 안됨](https://velog.io/@jurije/%ED%95%B4%EA%B2%B0-%EA%B9%83%ED%97%88%EB%B8%8C-pages-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-%EC%A4%91-posts-%ED%8C%8C%EC%9D%BC-%EC%97%85%EB%A1%9C%EB%93%9C-%EC%95%88%EB%90%A8)
