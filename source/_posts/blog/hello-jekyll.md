---
title: "Github + Jekyll로 빠르게 블로그 만들기"
date: 2017-05-07
categories: Blog
tags: jekyll
---

뻘짓경험을 기록해두기 위해 `도쿠위키`와 `미디어위키`를 알아보다가 심플한 theme를 우연히 발견하여 `jekyll`로 급 변경하였다.

## 0. 준비사항 (Mac OS X 기준)
맥엔 기본적으로 `Ruby`가 깔려있다. `RubyGems`이 업데이트가 필요할 경우 아래 명령어를 실행 [참고](http://jekyllrb-ko.github.io/docs/troubleshooting/)
```
$ sudo gem update --system
$ xcode-select --install
```

## 1. Jekyll 설치
```
$ sudo gem install jekyll
```

## 2. Jekyll을 이용하여 블로그 생성 및 실행
```
$ jekyll new 블로그이름
$ jekyll serve --watch
```

위와같이 원하는 블로그이름을 넣어서 생성해도 되지만, [이곳](http://jekyllthemes.org/)에서 이쁜 테마를 찾자

이 블로그는 [Kiko-plus 테마](https://github.com/AWEEKJ/Kiko-plus)를 이용하여 만들었다.

## 3. github에 등록
2에서 만든 폴더를 자신의 github에 푸시하면 블로그가 완성!
이때 github repository 이름을 `깃헙사용자이름.github.io`로 만들어야한다.

## 4. Troubleshooting
1. 테마버그인지 모르겠으나, 사용하지 않는 author를 삭제하고 push하면 github에서 `Page build failure` 이메일을 보낸다.
한참을 방황하다 찾은 결과, `_config.yml`파일에 다른 author는 상관없으나 `author.twitter`를 삭제하면 위에 이메일을 보낸다.
어쩔 수 없이 `_config.yml`에 `twitter` 계정입력은 그대로 냅두고, `_includes.footer.html`안에서 해당 태그를 직접 삭제하였다.
2. `jekyll serve --watch` 실행했을때 watch 옵션이 먹히질 않았다. 한참을 해매다가 [stackoverflow](http://stackoverflow.com/questions/43559969/jekyll-auto-regeneration-not-working-even-with-watch-used/43560078)에서 원하는 답을 찾게되었다.
사용하는 gems중에 `jekyll-admin`이 있는데 `v0.4.1` 버전에서 `--watch`옵션이 `disabled` 되어 있다고 한다. 임시방편으로 해당 gem을 주석처리하였다.


###### 많은 부분이 생략하고 최대한 간단하게만 작성해두었는데, 자세한 내용은 참고 사이트를 확인하자.
참고 사이트
 - [Jekyll 한국어 공홈](https://jekyllrb-ko.github.io/)
 - [Kiko-plus theme](https://github.com/AWEEKJ/Kiko-plus)
 - [워니님 블로그](https://brunch.co.kr/@hee072794/39)
 - [놀부님 블로그](https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/)