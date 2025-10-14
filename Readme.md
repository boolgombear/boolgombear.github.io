# boolgombear.github.io

Chirpy 테마 기반으로 꾸민 개인 GitHub Pages 블로그입니다.

## 로컬에서 실행하기
- Ruby 3.3 이상과 Bundler가 설치되어 있어야 합니다.
- 최초 실행:
  1. `bundle install`
  2. `bundle exec jekyll serve`
- 브라우저에서 <http://127.0.0.1:4000> 으로 접속하면 미리보기 가능합니다.

## 새 글 작성 방법
- `_posts/YYYY-MM-DD-title.md` 형식으로 파일을 추가합니다.
- 파일 상단에는 아래와 같이 프론트매터를 작성합니다.

  ```markdown
  ---
  title: "글 제목"
  date: 2025-10-14 09:00:00 +0900
  categories: [Diary]
  tags: [life, memo]
  ---
  ```

- 작성이 끝난 후 `git push` 하면 GitHub Actions가 빌드와 배포를 자동으로 진행합니다.

## 필수 설정 체크리스트
- `_config.yml`에서 `title`, `tagline`, `social` 정보를 본인에게 맞게 수정하세요.
- 댓글이나 웹 분석 기능을 사용하려면 `comments`, `analytics` 항목을 채워야 합니다.
- About 페이지 내용은 `_tabs/about.md` 파일에서 수정할 수 있습니다.
- 프로필 이미지 등 정적 파일은 `assets` 폴더에 저장하고 마크다운에서 `/assets/...` 경로로 불러오면 됩니다.

## GitHub Pages 배포 설정
- 리포지토리 → Settings → Pages에서 **Source**를 GitHub Actions로 설정합니다.
- 첫 배포가 완료되면 Actions 탭에서 `Build and Deploy` 워크플로우가 성공했는지 확인하세요.

행복한 블로깅 되세요! 🐻
