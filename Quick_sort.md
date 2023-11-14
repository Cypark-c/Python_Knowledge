# Quicksort

## Quicksort의 동작 원리

- 아래와 같은 배열을 정렬한다고 하자
- 퀵소트는 피벗을 이용하여 Average 하게 O(NlogN)의 시간복잡도를 구현하고자 하는 정렬 알고리즘이다.
- 피벗을 ★로 표시하고, left,right를 화살표로 나타낸다
___
```
[5,7,9,0,3,1,6,2,4,6]
★
[5,7,9,0,3,1,6,2,4,6]
   ↑             ↑(right)
[5,4,9,0,3,1,6,2,7,6]

[5,4,9,0,3,1,6,2,7,6]
     ↑         ↑(right)
[5,4,2,0,3,1,6,9,7,6] 
       ↑   ↑(right)
[5,4,2,0,3,1,6,9,7,6] 
         ↑ ↑(right)
[5,4,2,0,3,1,6,9,7,6] 
★         ↑ ↑(left)
        (right)
```
- <b>이제 Pivot과 right를 가리키는 인덱스가 서로 교환되면</b>
```
[1,4,2,0,3,5,6,9,7,6]
          ★
```
____
5의 양쪽은 모두 5보다 작으며, 5의 오른쪽은 모두 5보다 크다
이것을 ★기준으로 분할된 리스트에서 반복한다 즉, 퀵소트는 재귀적으로 동작한다.
___

### Quicksort의 단점

예를 들어서
[1,2,3,4,5,6,7,8,9] 이런 배열이라고 하면
left는 피벗보다 같거나 큰수를 찾는 것으로 설정하면, left는 바로 찾아지는데
right를 피벗보다 같거나 작은수를 찾는 것으로 설정을 해야하므로 right는 계속 진행을 하다가 1까지 도달하게 된다.
이 때서야 right<left 상황이 발생하게 된다. 결국 리스트를 전부 탐색한거나 다름이 없는데 매번 left, right를 찾을 때마다 이와 같이 배열을 전부 탐색해야한다면, 시간복잡도는 O(N**2) (worst case)

## Quicksort Python code

### normal version
```

```


### Easy version
```
a=[4,6,3,5,7,8,2,1]

def qsort(arr):
    if len(arr)<=1:
        return arr

    pivot=arr.pop() # 피벗
    left=[x for x in arr if x<pivot]
    right=[x for x in arr if x> pivot]
    return qsort(left)+[pivot]+qsort(right)

print(qsort(a))
```

## 참고문제
- BOJ 1181번 [[https://www.acmicpc.net/problem/1181]]
