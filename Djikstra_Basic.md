# 수정 및 보완 예정 
일단은 잠을 자야 하니 여기까지만

```
# INF 정하기

import sys
input=sys.stdin.readline
INF=int(1e9)

# 노드의 개수, 간선 입력받기 (그래프 초기화)
n,m=map(int,input().split())

# 시작노드
start=int(input())

# 각 노드에 연결되어 있는 노드에 대한 저옵를 담는 리스트를 만들기
graph=[[] for _ in range(n+1)] # 간선정보 초기화
visited=[False]*(n+1) # 노드 방문 여부 초기화
distance=[INF]*(n+1) # 최단거리 테이블 초기화

for _ in range(m):
    a,b,c=map(int,input().split()) # 간선정보 수집
    graph[a].append((b,c)) # 튜플형태로 간선정보를 입력

# print(graph) # 출력하면 그래프 확인 가능

'''
[다익스트라 알고리즘]
1. 현재의 노드에서 갈 수 있는 노드를 파악한다.
    - 이 과정은 현재 노드에서 노드내의 간선 정보를 살핌으로서 이루어 진다.
    - 간선 그래프를 초기화 할 때에 그 정보를 받아 두었다.

2. 이전 상태의 거리와 비교한다. 더 가까워서 갱신이 가능하다면 갱신해준다.
    - 현재의 노드를 now라고 한다면 
    for k in graph[now]:
        cost=distance[now]+k[1]
        if cost<distance[k[0]] # 이전의 다른 노드를 거쳐서 간 것 보다 더 작으면
            distance 갱신!

3. 한번 2번의 과정을 한번 하고 나서 다음 노드를 선택하여 탐색해야한다.
    - 우선 현재 노드에서 연결되어 있어야 한다.
    - 연결된 것 중에서는 가장 거리가 짧은 노드를 선택
        - 그런데 이렇게 해야하는 이유가? 그렇게 해서 뭐가 좋은데?
        
            - 우선 지금까지 거쳐온 "경로 중에서는" 최단거리를 확정하는 행위인 것이다.
            추후 업데이트가 될 수도 있겠지만, 만약 다른 노드에 대해 더 탐색을 안한다고 가정할 수도 있는데
            그렇게 되면, 무조건 현재 정보를 기반으로 최단 경로를 확정해야만 한다.
            
            - 지금 상태에 있는 노드는 앞에서 다른 노드들을 거치면서 최소 cost를 갱신한 끝에 도달한 노드다.
            그러므로, 이 상태에서 가장 비용이 작은 다음 노드(Next라 하자)를 선택하는 것은, 곧 Next 노드의 최단경로
            비용이 확정되는 것을 의미한다.
            
4. 추가적으로 주의해야 할 점은, 노드마다 갱신은 하는데, 방문처리하는 노드는 하나인 것을 기억해야한다.
                   
'''

# 방문하지 않은 노드 중에서, 가장 최단 거리가 짧은 노드의 번호 반환

def get_smallest_node():
    min_value=INF
    index=0
    for i in range(1,n+1):
        if distance[i]<min_value and not visited[i]:
            min_value=distance[i]
            index=i
    return index


# 다익스트라 알고리즘
def dijkstra(start):
    # 시작노드에 대해 distance 초기화
    distance[start]=0
    visited[start]=True # 방문처리

    for j in graph[start]:
        distance[j[0]]=j[1] # 시작노드와 관련된 간선들에 대한 정보 초기화

    # 시작 노드를 제외한 전체 n-1개의 노드에 대해 반복
    for i in range(n-1):
        # 현재 최단 거리가 가장 짧은 노드를 꺼내어 방문처리함
        now=get_smallest_node()
        visited[now]=True

        # 현재 노드와 연결된 다른 노드 확인
        for j in graph[now]:
            # graph[now] 에는 현재 노드에 대해 타 노드의 간선정보가 들어있다.
            cost=distance[now]+j[1] # j[1]은 타 노드(j[0])에 대한 거리
            if cost<distance[j[0]]:
                distance[j[0]]=cost # j[0] 노드에 대한 최단거리 갱신


dijkstra(start)

for i in range(1,n+1):
    if distance[i]==INF:
        print("INFINITY")
    else:
        print(distance[i])
```
