---
title: "배열을 활용한 다항식의 덧셈 구현"
description: "배열을 사용해 다항식을 구현하고 덧셈 연산을 처리하는 C언어 예제를 통해 자료구조 개념을 정리합니다."
date: 2025-05-04 22:00:00 +0900
categories: [자료구조]
tags: [C언어, 배열, 다항식, polynomial]
layout: post
---


## 다항식 덧셈이란?

다항식(Polynomial)은 변수와 계수로 구성된 수학 표현식입니다. 여러 항을 더하는 과정은 자료구조에서 중요한 예제로 자주 등장합니다.

이 글에서는 배열을 이용해 두 다항식을 더한 결과를 구하는 방법을 C언어로 구현해 보겠습니다.

---

## 다항식 덧셈 개요

- 다항식 $A(x)$ 와 $B(x)$ 를 더한 결과를 $D(x)$ 라고 할 때,  
  배열을 사용하여 각 항의 계수(coef)와 지수(expon)를 구조체로 저장합니다.
- 배열 인덱스 `avail`은 결과 다항식 $D(x)$의 시작점을 추적합니다.

---

## 구조체 정의 및 전역 변수

```c
#include <stdio.h>
#define MAX_TERMS 100

typedef struct {
    float coef;
    int expon;
} polynomial;

polynomial terms[MAX_TERMS];
int avail = 0;
```


---

## 항 비교 함수

```c
int compare(int a, int b)
{
    if (a == b) return 0;
    else if (a < b) return -1;
    else return 1;
}
```

---

## 핵심 함수: 다항식 덧셈 `padd`

```c
void padd(int starta, int finisha, int startb, int finishb, int* startd, int* finishd)
{
    float coefficient;
    *startd = avail;

    while (starta <= finisha && startb <= finishb)
        switch (compare(terms[starta].expon, terms[startb].expon)) {
            case -1:
                attach(terms[startb].coef, terms[startb].expon);
                startb++;
                break;
            case 0:
                coefficient = terms[starta].coef + terms[startb].coef;
                if (coefficient)
                    attach(coefficient, terms[starta].expon);
                starta++;
                startb++;
                break;
            case 1:
                attach(terms[starta].coef, terms[starta].expon);
                starta++;
                break;
        }

    for (; starta <= finisha; starta++)
        attach(terms[starta].coef, terms[starta].expon);
    for (; startb <= finishb; startb++)
        attach(terms[startb].coef, terms[startb].expon);

    *finishd = avail - 1;
}
```

---

## 항 삽입 함수 `attach`

```c
void err_prn()
{
    fprintf(stderr, "다항식에 항이 너무 많습니다.\n");
    exit(1);
}

void attach(float coefficient, int exponent)
{
    if (avail >= MAX_TERMS) {
        err_prn();
    }

    terms[avail].coef = coefficient;
    terms[avail++].expon = exponent;
}
```

---

## 다항식 입력 함수

```c
void input_polynomial(int* start, int* finish) {
    int num_terms;

    printf("다항식의 항의 개수를 입력하세요: ");
    scanf("%d", &num_terms);

    printf("다항식의 계수와 지수를 입력하세요:\n");
    *start = avail;

    for (int i = 0; i < num_terms; i++) {
        scanf("%f %d", &terms[avail].coef, &terms[avail].expon);
        avail++;
    }

    *finish = avail - 1;
}
```

---

## 결과 출력 함수

```c
void output_polynomial(int start, int finish) {
    printf("다항식의 결과는 다음과 같습니다:\n");

    for (int i = start; i <= finish - 1; i++) {
        printf("%.2fx^%d", terms[i].coef, terms[i].expon);
        if (i < finish - 1) printf(" + ");
    }
    printf("\n");
}
```

---

## main 함수

```c
int main() {
    int starta, finisha, startb, finishb;
    int startd, finishd;

    input_polynomial(&starta, &finisha);
    input_polynomial(&startb, &finishb);

    padd(starta, finisha, startb, finishb, &startd, &finishd);
    output_polynomial(startd, finishd);

    return 0;
}
```

---

## 실행 예시

```
다항식의 항의 개수를 입력하세요: 3
다항식의 계수와 지수를 입력하세요:
2 10
3 8
3 4
다항식의 항의 개수를 입력하세요: 2
다항식의 계수와 지수를 입력하세요:
3 8
5 5
다항식의 결과는 다음과 같습니다:
2.00x^10 + 6.00x^8 + 5.00x^5 + 3.00x^4
```

---

## 수식 정리

- $A(x) = 2x^{10} + 3x^8 + 3x^4$
- $B(x) = 3x^8 + 5x^5$
- $D(x) = A(x) + B(x) = 2x^{10} + 6x^8 + 5x^5 + 3x^4$

정상적으로 결과가 출력되며 배열 기반 다항식 덧셈이 잘 구현된 것을 확인할 수 있습니다.
