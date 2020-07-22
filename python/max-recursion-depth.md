---
description: >-
  The recursion depth in Python is way less for solving some of the algorithmic
  problems related to recursions. We might want to increase the recussion limit
  manually before executing recursion problems
---

# Max Recursion Depth

 On Python 3

```text
Python 3.6.9 (default, Apr 18 2020, 01:56:04) 
[GCC 8.4.0] on linux
>>> sys.getrecursionlimit()
1000
```

To increase the limit

```text
sys.setrecursionlimit(10**6) 
```

