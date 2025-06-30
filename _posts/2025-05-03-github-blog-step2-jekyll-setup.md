---
title: "[2편] Ruby, Jekyll 설치부터 나만의 블로그 첫 실행까지"
date: 2025-05-03 00:00:00 +0900
categories: [개발블로그]
tags: [GitHub, Jekyll, Ruby, GitHub Blog, 설치 가이드]
layout: post
---

## 블로그는 많지만, 나는 직접 만들고 싶었어요

국내에서는 **네이버 블로그**, **티스토리** 등 다양한 플랫폼이 많이 사용되고 있죠.  
조금 더 고급스러운 블로그를 원한다면 **워드프레스**도 많이 쓰이고요.

하지만 이런 서비스들은 구조나 기능이 제한적이라  
**개발 공부를 하며 직접 만들고 싶다**는 나에게는 조금 답답하게 느껴졌습니다.

워드프레스는 설치나 관리 부담, 비용이 걸림돌이었고요.

그러다 발견한 게 바로 **GitHub Pages + Jekyll** 조합이었습니다.

---

## GitHub Pages + Jekyll의 장점은?

- **무료**로 운영 가능
- **빠르고 가볍다**
- HTML/CSS 지식이 없어도 **마크다운**으로 글 작성 가능
- 디자인/기능도 자유롭게 **커스터마이징** 가능

그래서!  
개발자가 아니어도 **직접 블로그를 만드는 경험**을 할 수 있도록  
이 시리즈를 쓰게 되었습니다. 저도 처음엔 생소했지만, 충분히 누구나 할 수 있어요.

---

## Jekyll이란?

> Jekyll = "마크다운으로 작성한 글을 HTML로 변환해주는 도구"

- **정적 사이트 생성기**이고
- **Ruby 언어** 기반이며
- 대부분의 GitHub 블로그는 Jekyll 기반입니다

공식 사이트 👉 [https://jekyllrb.com](https://jekyllrb.com)

---

## Step 1. Ruby 설치

Jekyll은 Ruby 기반이라 먼저 Ruby를 설치해야 합니다.

🔗 [https://www.ruby-lang.org/en/downloads/](https://www.ruby-lang.org/en/downloads/)

설치할 때는 **기본값으로 진행하면 OK**  
단, 꼭! ✅ `MSYS2 development toolchain` 체크박스는 활성화된 상태로 설치해야 합니다.

---

### MSYS2 설치 단계

- 설치 중간에 **검은 창(CMD)**이 뜰 수 있어요
- 그냥 **엔터 키** 한 번만 누르면 자동 설치됩니다
- `succeeded!` 메시지가 초록색으로 뜨면 성공입니다
- 이후에는 창을 닫아도 됩니다

---

### 설치 확인

Windows 키 + `R` → `cmd` → 엔터  
명령 프롬프트에서 아래 명령어를 실행합니다:

```bash
ruby -v
```

→ 버전 정보가 출력되면 성공 🎉

---

## Step 2. Jekyll 설치

터미널(cmd)에 아래 명령어 입력:

```bash
gem install jekyll bundler
```

설치 완료 후 확인:

```bash
jekyll -v
# 예: jekyll 4.4.1
```

정상 출력되면 성공입니다!

---

## Step 3. Jekyll 블로그 만들기

이제 나만의 블로그 뼈대를 만들어볼게요 😎

터미널에서 원하는 위치로 이동 후 아래 명령어:

```bash
jekyll new myblog
```

> `myblog` 자리는 원하는 폴더명으로 바꿔도 OK  
예: `jekyll new jaemin-blog`

실행하면 해당 폴더 안에 블로그 파일들이 자동으로 생성됩니다.

---

## Step 4. 블로그 실행

```bash
cd myblog
bundle install
bundle exec jekyll serve
```

이후 아래 메시지가 뜨면 성공:

```
Server address: http://127.0.0.1:4000/
```

브라우저에서 `http://localhost:4000` 접속하면  
✨ 나만의 Jekyll 블로그가 열립니다!

---

## ⚠️ Deprecation Warning?

서버 실행 시 뭔가 경고 메시지가 뜨더라도 걱정 마세요.  
테마(minima)의 스타일 문법 관련 경고일 뿐, **에러가 아니며 블로그 작동에는 문제 없습니다.**

---

## 🎉 여기까지 완료!

- Ruby 설치
- Jekyll 설치
- 블로그 프로젝트 생성
- 로컬 실행 확인

**이제 나만의 GitHub Blog 뼈대가 완성되었습니다! 👏**

---

## 🔜 다음 편 예고: [3편] 테마 적용과 GitHub Pages 배포

다음 글에서는:

- Jekyll 테마 적용하는 법
- 실제 GitHub Pages에 블로그를 배포하는 방법

을 아주 쉽고 자세하게 알려드릴 예정입니다 😊  
계속 함께 해요!
