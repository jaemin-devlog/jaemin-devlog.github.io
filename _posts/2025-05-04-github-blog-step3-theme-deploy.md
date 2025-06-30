---
title: "[3편] Github 블로그, 테마 적용하고 내 블로그 세상에 공개하기"
date: 2025-07-02 00:00:00 +0900
categories: [개발블로그]
tags: [GitHub, Jekyll, Chirpy, Blog Theme, GitHub Pages]
layout: post
---

## 이제 진짜 블로그답게 만들어볼까요?

Jekyll 설치와 로컬 실행까지 성공하셨다면,  
이제는 예쁜 테마를 적용하고 세상에 내 블로그를 공개해볼 차례입니다 😄

Jekyll 기본 테마인 `minima`는 조금 심심한 감이 있죠.  
그래서 이번엔 깔끔하고 반응형인 **Chirpy 테마**를 적용해볼 거예요.

---

## Chirpy 테마 추천 이유

- ✅ 깔끔한 디자인
- ✅ 모바일에서도 완벽 대응
- ✅ GitHub Pages와 완벽 호환
- ✅ 커스터마이징 쉬움

처음 블로그를 시작하는 분들에게 매우 적합합니다.

---

## 🎨 Chirpy 테마 설치 & 적용하기

### 1️⃣ Chirpy Starter 테마 설치

> **비추천:** 원본 리포지토리 fork 방식  
> **✅ 추천:** `chirpy-starter` 레포지토리 clone 방식 (바로 사용 가능)

GitHub 링크 👉 [https://github.com/cotes2020/chirpy-starter](https://github.com/cotes2020/chirpy-starter)

---

### 2️⃣ 테마 Clone 하기

터미널에서 원하는 폴더 위치로 이동한 뒤 아래 명령어 실행:

```bash
git clone -b main https://github.com/cotes2020/chirpy-starter.git jaemin-devlog.github.io
```

---

### 3️⃣ Gem 패키지 설치

```bash
cd jaemin-devlog.github.io
bundle install
```

---

### 4️⃣ `_config.yml` 수정

아래 항목들을 본인 정보에 맞게 수정합니다:

```yaml
title: Jaemin Devlog
tagline: 개발 기록 블로그
url: "https://jaemin-devlog.github.io"

github:
  username: jaemin-devlog

social:
  name: Jaemin
  email: jjm0203311@naver.com

timezone: Asia/Seoul
theme_mode: dark
```

---

### 5️⃣ 로컬 서버 실행 (테마 확인)

```bash
bundle exec jekyll serve
```

브라우저에서 접속 👉 [http://localhost:4000](http://localhost:4000)  
→ Chirpy 테마가 적용된 화면이 보이면 성공! 🎉

---

### 6️⃣ GitHub 원격 저장소 확인

```bash
git remote -v
```

`origin` 경로가 다르다면 아래 명령어로 수정:

```bash
git remote set-url origin https://github.com/jaemin-devlog/jaemin-devlog.github.io.git
```

---

### 7️⃣ GitHub에 첫 Push

```bash
git add .
git commit -m "Initial commit with Chirpy starter"
git push origin main
```

---

### 8️⃣ GitHub Pages 설정

GitHub 저장소 → `Settings` → `Pages`  
- `Source`: `main` / `(root)`
- 설정 후 `Save`

잠시 후, 다음 메시지가 뜨면 완료!

> Your site is live at https://jaemin-devlog.github.io

---

## 📝 9️⃣ 첫 블로그 글 작성하기

`_posts` 폴더 안에 새 마크다운 파일을 만듭니다:

```text
2025-06-25-first-post.md
```

예시 내용:

```markdown
---
layout: post
title: "첫 포스트"
date: 2025-06-25 17:24:00 +0900
categories: [Devlog]
tags: [test]
published: true
---

첫 번째 블로그 글입니다.

Markdown 문법을 자유롭게 사용해서 예를 들면:

## 소제목

- 리스트 1
- 리스트 2

**강조 텍스트**  
일반 텍스트  
[링크 예시](https://jaemin-devlog.github.io)
```

작성 후 Push하면, 블로그에 글이 자동으로 반영됩니다!

---

## ✅ 마무리 체크리스트

- [x] Chirpy 테마 적용 완료
- [x] GitHub Pages 배포 완료
- [x] 첫 글 작성 완료 (잔디도 쌓임!)

---

## 🔜 다음 편 예고: [4편] 블로그 커스터마이징 (메뉴, 카테고리, SNS 연결 등)

지금까지는 "틀"을 만들었다면,  
다음부터는 진짜 "내 스타일"로 블로그를 꾸며가는 시간을 가질 거예요 😊  
계속 함께 해요!
