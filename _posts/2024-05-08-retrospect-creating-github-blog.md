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
last_modified_at: 2024-05-14 # 최종 수정 날짜
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

1️⃣ **blog repo에 feed.xml 추가**     
 
2️⃣ **username repo에 update_readme.py 추가**   

3️⃣ **github action 추가**    
- username repo 메뉴 중 Actions에서 **python application workflow**를 추가   

### 📌 feed.xml 오류
![feed error](/assets/images/posts_img/retrospect-creating-a-github-blog/feed_error.png)  
/feed.xml에 접속했을 때 위와 같은 오류가 발생했다.  
구글링 해 본 결과 밑에 써져 있는 파일에서 쓰면 안되는 문자가 생겨서 그렇다는 것을 보고,  
위에 update README.md에 써져있는 코드블럭을 제거했더니 해결됐다!  


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
- [Jekyll Blog, PCDATA invalid Char value Error 해결 방법](https://samslow.github.io/development/2019/01/28/Jekyll-Blog,-PCDATA-invalid-Char-value-Error-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95/)