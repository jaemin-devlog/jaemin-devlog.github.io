---
title: "후위연산의 정의와 코드 구현"
date: 2025-06-29 03:02:00 +0900
categories: [자료구조]
tags: [c언어, 스택, 후위연산, postfix, 자료구조]
---

## 후위연산의 정의와 필요

후위연산(Postfix Notation)은 연산자들이 피연산자 뒤에 위치하는 표기법입니다.  
중위 연산에 비해 연산자의 우선순위를 따지지 않아도 되어 컴퓨터에서 계산을 수행할 때 유용합니다.

제가 학교에서 방학 때 배우고 있는 자료구조의 후위연산을 직접 코드 작성해보려고 합니다.

---

## 후위연산의 특징

- **우선순위 불필요** : 중위연산과 달리 연산자 우선순위를 고려할 필요가 없습니다.  
- **괄호 불필요** : 연산의 순서가 명확하므로 괄호를 사용할 필요가 없습니다.  
- **스택 사용** : 후위연산을 사용하기 위해 주로 스택(Stack)을 사용합니다.

---

## 후위연산의 예제

예시: `(3+4) * 5 - 6`  
→ 후위연산: `3 4 + 5 * 6 -`

계산 순서:
1. `3 4 +` → `7`
2. `7 5 *` → `35`
3. `35 6 -` → `29`

---

## 후위연산 코드 작성 (C언어)

```c
#include <stdio.h>
#include <stdlib.h>
#define MAX_SIZE 100

typedef struct {
    int item[MAX_SIZE];
    int top;
} Stack;

void push(Stack* s, int add);
int pop(Stack* s);
int eval(char post[]);
int Precedence(char c);
void postfix(char* infix, char* postfix);
void Stack_Empty();
void Stack_Full();
void Stack_Empty() {
    fprintf(stderr, "Stack is Empty!\n");
    exit(1);
}

void Stack_Full() {
    fprintf(stderr, "Stack is Full!\n");
    exit(1);
}
void push(Stack* s, int add) {
    if (s->top >= MAX_SIZE - 1) {
        Stack_Full();
        return;
    }
    s->item[++(s->top)] = add;
}

int pop(Stack* s) {
    if (s->top == -1) {
        Stack_Empty();
    }
    return s->item[(s->top)--];
}
main 함수
int main() {
    char infix[MAX_SIZE];
    char post[MAX_SIZE];
    
    printf("중위식을 후위식으로 바꾸는 프로그램입니다.\n");
    printf("중위식을 입력하시오 : ");
    fgets(infix, sizeof(infix), stdin);
    
    int len = strlen(infix);
    if (len > 0 && infix[len - 1] == '\n') {
        infix[len - 1] = '\0';
    }
    
    printf("\n입력한 중위 표기식 : %s", infix);
    
    postfix(infix, post);
    printf("\n변환한 후위 표기식 : %s", post);

    int result = eval(post);
    printf("\n계산 결과 : %d\n", result);

    return 0;
}
Precedence 함수
int Precedence(char c) {
    switch(c) {
        case '(': case ')': return 0;
        case '+': case '-': return 1;
        case '*': case '/': return 2;
    }
    return -1;
}
postfix 변환 함수
void postfix(char* infix, char* postfix) {
    char postfix_index = 0;
    char current;
    int len = strlen(infix);
    Stack s;
    s.top = -1;

    for (int i = 0; i < len; i++) {
        current = infix[i];
        switch (current) {
            case '+': case '-': case '*': case '/': case '%':
                while (s.top != -1 && Precedence(current) <= Precedence(s.item[s.top])) {
                    postfix[postfix_index++] = pop(&s);
                }
                push(&s, current);
                break;
            case '(':
                push(&s, current);
                break;
            case ')':
                while (s.item[s.top] != '(') {
                    postfix[postfix_index++] = pop(&s);
                }
                pop(&s); // pop '('
                break;
            default:
                postfix[postfix_index++] = current;
                break;
        }
    }

    while (s.top != -1) {
        postfix[postfix_index++] = pop(&s);
    }

    postfix[postfix_index] = '\0';
}

eval 함수 (후위연산 계산)
int eval(char* post) {
    Stack s;
    int op1, op2;
    int len = strlen(post);
    s.top = -1;

    for (int i = 0; i < len; i++) {
        char current = post[i];
        if (current == ' ')
            continue;
        else if (current >= '0' && current <= '9') {
            push(&s, current - '0');
        } else {
            op1 = pop(&s);
            op2 = pop(&s);
            switch (current) {
                case '+': push(&s, op2 + op1); break;
                case '-': push(&s, op2 - op1); break;
                case '*': push(&s, op2 * op1); break;
                case '/': push(&s, op2 / op1); break;
            }
        }
    }

    return pop(&s);
}
마무리
후위연산을 구현하면서 C 언어에서의 문자열 처리와 스택 활용 방법을 복습할 수 있었고,
main 함수부터 전체 흐름을 잡아가며 구조적으로 작성하니 훨씬 이해가 쉬웠습니다.
![후위연산 다이어그램](assets/img/postfix-c-example.png)

