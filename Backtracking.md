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
  - 추가로 이런 생각을 할 수도 있다. 어 왜 visit은 되돌리는데 길이는 왜 안되돌리나? 그거엔 2가지 이유가 있다.
    - 우선 visit만 되돌리면 이전에 쓰였던 값은 그냥 덮이게 되어있다. 반복문을 실행하기 전의 리스트인 lst를 고정함으로서 받을 수 있는 효과다.
    - number_lt=number_lt[:-1] 이런식으로 해버리면 슬라이싱을 활용하겠다는 것인데
    - 슬라이싱은 리스트의 값을 <b>지우지 않는다</b> 단지 원본리스트가 그대로 존재하는데 원하고자 하는 범위만큼 출력을 한다. 그래서 이건 결국 뭐냐
   
    - ```
         for i in range(1,N+1):
        # 아직 방문하지 않았다면
        if N_visit[i]!=1:
            N_visit[i]=1
            N_arr.append(i)
            DFS_Bt(count+1,N_arr) # 길이 변형이 좀 문제// 원래의 모양을 변형시키면 안된다.
            N_visit[i]=0
            N_arr=N_arr[:-1]
            print(N_arr)
            print(i)
      ```
    - 이렇게 작성을 해버리면 예를 들어 N=4, M=2에서 시작한다고 하자.
    - 우선 [] 빈칸에서 시작을 해서 [1] 을 먼저 배열에 담은 후에 [1,2], [1,3], [1,4] 까지 한번 순회를 하지만, 그 이후에 다시 [1]이 남는다.
    - pop 연산, slicing 모두 처음 겉으로 보기에는 비슷하다. pop은 <b>원본배열</b>에서 뽑아내는 것이라 저 [1]도 제거한다. 그러나 slicing은 그렇게 안한다. slicing을 하면서 나타나는 현상은 slicing 할 때마다 N_arr의 id가 바뀌어버린다. 그건 결국 N_arr을 메모리 어딘가에 새로 만들었다는 것이다. 우선 지금 정리하기로는 [1,4] → [1] → [] 이렇게 돌아가야 하는 과정에서, N_arr이 slicing을 할 때마다 어느 시점에서 만든 N_arr을 참조할 것인가. 이걸 고민하게 되는 것이다.
    - 이 경우에는 [] → [1] → [1,2] →[1] 과정으로 갈 때 [1,2] → [1] 과정에서 만든 id인 2472817665408을 가진다.
    - [1] → [1,3] → [1] 이 과정에서 id 2472817665344 을 슬라이싱 과정에서 메모리 공간에 또 만들게 된다.
    - [1] → [1,4] → [1] 이 과정에서는? id 2472817665408 이 아이디는 [1,2] → [1] 과정에서 만든 id와 같게 되어버림
    - 그런데 [1] → [] 과정에서는 슬라이싱을 할 때에 아예 최초의 주소인 시작 id: 2246899696704 이걸 참조해 버리는데 문제는 [] → [1] → [1,2] 이렇게 가는 과정에서는 append 연산만 나와서 이 아이디는 변경이 안되었던 것이고, 그렇기 때문에 슬라이싱 할 때에 내 의도인 [1] →[] 이게 아니라 [1,2](id: 2246899696704) → [1] 이렇게 되어버리는 매우 황당한 경우가 발생한다.
    - 따라서 백트래킹 할 때 <b>절대로 절대로 슬라이싱을 사용해서는 안된다. 메모리 주소가 어떻게 돌아가는지 예측 불능!!!!!!</b>

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
- Chat GPT에 문의한 결과로는 일단 리스트 슬라이싱 자체는 새로운 리스트를 생성을 하는 것이다 보니. 저게 for 루프 안에서 생성이 되면, 그 루프 안에서만 공유가 된다. 이렇게 생각해 볼 수 있을 것 같음(추후 검증은 나중에)

## append, pop 으로 구현할 때는 이런 부분을 조심해야함
```
def DFS_B(start,count,Narr):
    if count==M:
        result.append(Narr) # Caution!!!!
        print("1",result,Narr,count)
        return

    # start 쓰는 방식이 훨씬 나음
    for i in range(start,N+1):
        Narr.append(i)
        print("0",result,Narr,count)
        DFS_B(i,count+1,Narr)
        print("2",result,Narr,count)
        Narr.pop()
        print("3",result,Narr)
```
- <b>저기서 Caution!! 이라고 되어있는 부분. 그냥 append를 해서는 안된다.</b>
  - Narr은 메모리 공간에서 계속 존재하고 있다. <b>사실은 이게 치명적임. 왜냐면 pop 연산을 할때 Result에 결과로서 들어가 있던 Narr이 같이 뽑혀짐.</b>
  - 결과로서 Result에 넣어 놓았는데 마치 포인터처럼 뽑히게되는 결과를 보여줌. 더군다나 Result에서 Narr이 지워지는 것도 아니라서 pop을 한 결과는 Narr 자체에서 원소
하나만 뽑힐 뿐이지 실상은 [[]] 이런 형태로 Result에 [] 로서 존재함. 그래서 여기에 Narr.append(2)를 하면 분명 Result.append(2)가 아님에도 결과는 황당함. [[2]] 로 바로 입력이 된다는 것. 그래서 웬만하면 result.append(Narr[:]) 이 방식을 이용하는게 뒤탈이 없음. 


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
  
