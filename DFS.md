# Recursion

# 재귀가 어려운 이유

## 백준 18511 (https://www.acmicpc.net/problem/18511)
```
'''
[아래 코드의 예외 케이스는 아래와 같음]
655 3
6 7 9
609
'''

def num_attach(num,pos,result):
    # Base case
    if pos==-1:
        print(result)
        return

    # 0이 들어있으면 없애야함.
    if num%10==0:
        num-=1

    for i in range(len(K_sets)-1,-1,-1):
        if (result+K_sets[i]*(10**pos)<=num):
            result+=K_sets[i]*(10**pos)
            break

    # print(num,pos,result)
    num_attach(num, pos - 1, result)
    return

import sys
input=sys.stdin.readline
N,K=map(int,input().split())
K_sets=list(map(int,input().split(' ')))
K_sets.sort()
result=''
num_attach(N,len(str(N))-1,0)
```

# DFS의 로직


# DFS 구현 시 유의사항

## 종료조건



## 변수관리
- 변수관리란 무엇인가?

