---
layout: single
title: "백준 1918번 후위표기식"

---
>## 후위표기법 개념&백준 1918번

앞서 후위를 중위로 바꾸는 법을 배웠다. 이번 문제는 중위를 후위로 바꾸는 문제였는데, 전위,중위,후위의 개념이 부족하다는 것을 느꼈다. 

>## 전위,후위표기법 개념
우선순위를 고려하여 괄호 없이도 컴퓨터가 이해 하기 쉽게 만들어놓은 형태라고 기억 하면 된다. 바꾸는 법은 백준 1918번 문제에 자세하게 나와있다. 


![Problem](https://github.com/BlackHan26/BlackHan26.github.io/blob/master/1.png?raw=true)
   
___

* [문제링크](https://www.acmicpc.net/problem/1918)


## 알고리즘

1. 계산 입력을 받고 한 글자씩 확인하면서 연산자와 피연산자를 구분한다.
2. 피연산자를 받으면 바로 answer에 넣어준다.
3. 연산자를 받으면 무엇인지 if를 사용하여 분류하고 우선순위를 생각하며 **Stack**에 쌓아둔다.   
   Stack 가장 위에 있는 연산자가 다음 연산자보다 우선순위가 높으면 **pop**을 해준다.
* 우선순위
   *  ' ( '여는 괄호가 우선순위가 가장 높기에 바로 **Stack**을 해준다.
   *  ' * , / ' 곱하기와 나누기 연산자가 그 다음으로 높기때문에 **Stack**을 해주는데, Stk[-1]이 같은 순위의 연산자인지 확인한다. 만약 같다면 * / 가 아닐 때 까지 pop()을 해주고나서!! **Stack**을 해준다. 
   *  ' +,- ' 연산자는 가장 순위가 낮으므로 Stk에 있는 모든 연산자를 pop하고 나서 **Stack**해야 한다. 
   *  ' ) ' 닫는 괄호가 나오면 ' ( '여는 괄호가 나올 때 까지 모두 **POP** 해준다. 
___

> 내가 작성한 코드

``` py
import sys
input=sys.stdin.readline

word=input().strip()
stk=[]
answer=[]

for i in word:
    if i.isalpha()==1:
        answer.append(i)
    else:
        if i=='(':
            stk.append(i)
        elif i=='*'or i=='/':
            while stk and ( stk[-1]=='*' or stk[-1]=='/'):
                answer.append(stk.pop())
            stk.append(i)         
        elif i=='+' or i=='-':
            while stk and stk[-1]!='(':
                answer.append(stk.pop())
            stk.append(i)
        elif i==')':
            while stk and stk[-1]!='(':
                answer.append(stk.pop())
            stk.pop()
while stk:
    answer.append(stk.pop())
print("".join(answer))
```

___

## 나의 실패 Point & 배운 것 

* ### 우선순위
   우선순위를 크게 고려하지 않았다. 전위,중위,후위에서는 우선순위를 생각하는 것이 가장 중요하다.   
   무엇을 먼저 Stack하고 pop을 해야하는지가 결정되기 때문이다. 
   if문의 순서도 중요하다 .   
   처음 작성할 때 ` if i=='*'or i=='/':`을 먼저 쓰고, `elif i=='(':` 를  다음으로 쓰는 식으로 작성 하였는데,   
   이렇게 되면 여는 괄호보다 곱셈,나눗셈을 먼저 차리하게 되어서 괄호 안에서 곱셈,나눗셈을 처리하는 코드를 한번 더 작성해야 한다. 

* ### 조건문 형태
   조건문의 형태를 작성할때 예를 들어 `stk and ( stk[-1]=='*' or stk[-1]=='/'):` 를 쓸 때는 and 후에 괄호로 묶어주어야 한다. 이유는 괄호로 묶지 않으면 `stk and ( stk[-1]=='*'`이 먼저 나온 and로 묶이게 되고, 그다음 나오는 or로 처리되기 때문이다.
   
   또 `if a==4 or a==3 ` 과 `if a==4 or 3 ` 는 다르다는 것을 배웠다.
   후자같은 경우는 a와 상관없이 그냥 3이라는 값이 대입되면 무조건 참인 결과가 나온다.
