# append 메서드는 None을 리턴한다.

```
number_lt=[1,2]
cardlist=[11]
new_result=number_lt.append(cardlist[0])
print(new_result) #진짜 none 떠버림
new_result=number_lt
print(new_result) # OMG
```

```
None
[1, 2, 11]
```
