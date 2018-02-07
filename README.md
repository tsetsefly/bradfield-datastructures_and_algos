# [/algos](https://bradfieldcs.com/algos/analysis/introduction/)

## [Introduction](https://bradfieldcs.com/algos/analysis/introduction/)

### The Big Picture

"Algorithm analysis is a way to compare the time and space efficiency of programs with respect to their possible inputs, but irrespective of other context."

```python
def sum_to_n(n):
    total = 0
    for i in range(n + 1):
        total += i
    return total
```

Will ```sum_to_n``` take longer to run given a larger n? Intuitively, the answer seems to be yes, as it will loop more times.

Will ```sum_to_n``` take the same amount of time to run each time it’s invoked with the same input? Intuitively the answer seems to be yes, since the same instructions are executed.

```python
import time

def sum_to_n(n):
    # record start time
    start = time.time()

    # run the function's code
    total = 0
    for i in range(n + 1):
        total += i

    # record end time
    end = time.time()

    return total, end - start
```


