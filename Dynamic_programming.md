# Fibo: Recursive

```
from time import time
def fibo(x):
    if x==1 or x==2:
        return 1
    else:
        return fibo(x-1)+fibo(x-2)

start=time()
print(fibo(40))
end=time()
print(end-start)
```
```
102334155
17.56693172454834
```
- 시간이 17초가 넘게 걸렸음.
- 피보나치 수열은 빅오 표기법으로 $O(2^N)$ 연산 수행이 필요함
- $2^{30}$정도면 $2^{10}$ 정도가 대략 1000번 연산이니 $10^9$ 즉, 10억번 가량의 연산을 해야함.
- 이런 구조는 매번 함수에서 return 문을 출력 할 때 마다 매번 새로 함수를 실행시켜서 f(1) f(2) 까지 접근해야해서 그렇다.
  - 이런 이유로 Dynamic programming이 등장함

# Dynamic Programming
- 다이나믹 프로그래밍은 아래 두 조건을 만족해야함
  - 큰 문제를 작은 문제로 나눌 수 있음.
  - 작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에서도 동일함.
    - 피보나치 수열이 대표적인 케이스

- 하향식(Top-Down), 상향식(Bottom-Up) 방식 두 가지가 존재
- 분할정복과 다른점은 부분문제가 서로 중복되지 않고, Memozation 기법도 사용할 이유가 없다는 것.

## 하향식 다이나믹 프로그래밍(Memoization)
```
from time import time
def fibo(x):
    if x==1 or x==2:
        return 1
    if d[x]!=0: # 이미 계산을 한적이 있다면
        return d[x]

    d[x]=fibo(x-1)+fibo(x-2)
    return d[x]

d=[0]*100

start=time()
print(fibo(40))
end=time()
print(end-start)
```
```
102334155
0.0
```
- 보는 것과 같이 값을 리스트에 저장해 두면, 저장해 온걸 불러서 쓰면 된다.
- 큰 문제를 해결하기 위해, 작은 문제를 호출함.

<b>메모이제이션(Memoization): Cashing</b>

- <b>다이나믹 프로그래밍을 구현하는 방법 중 한 종류</b>
- 한 번 구한 결과를 메모리 공간에 메모를 해두고 같은 식을 다시 호출하면, 메모리 결과를 그대로 가져옴.
- <b>Cache the results of function calls and return the cached result if the function is called again with the same inputs</b>
  - 메모이제이션이라고 말하려면 function call을 배열에 저장을 해야함.
- fibo를 예로 들면 fibo(n)→fibo(n-1)→fibo(n-2)...→[fibo(0) & fibo(1)] 여기까지 내려간 다음에 여기서 부터 function call을 하나씩 저장해 나감. 
  - [Call Stack, Call Stack, Call Stack, Call Stack,........] ;  이게 너무 많아지면 irritating!


## 상향식 다이나믹 프로그래밍(Tabulation)

```
from time import time
def fibo(x):
    d[1]=d[2]=1
    for i in range(2,x+1):
        d[i]=d[i-1]+d[i-2]
    return d[x]

d=[0]*100

start=time()
print(fibo(40))
end=time()
print(end-start)
```
- Memoization과 다르게 function call의 결과를 저장하는게 아니라, subproblem의 결과를 table에 저장
- 반복문을 사용함

  
# 정리

- 당연하겠지만, 처음에 Memoization은 Function call 하는 과정에서 초기 state까지 쭉쭉 내려가게 되어있고, 이 과정에서 보통은 비효율이 발생함.
- memoization을 큰 수에 적용하면 call stack이 많아져서 느려짐! (재귀함수는 기본적으로 stack이고 메모이제이션 또한, recursive call의 수는 줄여주지만, recursive 구조인 것은 맞다.)
- Bottom UP 방식의 단점을 굳이 말하면, 순차적인 계산을 무조건 하기 때문에, 필요없는 메모리 공간을 사용할 수도 있다는 것

# Reference

- [[https://www.geeksforgeeks.org/tabulation-vs-memoization/]]
- 이것이 코딩테스트다 with 파이썬
