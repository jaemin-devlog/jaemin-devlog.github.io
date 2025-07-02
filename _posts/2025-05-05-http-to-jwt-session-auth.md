---
title: "세션 기반 인증 방식 — HTTP부터 JWT까지 (3)"
date: 2025-05-05 00:02:00 +0900
categories: [웹기초]
tags: [Session, Authentication, Spring Security, 쿠키, 로그인]
layout: post
---

## 세션 기반 인증 방식 — 고전이자 여전히 강력한 방법

웹 애플리케이션에서 사용자 인증(Authentication)은 반드시 필요한 기능입니다.  
그 중 가장 전통적이고 널리 사용되는 방식이 바로 **Session 기반 인증**입니다.

이번 글에서는 이 방식이 **어떻게 동작하는지**,  
**왜 여전히 유효한지**,  
**Spring Boot에서 어떻게 구현되는지**까지 정리해보겠습니다.

---

##  세션 기반 인증이란?

서버가 사용자의 로그인 상태를 기억하는 방식입니다.  
즉, 서버는 로그인 성공 시 **세션(Session)** 을 만들고,  
고유한 **Session ID** 를 클라이언트(브라우저)에게 전달합니다.

이 ID는 **쿠키(cookie)** 로 저장되고,  
이후의 요청마다 브라우저가 자동으로 포함해 서버에 전송합니다.

> 쉽게 말해,  
> “너 로그인했는지 내가 기억하고 있을게!”  
> — 서버

---

## 세션 기반 인증 흐름 요약 (8단계)

1️. **클라이언트 → 서버:** 로그인 요청 (ID/PW 전송)  
2️ **서버 → DB:** 사용자 자격 검증  
3. **서버 → 세션 저장소:** 세션 생성 + 사용자 정보 저장 (보통 메모리, Redis, DB)  
4️. **서버 → 클라이언트:** Session ID를 Set-Cookie로 전송  
5️. **클라이언트 → 서버:** 매 요청마다 쿠키로 Session ID 자동 포함  
6️. **서버 → 세션 저장소:** 세션 ID 유효성 검사  
7️. **서버 → DB:** 인증된 사용자로 데이터 처리  
8️. **서버 → 클라이언트:** 응답 반환

---

## 세션 인증 핵심 정리

- **Session은 로그인 수단이 아니다.**  
  → 로그인 **결과**로 생성된 상태를 유지하기 위한 장치일 뿐

- 세션이 만료되면 → 다시 로그인해야 새로운 세션이 생성됨

- 브라우저가 쿠키로 Session ID를 자동 관리해주기 때문에  
  **클라이언트 단에서 인증 처리 부담이 없음**

---

## Spring Boot에서 세션 기반 인증 설정하기

```java
@Configuration
@EnableWebSecurity 
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/login", "/register").permitAll()
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard", true)
                .permitAll()
            )
            .logout(logout -> logout
                .logoutUrl("/logout")
                .logoutSuccessUrl("/login?logout")
                .invalidateHttpSession(true)
                .deleteCookies("JSESSIONID")
            );

        return http.build();
    }
}
```

---

## 코드 설명

### 1. URL 접근 제어

```java
.authorizeHttpRequests(auth -> auth
    .requestMatchers("/login", "/register").permitAll()
    .anyRequest().authenticated()
)
```

- 로그인/회원가입 페이지는 인증 없이 접근 가능
- 그 외 모든 요청은 로그인 후 접근 가능

---

### 2. 로그인 설정

```java
.formLogin(form -> form
    .loginPage("/login")
    .defaultSuccessUrl("/dashboard", true)
    .permitAll()
)
```

- 로그인 성공 후 `/dashboard`로 리디렉트  
- `true` → 이전 URL 무시하고 고정된 페이지로 이동

---

### 3. 로그아웃 처리

```java
.logout(logout -> logout
    .logoutUrl("/logout")
    .logoutSuccessUrl("/login?logout")
    .invalidateHttpSession(true)
    .deleteCookies("JSESSIONID")
)
```

| 항목 | 설명 |
|------|------|
| `invalidateHttpSession(true)` | 서버의 세션 무효화 (메모리/Redis 제거) |
| `deleteCookies("JSESSIONID")` | 클라이언트의 쿠키 제거 (세션ID 삭제) |

→ **서버와 클라이언트 양쪽에서 완전한 로그아웃 구현**

---

## 세션 기반 인증의 장점과 단점

| 항목 | 장점 | 단점 |
|------|------|------|
| 상태 유지 | 서버가 로그인 상태를 기억 → UX 우수 | 서버 메모리 점유, 확장성 ↓ |
| 보안성 | 세션 저장소 관리 → 직접 통제 가능 | 쿠키 탈취 시 보안 위협 가능 |
| 구조 | 브라우저 쿠키 기반 자동 인증 | 모바일/REST API 대응 불편 |

---

## 요약

- 세션 기반 인증은 **서버 중심 인증 방식**입니다.
- 인증에 성공한 사용자에 대해 서버가 세션을 만들어 상태를 유지합니다.
- 브라우저는 이 세션 ID를 **쿠키로 자동 전송**하여 인증 상태를 유지합니다.
- Spring Boot에서는 **Spring Security 설정**만으로 간단히 구현 가능합니다.

---

## 🔜 다음 글 예고

다음 포스트에서는  
**JWT 기반 인증(Json Web Token)** 이  
세션 방식과 어떻게 다른지, 어떤 문제를 해결하기 위해 등장했는지를 다뤄보겠습니다.
