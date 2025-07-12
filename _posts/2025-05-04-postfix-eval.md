---
title: "후위연산의 정의와 코드 구현"
date: 2025-05-04 09:0:00 +0900
categories: [자료구조]
tags: [C언어, 스택]
layout: post
---


## 후위연산의 정의와 필요

후위연산(Postfix Notation)은 연산자가 피연산자 뒤에 위치하는 표기법입니다.  
중위 표기법보다 연산 순서를 쉽게 처리할 수 있어 컴퓨터가 계산하기에 유리합니다.

제가 방학 중 배우고 있는 자료구조 내용 중 하나로, 후위연산을 C언어로 구현해봤습니다.

---

## 후위연산의 특징

- **우선순위 불필요** : 연산자 우선순위를 따로 고려하지 않아도 됩니다.
- **괄호 불필요** : 연산 순서가 명확하므로 괄호를 사용할 필요가 없습니다.
- **스택 사용** : 주로 스택(Stack)을 활용하여 구현합니다.

---

## 예시 변환

중위식 `(3 + 4) * 5 - 6` → 후위식 `3 4 + 5 * 6 -`

계산 과정:

1. `3 4 +` → `7`  
2. `7 5 *` → `35`  
3. `35 6 -` → `29`

---

## 후위연산 코드 작성 (C언어)

```c
//후위표기식 1자리
#include <stdio.h>
#include <stdlib.h>
#define MAX_SIZE 100

typedef struct {
	int item[MAX_SIZE];
	int top; // 1. 코드의 모듈화와 재사용성을 높이고 스택 관리의 안전성을 보장
}Stack;


void push(Stack* s, int add);
int pop(Stack* s);
int eval(char post[]); // 후위연산 계산함수
int Precedence(char c); // 우선순위 반환함수
void postfix(char* infix, char* postfix);
void Stack_Empty();
void Stack_Full();

```
먼저 헤더파일 선언 후 100개의 공간을 가진 item의 배열, top을 Stack구조체에 선언합니다.

그 후 전역변수로 top을 -1로 초기화를 한 후 필요한 함수들을 적었습니다.

## main 함수

```c
int main()
{
	char infix[MAX_SIZE];
	char post[MAX_SIZE];
	printf("중위식을 후위식으로 바꾸는 프로그램입니다.\n");
	printf("중위식을 입력하시오 : ");
	fgets(infix, sizeof(infix), stdin);
	int len = strlen(infix);
	if (len > 0 && infix[len - 1] == '\n') {
		infix[len - 1] = '\0';
	}
	printf("\n입력한 중위 표기식 : %s",infix);

	postfix(infix, post);
	printf("\n변환한 후위 표기식 : %s", post);

	int result = eval(post);
	printf("\n계산 결과 : %d\n", result);

	return 0; 
}
```
함수를 작성하기 전 main함수를 작성하였습니다. 

함수보다 main함수 먼저 작성하면 더 생각하기 편하더라구요..

​

c언어를 잘 사용하지 않다 보니까 문자열 입력받는 방법을 까먹었는데 다시 요약해보겠습니다.

​

fgets() 사용법 :

fgets(buffer, size, stream);

buffer : 문자열을 저장할 배열포인터

size : buffer의 최대 저장 가능한 문자 수

stream : 입력을 받을 스트림 지정  - 보통 'stdin'을 사용함.

​

fgets() 주의사항 :  

fgets()는 개행문자('\n')을 만나면 입력을 멈추지만 이 개행문자를 버퍼에 포함시킴.

이를 처리하기 위해 NULL문자로 대체하거나 제거해야함.## push함수


## Stack의 기본 Push, pop

```c
void Stack_Empty()
{
	fprintf(stderr, "Stack is Empty!\n");
	exit(1);
}
void Stack_Full()
{
	fprintf(stderr, "Stack is Full!\n");
	exit(1);
}

void push(Stack* s, int add)
{
	if (s->top >= MAX_SIZE - 1)
	{
		Stack_Full();
		return;
	}
	s->item[++(s->top)] = add;
}

int pop(Stack* s)
{
	if (s->top == -1)
	{
		Stack_Empty();
		s->top = -1;
	}
	return s->item[(s->top)--];
}

```
Stack의 기본 코드 push와  pop코드를 작성하였습니다.

## postfix 변환 함수

```c
void postfix(char* infix, char* postfix)
{
	char postfix_index = 0; //후위연산 배열
	char current; // 현재 문자
	int len = strlen(infix);
	Stack s;
	s.top = -1;

	for (int i = 0; i < len; i++)
	{
		current = infix[i];
		switch (current)
		{
		case '+':
		case '-':
		case '*':
		case '/':
		case '%':// 현재문자가 연산자일 경우
			while (s.top == -1 && Precedence(current) <= Precedence(s.item[s.top])) // top이 -1이거나 현재 연산자가 스택에 있는 연산자보다 우선순위가 낮으면
			{
				postfix[postfix_index++] = pop(&s);
			}
			push(&s, current);
			break;
		case '(':
			push(&s, current);
			break;
		case ')':
			while (s.item[s.top] != '(')
			{
				postfix[postfix_index++] = pop(&s);
			}
			pop(&s); //왼쪽괄호를 pop함
			break;
		default: //피연산자일 경우
			postfix[postfix_index++] = current; //후위 표기식에 바로 삽입
			break;
		}
	}
	while (s.top != -1)
	{
		postfix[postfix_index++] = pop(&s);
	}

	postfix[postfix_index] = '\0';

}
```
가장 중요한 postfix함수...

main함수에서 infix(중위연산식) 배열을 입력받고 postfix로 변환하는 과정을 코드로 구현했습니다.
```c
char postfix_index = 0; //후위연산 배열
char current; // 현재 문자
int len = strlen(infix);
Stack s;
s.top = -1;
```
먼저,  postfix_index라는 변수를 선언 후 0으로 초기화합니다 . postfix_index는  새로운 후위연산식을 나타냅니다.

current는 입력받은 infix의 배열에서 현재 문자를 나타냅니다.

infix의 길이를 len에 저장 후 for문으로 0번째 인덱스부터 infix의 길이까지 연산을 합니다.

```c
current = infix[i];
switch (current)
{
case '+':
case '-':
case '*':
case '/':
case '%':// 현재문자가 연산자일 경우
	while (s.top != -1 && Precedence(current) <= Precedence(s.item[s.top])) // top이 -1이 아니고 현재 연산자가 스택에 있는 연산자보다 우선순위가 낮으면
	{
		postfix[postfix_index++] = pop(&s);
	}
	push(&s, current);
	break;
case '(':
	push(&s, current);
	break;
```
변환과정

연산자일 경우  : current(현재 문자)가 '+', '-', '*', '/', %'일 경우에는  스택에 push를 합니다.

  여기서 top이 -1이 아니고 current가 스택에 있는 연산자보다 우선순위가 낮을 경우에는 현재 스택에 들어있는 연산자를 먼저 pop을 하여 postfix에 넣습니다.

```c
case '(':
	push(&s, current);
	break;
case ')':
	while (s.item[s.top] != '(')
	{
		postfix[postfix_index++] = pop(&s);
	}
	pop(&s); //왼쪽괄호를 pop함
	break;
```
괄호일 경우: 

왼쪽 괄호 : 왼쪽괄호 나올 경우 무조건  스택에 push를 합니다.

오른쪽 괄호 : 오른쪽 괄호가 나올 경우 왼쪽괄호가 나올 때까지 pop을 하여 postfix배열에 넣습니다. 그 후 pop을 하여 왼쪽괄호도 제거합니다.

```c
default: //피연산자일 경우
	postfix[postfix_index++] = current; //후위 표기식에 바로 삽입
	break;
```
피연산자일 경우  : 피연산자일 경우에는 후위표기식에 바로 삽입을 합니다.

```c
중위식을 후위식으로 바꾸는 프로그램입니다.
중위식을 입력하시오 : 1+4*3

입력한 중위 표기식 : 1+4*3
변환한 후위 표기식 : 143*+
```
현재까지의 결과물입니다. 문제 없이 잘 출력되는 걸 볼 수 있습니다. 휴우.. ㄴㅇㅅ

​

이제 후위연산까지 잘 바꿨으니 계산을 해야겠죠?

마지막 하나 남은 eval함수를 작성해줍시다.

## eval 함수 (후위연산 계산)

```c

int eval(char* post){
	Stack s;
	int op1,op2;
	int len = strlen(post);
	s.top = -1;
	for (int i = 0; i < len; i++)
	{
		char current = post[i];
		if (current == ' ')
			continue;
		else if (current >= '0' && current <= '9')
		{
			push(&s, current - '0');
		}
		else{
			op1 = pop(&s);
			op2 = pop(&s);
			switch (current)
			{
			case '+':
				push(&s, op2 + op1);
				break;
			case '-':
				push(&s, op2 - op1);
				break;
			case '*':
				push(&s, op2 * op1);
				break;
			case '/':
				push(&s, op2 / op1);
				break;
			}
		}
	}
	return pop(&s);
}
```
eval함수 전체 코드입니다
```c
char current = post[i];
if (current == ' ')
	continue;
else if (current >= '0' && current <= '9')
{
	push(&s, current - '0');
}
```
len의 길이만큼 for문을 돌린 후 post배열의 문자들을 current에 저장시킴.

만약 current가 공백일 경우엔 무시

current가 피연산자일 경우엔 정수로 변환 후 push
```c
else{
	op1 = pop(&s);
	op2 = pop(&s);
	switch (current)
	{
	case '+':
		push(&s, op2 + op1);
		break;
	case '-':
		push(&s, op2 - op1);
		break;
	case '*':
		push(&s, op2 * op1);
		break;
	case '/':
		push(&s, op2 / op1);
		break;
	}
}
```
연산자일 경우에는 pop을 하여 계산을 한다

주의사항 : op1을 먼저 꺼냈다 -> op1이 op2보다 스택 구조 상 위에 있다. -> 계산할 때 op2가 앞으로 가서 계산해야함.
```c
return pop(&s);
```
마지막으로 pop을 하여 계산값을 main함수로 넘긴다.

```c
int result = eval(post);
printf("\n계산 결과 : %d\n", result);
```
result에  저장 후  출력
```c
중위식을 후위식으로 바꾸는 프로그램입니다.
중위식을 입력하시오 : 3*4+(3-2)+1

입력한 중위 표기식 : 3*4+(3-2)+1  
변환한 후위 표기식 : 34*32-+1+  
계산 결과 : 14
```
3·4+(3−2)+1을 계산했을 때 34*32−+1+으로 후위연산식으로 바뀐 후 14로 계산이 성공적으로 출력된다.​


