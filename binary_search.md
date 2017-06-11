# Binary Seach
> Given an array $X$ sorted in ascending order, return the index of a queried element, or -1 if the element is not present in the array.

## Discussion
The key pattern to notice here is the sorted order. Sorted order implies a lot of equivalent things:

1. All elements are in order
2. The sequence is monotone according to some pre-defined order. 
3. Every $x_k$ to the right of $x_i$ shares its relation with $x_i$ with every $x_j$ to the left of $x_i$. Convoluted definition, but useful. i.e. $\forall\: k > i > j \: x_k \: R \: x_i \Longrightarrow\ x_i \: R \: x_j$. The converse also holds.

All of those three are the same idea. As with any problem begin with a guess at the solution.

Say the input array is: $X = \{x_1, x_2, ..., x_n\}$, and we queried for $q$. Notice that the array is sorted, so all the elements form a monotone sequence. So everything smaller than $q$ is to the left of $q$, and $q$ is smaller than everything to its right.

If $q$ is a valid query: $\exists\:x_i \in X$ s.t. $x_i = q$, else $\exists\:x_i, x_j \in X$ s.t. $x_i < q < x_j$.

So we can guess an element to begin with. 

* If our guess $x$ is larger than $q$, it means $x$ is to the right of $q$, effectively forming a right-boundary of sorts. We can discard everything to the right of $x$ because we know $q$ has to be in the part that is to the left of $x$.
* If our guess $x$ is smaller than $q$, it means $x$ is to the left of $q$, acting like a left boundary. We can discard everything left of this boundary since we know $q$ has to be on the right side of this boundary.
* Each iteration discards roughly half the current number of elements.
* The process goes on till the left and right boundary become one or are at adjecent elements. If we still don't find anything, it means that there was no such element!

## Code

```python
def binarySearch(ary, q, lBound=0, rBound=-1):
    # Initial guess. Can be anything, choose midpoint for simplicity
    if rBound == -1:
        rBound = len(q) - 1
    # Terminate if left and right boundaries merge or cross
    if lBound >= rBound:
        if ary[lBound] == q:
            return lBound
        else:
            return -1
    mid = (lBound + rBound) // 2
    if ary[mid] == q:
        return mid
    elif ary[mid] > q:
        # Move right boundary inwards
        return binarySearch(ary, q, lBound, mid - 1)
    else:
        # Move left boundary inwards
        return binarySearch(ary, q, mid + 1, rBound)
```


## References
1. CLRS Chapter 4
2. [Topcoder - Binary Search](https://www.topcoder.com/community/data-science/data-science-tutorials/binary-search/)
3. [LeetCode - Majority Element](https://leetcode.com/problems/majority-element/#/description)