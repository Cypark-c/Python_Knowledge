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
