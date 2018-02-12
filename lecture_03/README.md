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

### [Difficulties with Binary Search](https://en.wikipedia.org/wiki/Binary_search_algorithm#Implementation_issues)

Very difficult to implement effectively
* Issues can be due to overflow on datatypes for large arrays. ```(L + R) / 2``` will overflow
* Can avoid with ```L + (R - L) / 2```


## [Hashing](https://bradfieldcs.com/algos/searching/hashing/)

Goal of hashing is to allow for O(1) search (more in theory than in practice)

Hash tables have numbered slots.

*Hashing function*: the mapping between an item and the slot.

Example
```python
h(item = item % 11) # Or some other prime
```

*Load factor*: number of slots that are filled after passing data through the hashing function.
* Load factor = number of items / tablesize

Unfortunately, hashing functions often lead to *collisions*. When the hashing function puts multiple datapoints into the same slot.

Optimizing for perfect hashing functions
* Only possible if you know the items and collection will never change
* Could increase the size of the hash table (increase memory utilization) to improve performance
* Number of methods to optimize, Ex. Folding Method, Mid-square method

*Folding Method*
* Dividing the item into equal sized pieces (except the last piece)
* Add up the pieces to give the hash value
* Dividing / modulo'ing by the number of slots available

*Mid-square Method*
* Square the item, extract some portion of the resulting digits
* Ex. 44 --> 44^2 = 1,936 --> extract middle two digits (93) --> 93 % 11 --> 5

*Some string-based methods*
* "cat" --> ord('c') = 99... 'a' = 97, 't' = 116 --> 99 + 97+ 116 = 312 --> 312 % 11 = 4

```python
def hash(astring, tablesize):
    the_sum = sum(ord(char) for char in astring)
    return the_sum % tablesize
```

In the above example, anagrams will always get the same value so you can weight each character based on its position in the string.

Ex. 99*1 + 97*2 + 116*3 = 641 % 11 = 3

### Collision Resolution

Dealing with multiple pieces of data in each slot.

*Open addressing*
* Finding the next open slot (*rehashing*)... often through *linear probing*
* Use the same method for searching items
* Problem because of *clustering*, when many collisions occur at the same hash value
* To avoid clustering you can skip slots

```
newhashvalue = rehash(oldhashvalue)
rehash(pos) = (pos + 1) % sizeoftable
rehash(pos) = (pos + skip) % sizeoftable
```

* *Quadratic probing*, instead of using a constant skip value you increment by squares of an incrementer
* *Chaining*, linking collisions... however the problem is that searching gets more complicated the more collisions there

### Analysis of Hashing

The most conceptually important part of analyzing hash functions is the load factor.

# Sorting

## O(n^2) in average cases
1) Selection Sort
2) Bubble Sort
3) Insertion Sort

## Merge Sort

[Mycodeschool Video](https://www.youtube.com/watch?v=TzeBrDU-JaY)
* O(nlogn) in worst case (time)
1) First, break array A into two halves (L and R)
2) Second, have incrementers for each array: k, i, j respectively for A, L, R
3) Third, have nL and NR be the lengths of L and R

```
# will work through R & L until one is done
while(i < nL && j < nR)
{
	if(L[i] <= R[j])
	{
		A[k] <- L[i]
		i++
	}
	else
	{
		A[k] <- R[j]
		j++
	}
	k++
}
# next two while loops will iterate through the remaining numbers in either R or L (only one will execute)
while(i < nL)
{
	A[k] <- L[i]
	i++
	k++
}
while(j < nR)
{
	A[k] <- R[j]
	j++
	k++
}
# will result in a sorted array, A
# can also break down sub-lists into further sub-lists until sub-list has 1 element (because its already sorted)
# can handle this recursively...

mergesort(A)
{
n <- length(A)
if(n < 2)
	return # array is sorted, termination condition
mid <- n / 2
left <- array of size(mid)
right <- array of size(n - mid)



}

```

## Quick Sort
