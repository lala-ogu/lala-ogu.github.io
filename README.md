# [블로그 바로가기](https://lala-ogu.github.io/)

## [블로그 댓글](https://c11oud.tistory.com/entry/GitHub-%EA%B9%83%ED%97%88%EB%B8%8C-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%8C%93%EA%B8%80-%EA%B8%B0%EB%8A%A5-%EC%B6%94%EA%B0%80)

## 수정 참고 링크
- [syh39](https://syh39.github.io/blog/github_blog_setting/)
- [커스터마이징](https://www.wonseoko.com/jekyll/minimal-mistakes/)

## 주의할 점
- 띄어쓰기는 대부분 하이픈(-)으로 작성
- 태그는 웬만하면 한글로 적었음
- 파일명 대문자는 허용하지 않음


## frontmatter 작성법
```
---
title: '제목'
excerpt: '간략한 설명'
tags: [태그,들을,넣는,곳]
header:
  teaser: /assets/images/
  overlay_image: /assets/images/
  overlay_filter: 0.4
---
```
## collapsible block 사용법
details 태그 안에는 markdown 작성을 못하므로, liquid 문법을 이용하여 capture의 변수와 치환하여 사용하여야합니다.  
모두 본문에 작성합니다. 변수는 영어로 작성해야합니다.
```liquid
{% capture summarymd %}이곳에 마크다운 작성{% endcapture %}
{% capture detailsmd %}이곳에 마크다운 작성{% endcapture %}

<details>
  <summary>
    {{ summarymd | markdownify | remove: '<p>' | remove: '</p>' }}
  </summary>
  
  {{ detailsmd | markdownify }}

</details>
```
## fork한 저장소와 merge하기
- [upstream](https://wikidocs.net/74836)

## git-secret
- [git-secret 윈도우](https://github.com/sobolevn/git-secret/tree/v0.5.0)
- [gpg 서명 push]([https://stuffstuff.tistory.com/3](https://funveloper.tistory.com/153))
- [wsl Ubuntu 설치](https://velog.io/@darktrace1/%EC%9C%88%EB%8F%84%EC%9A%B011%EC%97%90-UbuntuWSL2-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)
- [wsl 에러](https://learn.microsoft.com/ko-kr/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
- [git-secret](https://kimmj.github.io/git/git-secret/)
- [wsl 명령어](https://velog.io/@donghak/wsl)

## TIL 자동화
- [TIL 자동화](https://seulog.netlify.app/etc/TILGitHubAction/)
