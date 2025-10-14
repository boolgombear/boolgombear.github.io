---
title: "Chirpy 테마 설치 완료"
date: 2025-10-14 09:00:00 +0900
categories: [Blog]
tags: [jekyll, chirpy, github-pages]
# image:
---

GitHub Pages 저장소에 [Chirpy 테마](https://github.com/cotes2020/jekyll-theme-chirpy)를 연결했습니다.   
이제 `posts` 폴더에 글을 추가하면 자동으로 블로그에 노출됩니다.

## 글 작성 방법

1. `_posts` 폴더에 `YYYY-MM-DD-title.md` 형식의 새 파일을 만듭니다.  
2. 파일 상단에 YAML 프론트매터(front matter)를 작성합니다.

```markdown
---
title: "새 글 제목"
date: 2025-10-14 09:00:00 +0900
categories: [Diary]
tags: [life, memo]
---

여기에 본문을 작성하세요.
```

3. 로컬에서 `bundle exec jekyll s` 명령으로 확인하거나 GitHub에 푸시하면 자동으로 배포됩니다.

## 다음 할 일

- `_config.yml`에서 프로필, SNS 링크, 댓글/통계 설정을 본인 정보로 수정합니다.
- `_tabs/about.md`에 소개글을 적어 사이드바의 About 페이지를 채워주세요.
- 이미지나 첨부 파일은 `assets` 아래에 정리하면 깔끔합니다.

즐거운 블로깅 되세요! 🐻

