# 임의의 여러 줄 입력 받기

```
import sys
for line in sys.stdin:
     if line=="\n":
          break
     print(list(map(int, line.split())))
```
