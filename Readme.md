# boolgombear.github.io

Chirpy 테마로 꾸민 BoolgomBear의 개인 GitHub Pages 블로그입니다.

## 프로젝트 소개
- 기술 실험과 기록을 남기는 블로그.
- Chirpy 테마(`jekyll-theme-chirpy`)를 사용하여 사이드바, 태그, 카테고리, PWA 등을 지원합니다.

## 빠르게 시작하기
1. Ruby 3.3 이상과 Bundler 설치.
2. 의존성 설치  
   ```bash
   bundle install
   ```
3. 로컬 미리보기  
   ```bash
   bundle exec jekyll serve --livereload
   ```
4. 브라우저에서 <http://127.0.0.1:4000> 접속.

## 새 글 작성
- `_posts/YYYY-MM-DD-title.md` 형식으로 파일 생성.
- 파일 상단에 프론트매터 작성.
  ```markdown
  ---
  title: "글 제목"
  date: 2025-10-14 09:00:00 +0900
  categories: [Diary]
  tags: [jekyll, chirpy]
  ---
  ```
- 마크다운 본문을 채우고 커밋/푸시하면 GitHub Actions가 자동 배포합니다.

## Chirpy 설정 체크리스트
- `_config.yml`  
  - `title`, `tagline`, `description`, `lang`, `timezone` 등을 실제 정보로 수정.  
  - `social.links`에 GitHub, Twitter, 이메일 등 링크 추가.  
  - 댓글(`comments`)이나 분석(`analytics`) 기능을 쓰려면 제공자 정보를 채웁니다.
- `_tabs/about.md` 에 소개글 작성.
- `assets` 폴더에 프로필 이미지, 첨부 파일 등을 정리하고 `/assets/...` 경로로 사용.

## GitHub Pages 배포
- 저장소 **Settings → Pages**에서 Source를 GitHub Actions로 설정.
- `main` 브랜치에 푸시하면 `.github/workflows/pages-deploy.yml` 워크플로가 실행되어 `_site`를 Pages에 배포.
- Actions 탭에서 `Build and Deploy` 상태를 확인하세요.

## 자주 쓰는 명령
- 캐시 정리: `bundle exec jekyll clean`
- 프로덕션 빌드: `JEKYLL_ENV=production bundle exec jekyll build`
- 링크 검증: `bundle exec htmlproofer _site --disable-external`

필요한 설정을 채우고 글을 추가하면 바로 라이브 블로그로 활용할 수 있습니다. 즐거운 블로깅 되세요! 🐻
