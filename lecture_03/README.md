# [Search](https://bradfieldcs.com/algos/searching/)

## [Searching](https://bradfieldcs.com/algos/searching/searching/)

Basic way in Python to search:

```python
15 in [3, 5, 2, 4, 1] # => False
3 in [3, 5, 2, 4, 1] # => True
```

## [Sequential Search](https://bradfieldcs.com/algos/searching/the-sequential-search/)

Simply move from item-to-item.

```python
def sequential_search(alist, item):
    position = 0

    while position < len(alist):
        if alist[position] == item:
            return True
        position = position + 1

    return False

testlist = [1, 2, 32, 8, 17, 19, 42, 13, 0]

print sequential_search(testlist, 3)  # => False
print sequential_search(testlist, 13)  # => True
```

### Analysis of Sequential Search

What if the list is *unordered*?
Best Case vs. Worst Case vs. Average Case?
Item present vs. Item not present?

What if the list is *ordered*?
Best Case vs. Worst Case vs. Average Case?
Item present vs. Item not present?


```python
def ordered_sequential_search(alist, item):
    position = 0

    while position < len(alist):
        if alist[position] == item:
            return True

        if alist[position] > item:
            return False

        position = position + 1

    return False

testlist = [0, 1, 2, 8, 13, 17, 19, 32, 42,]
ordered_sequential_search(testlist, 3)  # => False
ordered_sequential_search(testlist, 13)  # => True
```

## [Binary Search](https://bradfieldcs.com/algos/searching/the-binary-search/)

Finding things from an ordered list.

Divide and conquer:
* Divide the problem into smaller pieces, solve the smaller pieces, reassemble

```python
def binary_search(alist, item):
	if not alist: # list is empty -- our base case
		return False

	midpoint = len(alist) // 2
	if alist[midpoint] == item: # found it!
		return True

	if item < alist[midpoint]: # item is the first half, if at all
		return binary_search(alist[:midpoint], item)

	# otherwise item is in teh second half, if at all
	return binary_search(alist[midpoint + 1:], item)

testlist = [0, 1, 2, 8, 13, 17, 19, 32, 42]
binary_search(testlist, 3) # => False
binary_search(testlist, 13) # => True
```

### Analysis of Binary Search

Number of items left = n / 2^i (i is number of comparisons)

The number of comparisons necessary to get to this point is i where n / 2^i = 1

This solves to i = log(n) ... binary search is O(log(n))

```python
binary_search(alist[:midpoint], item)
```

Above uses the slice operator to create the left half of the list. The slice operation is O(k).

Binary search assumes sorting so for low n, sequential search might be better.

## [Hashing](https://bradfieldcs.com/algos/searching/hashing/)

