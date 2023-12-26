# Backtracking

- Constraint Satisfaction Problem에서 해를 찾기 위한 전략
  - 제약 조건을 지속 체크하다가 후보군이 제약조건에 걸리면, backtrack(다시는 체크하지 않음)설정 후 다른 후보군을 탐색하여 최적의 해를 찾음
  - 
- 실제 구현할 때는 우선 State Space Tree를 Drawing 해본다.
  - 보통, DFS로 들어가게 되는데, DFS니까 Recursion이 필연적이므로 종료조건에 유의한다.
  - 후보 군이 제약조건에 걸리면, 다른 후보군으로 넘어가 탐색함.
    - Promising: 해당 루트가 조건에 맞는가?
    - Pruning: 조건에 맞지 않으면 포기하고 다른 루트로 돌아가 탐색의 시간을 절약
    - <b>무엇보다 순회할 때의 후처리가 매우 중요함(아래 Image View 참고)</b>
- list를 사용한다면, append는 리턴이 None이니 조심해야함. (loop에서 하부함수 호출 시에 이 부분을 조심해야함)
- 제대로 되었다면, 루트노드를 한번 호출하는 것에서 백트래킹은 끝나야함. 메인함수에서 for문으로 돌린다? →정말 그렇게 복잡해야 하는지 생각해볼 것.
   
-  Code Frame
```
def dfs(n, lst):
    # 종료조건(n에 관련) 처리+정답처리
    if n==M:    # M개의 수열을 완성
        ans.append(lst)
        return

    # 하부단계(함수) 호출
    for j in range(1, N+1):
        if v[j]==0: # 선택하지 않은 숫자인 경우 추가
            v[j]=1
            dfs(n+1, lst+[j])
            v[j]=0

N, M = map(int, input().split())
ans = []            # 정답 리스트를 저장할 리스트
v = [0]*(N+1)       # 중복확인을 위한 visited[]
```

- <b>for문이 특히 중요함! 처리 방식을 완벽하게 따라해야한다.</b>
- 특히 for 루프의 방문했던 노드 취소하는 방식
- 특히 for 루프 내부의 dfs의 실행에서 lst를 처리하는 방식
  - <b>보면 알 수 있지만, 원래 lst를 변형하지 않도록 lst+[j]와 같은 형태로 사용하고 있음!</b>
  - 결론은 append를 써도 되기는 하지만, 함수를 처리하고 나서 append가 변형된 값이 <b>for loop의 다음 인덱스에 반영되지 않도록 해야한다.</b>


```
#만약에 pop 등을 이용한다면 이런 식으로 작성할 수도 있긴하다.

def DFS_BT(number_lt,visited):

    if len(number_lt)==k:
        # 결과 append
        Str=''.join(number_lt)
        if Str not in result:
            result.append(Str)
        return

    # 방문노드를 append했으면 방문하지 않은 곳 중에서 하나를 골라서 들어감
    for index in range(len(visited)):
        # 아직 방문하지 않은 노드면 방문
        if visited[index]==0:
            # 방문노드 append
            number_lt.append(str(cardlist[index]))
            visited[index] = 1  # 1은 해당 노드를 방문했다는 의미
            DFS_BT(number_lt,visited) # 여기서 오류나기 쉬움
            # 해당 노드는 방문했으면 다시 원복 시켜주어야 함
            visited[index]=0 # 원복 ㅇㅇ 여기까진 알겠음
            #number_lt=number_lt[:-1]
            number_lt.pop() # 이것도 number_lt=number_lt[:-1] 이랑 다름 ㅋㅋㅋㅋ 미친
```

- <b>그런데 주석에 달아놓았듯이 pop 대신에 slicing을 이용하려고 하면 매우 크나큰 문제가 발생할 수 있음</b>

- Image View
- ![image](https://github.com/Cypark-c/Python_Knowledge/assets/76925535/146d0738-c133-40ed-8481-f9cc0b9ee2c3)


## Recursive function review

반드시 두 가지 경우가 존재해야함
- Base case: 계산하지 않아도 한번에 답이 바로 나오는 경우
  - 회귀의 종료를 의미함. 기본 조건을 만족한다면 바로 <b>종료</b>

- Recursive: Not Base Case
  - 자기자신의 호출이 진행됨.
  - 재귀의 깊이가 깊어지면 빅-오로 미쳐 날뛰기 때문에 $O(2^n)$ Time Complexity에 악영향

## Backtracking과 Recursion의 연결
  - Backtrack을 한다는건 일단 Base case로 진행을 한다는 의미이며 이는 Recursive하게 알고리즘을 진행하겠다는 것과 같다.
  - 대표적인 문제로 BOJ N과 M이 있다. [[https://www.acmicpc.net/problem/15649]]
  
