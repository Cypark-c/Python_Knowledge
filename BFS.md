# 전형적인 해결 전략

- 함수 구현은 일반적으로 DFS에 비해 쉬움 (재귀가 아니기 때문)
- <b>★주요 전략 중 하나로 큐에 노드들을 추가할 때 현재 위치 +1 과 같은 방식으로 숫자를 늘려나가면서 얼마나 멀리 이동했는지 확인하는 방식이 있음!</b> 대표문항: [백준 7576:토마토](https://www.acmicpc.net/problem/7576)
 - 이 전략은 1000번쯤 강조해도 전혀 지나치지 않음! 절대 잊어서는 안되는 전략 중 하나다.
- <b>BFS를 돌면서 순전히 방문을 체크하기 위해서 추가적인 배열을 선언할 수 있다.</b>

# 주의점

- 큐에 집어넣기 전에! 방문처리를 해야만 함! 그래야 중복 방문을 하지 않음!(그림을 그려보면 무슨말 인지 파악 가능)
- <b>반드시 1칸</b>

# 대표예시

- 아기 상어 https://www.acmicpc.net/problem/16236

## 잘못된 접근
```
'''
[아이디어]

*크기
1.아기상어는 자신보다 작은 물고기는 먹을 수 있음
2.크기와 '같은 수'의 물고기를 먹으면 크기가 1증가

*탐색
1.더 잡아먹지 못하면 엄마상어 소환 후 종료
2.자신과 크기가 같다면 먹을수는 없으나 지나갈 수는 있음
3.상어의 이동은 1초가 걸림.
    - 현재상태에서 물고기가 여러마리 있고 거리가 같으면 가장 위, 가장 왼쪽 순서대로 먹음

예시
4 3 2 1
0 0 0 0
0 1 S 1
2 1 3 4

S=2, count=1

4 3 2 1
0 0 0 0
0 S 0 1
2 1 3 4

### 중요 ###
- 만약에 처음 1단계에서 우측 1을 넣었다가 빼면 좀 웃기는 상황이 벌어짐. 가장 가까운 물고기를 먹지 못함
그렇기 때문에! 가까운 것 중에서 하나만을 넣는 방식으로 구현해야함

- 그리고 그 가까운 것을 판정하기 위해서 위치를 받아야하고
그 위치에 해당하는 걸 먹으러 가야함 →함수든 뭐든 구현이 필요함



[시간복잡도]
N=20 정도에 불과함. 상어가 물고기를 잡아먹을때 이동에는 모든 칸 잡아먹으면 O(N^2)
다음 BFS 수행 시간도 필요하긴 함.

'''

# BFS 함수, i,j는 처음 아기상어의 위치
def BFS(p,q):

    graph[p][q] = 0 # 큐는 항상 집어넣기 전에 방문처리 먼저 해줘야함
    queue=deque([(p,q)]) # 초기 위치를 넣어 준다.
    path=0 # 초기 상어의 이동거리
    size=2 # 초기 상어의 사이즈
    count=0 # 몇 마리를 잡아먹었나
    distance=0 # 초기 distance는 0으로 본다.

    while queue:
        # 현재 가장 가깝고 문제의 조건에 맞는 (위쪽부터, 왼쪽부터) 물고기 소환
        # 애초에 이 위치는 무조건 상어가 어떤 방법으로든 이동 가능해야함을 전제로 깜
        # print(queue)
        i,j=queue.popleft()
        # 거리를 늘려줌
        distance+=1
        if distance>=N**2:
            break

        # 상어 위치 갱신, 현재 사이즈에서 잡아먹은 물고기 수 갱신
        if i!=p or j!=q:
            # 상어의 이동거리 갱신 (몇 초를 버티느냐) 이건 위치 갱신 전에 먼저 해야함
            path += abs(p - i) + abs(q - j)
            p, q = i, j


        # 현 상황의 상어 위치에서 가장 가깝고 먹을 수 있는 물고기 탐색 (Investigate)
        x,y=Investigate(p,q,distance,size) # None이면 문제 될 수 있다
        # print((i,j),(x,y),distance)

        if x!=p or y!=q: # 현재 위치가 아닌 경우에 위 함수는 서칭에 성공함
            # Investigate에서 반환한 함수 append, 잡아먹을 수 있는 물고기 찾음
            graph[x][y]=0 # 잡아먹을 수 있음! 미리 방문처리함
            count += 1 # 물고기를 하나 잡아먹음
            distance=0 # 잡아먹었으면 거리 초기화
            print("상어의 사이즈는 이래요:",size,"잡아먹었어요:",count)

            # 현재 잡아먹은 물고기가 상어의 사이즈와 같다면
            if count == size:
                count = 0  # 카운트 초기화
                size += 1  # 상어가 성장했어요!
                print("성장했어요:", size)

            print("\n")
            for line in graph:
                print(line)
            print('\n')
            queue.append((x,y))
        elif x==p and y==q:
            queue.append((p,q)) # 서칭 실패하면 이거 다시 넣어줘야함! 중요!


    print("\n")
    print(path)
    return path

# 현재 위치에서 가장 접근 가능하고 잡아먹을 수 있는 물고기 찾기 (Investigate 함수)
# 방향을 탐색해야 함! 이 함수는 문제의 조건을 만족하는 가장 최적의 물고기 위치를 반환함
'''
BFS에서 매 distance에 대해 실행해 줌. 그리고 distance가 뭔가 행렬 벗어나는 상황이면
그 때는 상어가 먹이를 찾지 못하므로 종료시킴
'''
def Investigate(p,q,distance,size):
    pos_list=deque()
    for i in range(-distance,distance+1):
        if i==-distance:
            j=0
            pos_list.append((i,j))
        elif abs(i)<distance:
            j=-(distance-abs(i))
            pos_list.append((i,j))
            j=(distance-abs(i))
            pos_list.append((i,j))
        else: # i==distance
            j=0
            pos_list.append((i,j))
    # 위에까지 진입 가능한 모든 포지션 담았음
    # 이제 해당 위치가 접근 가능한 위치인지 확인 필요함.
    # 즉, 해당 distance 안에 갈 수 있는가
    # print(pos_list)
    flag=0
    while pos_list:
        a,b=pos_list.popleft()
        np=p+a
        nq=q+b
        # print((np,nq),end=' ')
        if -1<np<N and -1<nq<N:
            '''
            # distance를 3이라 가정해보자. 
            distance가 애초에 3이 되었다면 거리가 1이거나 2이거나...에서
            아무것도 찾지 못하거나 진행 불가해서 이렇게 된 것이다.
            그러니 여기서 굳이 거리가 1일때 장애물이 있고 거리가 2일때 장애물이 있고
            따질 필요는 전혀 없다. 애초에 그 전에 distance=1,2에서 도달 불가능 했던 것임
            '''
            if size>graph[np][nq] and graph[np][nq]!=0:
                print("방문위치:",np,nq,"상어크기:",size,"물고기 크기:",graph[np][nq])
                return np,nq
    # 서칭에 실패했을 경우 원래 위치인 p,q를 반환
    return p,q

from collections import deque
import sys
input=sys.stdin.readline
N=int(input())
graph=[]
for _ in range(N):
    graph.append(list(map(int,input().split())))
print(graph)

# 방향벡터 북쪽, 서쪽, 동쪽, 남쪽.... 필요할지 안 필요할지 지금으로서 모름
dr=[-1,0,0,1] # 행
dc=[0,-1,1,0] # 열

# 처음 상어 위치 조사
first_x,first_y=0,0
for p in range(N):
    for q in range(N):
        if graph[p][q]==9:
            print(p,q)
            first_x,first_y=p,q
            break
print("초기위치:",(first_x,first_y))
print("\n")
BFS(first_x,first_y)
```

- 복잡하게 구현했는데 안타깝게도 문제가 있었다.
 - 일단 BFS가 기본적으로 1칸식 움직인다는 기본적인 사항을 망각했다는 것이다.
 - 위의 Investigate 함수는 마름모 형태로 탐색하면서 거리에 따라 가능한 부분을 확인하려고 하는 것인데, 이렇게 최단거리를 탐색하다가 중간에 막혀버리면 어떻게 할 것인가? 에 대해 대책이 없거나 매우 복잡한 구현이 필요할 것 같다.
 - <b>1칸씩!← 사실상 이걸 안지켜서 대차게 꼬인 코드</b>

## 정답코드

```
'''
[아이디어]
가까운 먹이를 찾기에 BFS로 생각할 수 있음
BFS로 입력할때 현재 위치를 아기상어의 위치로 입력하고 먹을 수 있는 포지션을 찾음
'''

# BFS를 탐색하며 먹이가 될 수 있는 후보들을 반환해야함
# 그 후보를 cand라고 하자
def BFS(x, y):
    visited = [[0] * N for _ in range(N)]
    queue = deque([[x, y]])
    cand = []

    visited[x][y] = 1

    while queue:
        i, j = queue.popleft()

        for idx in range(4):
            ii, jj = i + dr[idx], j + dc[idx]

            if 0 <= ii and ii < N and 0 <= jj and jj < N and visited[ii][jj] == 0:
                # 5. 간선은 상하 좌우지만 조건에 따라서 움직이기 때문에 조건을 상세하여야한다.
                if graph[x][y] > graph[ii][jj] and graph[ii][jj] != 0:
                    visited[ii][jj] = visited[i][j] + 1
                    cand.append((visited[ii][jj] - 1, ii, jj))
                elif graph[x][y] == graph[ii][jj]:
                    visited[ii][jj] = visited[i][j] + 1
                    queue.append([ii, jj])
                elif graph[ii][jj] == 0:
                    visited[ii][jj] = visited[i][j] + 1
                    queue.append([ii, jj])

    # 6. 후보 리스트는 우선 순위가 있기 때문에 정렬을 사용할 수 있다.
    return sorted(cand, key=lambda x: (x[0], x[1], x[2]))

from collections import deque
N=int(input())
graph=[list(map(int,input().split())) for _ in range(N)]

# 초기위치 설정
p,q=0,0
for i in range(N):
    for j in range(N):
        if graph[i][j]==9:
            p,q=i,j
            break


# 방향벡터 설정
dr=[-1,0,0,1]
dc=[0,-1,1,0]

# BFS로 나온 것 중 맨 앞에 나온 물고기만 먹고 다시 BFS 해야함
size=2 # 초기 상어 사이즈
cnt=0
ate=0 # 얼마나 먹었나

while True:
    graph[p][q]=size # 이게 중요한데 갱신을 하지 않으면 잡아먹다가 포기해버림
    cand=deque(BFS(p, q))
    # 큐에서 하나 뽑았는데 위치가 없으면 이제 BFS해도 의미 없다는 것이니 break
    if not cand:
        break
    time,np,nq=cand.popleft()

    cnt+=time
    ate+=1


    if ate==size:
        ate=0
        size+=1
    graph[p][q]=0
    p,q=np,nq

print(cnt)
```

- visited graph를 따로 선언하여 현 상태에서 갈 수 있는 위치만을 탐색하였음
 - 갈 수 있는 위치는 전부 아기 상어가 먹을 수 있는 후보 먹이들
 - 그런데 문제에 의하면 가장 가까운 거리이면서도 맨 위쪽, 그리고 왼쪽 먹이를 우선적으로 먹음
 - 그렇게 해서 후보 중 가장 최우선을 deque 자료구조를 활용해서 뱉어냄. (while 반복문에서 popleft())
   
- <b>먹이를 먹으면 사이즈를 계속 갱신 해주어야함</b>
