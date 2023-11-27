# Backtracking

- Constraint Satisfaction Problem에서 해를 찾기 위한 전략
  - 제약 조건을 지속 체크하다가 후보군이 제약조건에 걸리면, backtrack(다시는 체크하지 않음)설정 후 다른 후보군을 탐색하여 최적의 해를 찾음
  - 
- 실제 구현할 때는 우선 State Space Tree를 Drawing 해본다.
  - 보통, DFS로 들어가게 되는데, DFS니까 Recursion이 필연적이므로 종료조건에 유의한다.
  - 후보 군이 제약조건에 걸리면, 다른 후보군으로 넘어가 탐색함.
    - Promising: 해당 루트가 조건에 맞는가?
    - Pruning: 조건에 맞지 않으면 포기하고 다른 루트로 돌아가 탐색의 시간을 절약
   
-  Code Frame
-  ```
  def checknode(v): #node
    if promising(v):
    if there is a solution at v:
        write the solution
      else:
        for u in each child of v:
            checknode(u)
   ```

## Recursive function review

반드시 두 가지 경우가 존재해야함
- Base case: 계산하지 않아도 한번에 답이 바로 나오는 경우
  - 회귀의 종료를 의미함. 기본 조건을 만족한다면 바로 종료

- Recursive: Not Base Case
  - 자기자신의 호출이 진행됨.
  - 재귀의 깊이가 깊어지면 빅-오로 미쳐 날뛰기 때문에 $O(2^n)$ Time Complexity에 악영향

## Backtracking과 Recursion의 연결
  - Backtrack을 한다는건 일단 Base case로 진행을 한다는 의미이며 이는 Recursive하게 알고리즘을 진행하겠다는 것과 같다.
  - 대표적인 문제로 BOJ N과 M이 있다. [[https://www.acmicpc.net/problem/15649]]