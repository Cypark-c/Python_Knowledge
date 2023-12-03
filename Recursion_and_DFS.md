# Recursion, Why difficult?


- 종료조건 설정 (Base case) 잘못 설정하면 코드가 엄청 꼬임
- Value Error, RuntimeError 등 발생
    - Value Error의 경우에는 파이썬의 sys.setrecursionlimit(100000) 코드를 이용해 볼 수 있다.
## Tip
- <b>★재귀함수는 반드시 특정입력에 대해 자기 자신의 호출 없이 종료 (Basecase)</b>
- ★재귀함수의 모든 과정은 Base condition으로 수렴
- 재귀함수의 형태를 잘 잡아야 하며, 재귀함수는 반복문으로 완전히 동일하게 만들 수 있다

## 백준 18511 (https://www.acmicpc.net/problem/18511)

### 오답코드
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

- 오답코드의 케이스
    - 아래 예외가 발생함

```
655 3
6 7 9
609
```
- 600과 같이 10의 자리가 0인건 1을 빼면 가능할지 몰라도 위와 같은 케이스는 6을 한번 호출하여 100의 자리를 만들면, 
그 다음 십의자리 5, 일의자리 5에서는 넣을 수 있는 K의 원소는 아무것도 없으므로 600이 결과값으로 출력된다.
→바로 이와 같은 요인이 재귀함수를 어렵게 만듬

### 정답코드
```
'''
난 솔직히 어려웠음
약간 복불복 문제 같아서 좀..
그러나 예외처리를 굳이 반복문을 돌면서 안해도 될 수 있다.
다 만들고 나서 해도 됨. ←이걸 생각하는게 어려운 문제
'''
def make_nums(pos,result):
    if pos==len(str(N)):
        if not '0' in str(result):
            result_lt.append(result)
        return

    for key in K_list:
        now=key*(10**(len(str(N))-(pos+1)))+result
        if now<=N:
            make_nums(pos+1,now)
        else:
            make_nums(pos+1,result)

import sys
input=sys.stdin.readline
result_lt=[]
N,K=map(int,input().split())
K_list=list(map(int,input().split()))
K_list.sort(reverse=True) # 큰 수부터 대입
make_nums(0,0)
print(max(result_lt))
```

- 정답코드는 예외처리를 그때 그때 처리하는게 아니라. <b>Base condition 까지 일단 도달한 다음에 설정하고 있다.</b> 


# DFS의 로직

- 재귀함수와 완벽하게 동일하다.
- DFS는 상하좌우 모두 탐색을 하는데
    - 방향벡터를 설정하거나 (dr,dc)
    - 그래프 노드 간 연결관계를 참고 


## DFS 메서드 정의
def dfs(graph, v, visited):
	# 현재 노드를 방문 처리
    visited[v] = True
    print(v, end=' ')
    # 현재 노드와 연결된 다른 노드를 재귀적으로 방문
    for i in graph[v]:
    	if not visited[i]:
        	dfs(graph, i, visited)

## DFS 구현 시 유의사항

### 종료조건
- 재귀함수의 종료조건과 비슷함.


### 변수관리
- 변수관리란 무엇인가?
    - DFS에서의 변수 관리의 중요성은 for loop를 돌 때 발생한다. 예를 들어서 for loop 돌면서 재귀 함수를 실행할 수 있다.
```
for node in graph[v]:
    if node not in visited:
        DFS(node)
```
    - 이런 케이스에서 우선 DFS는 말 그대로 "Depth"를 먼저 완성하고자 하는 알고리즘이기 때문에 DFS가 계속 재귀적으로 호출된 다음에
    graph[v]의 다른 노드에 접근을 한다. 가령 위의 for문에서 graph[v]=[1,2,3] 이라고 가정을 하면, 1번 노드가 첫번째 for loop의 대상인데 1번으로 부터 쭉쭉 파내려가는 DFS를 실행을 하고, Base condition을 만나서 1로부터 시작된 함수들이 리턴을 하고 그 후에 다시 처음의 for문으로 온다.
        - 결국 1번 노드로 부터 실행된 결과는, 2번 노드 시작하여 순회할 때에 이미 반영이 되어있다는 의미이다.
        - 문제를 풀면서, 이전 state를 계속 반영을 해야하는지 반영을 하지 말아야 하는지 판단을 해야한다.
        - 이 부분이 Backtracking과도 연결된다.
