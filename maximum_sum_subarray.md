# Maximum Sum Subarray
> Given an array of size $n$, find a contiguous subsequence (a subarray) with the maximum sum. The subarray should contain at least 1 element.


## Discussion
This problem can be solved in many ways with varied time complexities, shown in the below list.

1. Brute force search (compute all partial sums and brute-force look for a pair that maximizes difference)
2. Divide and conquer $O(n \log{n})$ [1]
3. Kadane's algorithm $O(n)$ in time [2]
4. Dynamic programming $O(n)$ in time [3]

We will be focusing our attention on 3 and 4. Since it is easy to derive these from scratch once we understand some properties of the Maximum Sum Sub-array(MSS).

### Properties
Whenever dealing with a problem which asks you to look for a kind of optimum (max / min some set of properties) it is a good idea to try to come up with a solution that would violate this property, this will usually be a good first step to discovering interesting properties that may help in algorithm development.

So, some questions to answer:
**Does the MSS have to be positive?***
No, if the array is all negative, the MSS will fall back to the least negative element.

**Can the MSS contain negative elements?**
Yes. An example would be $MSS(-1, -1, -1, 1, 1, -1, 1, 1, 1, -1, -1) = (1, 1, -1, 1, 1, 1)$

**Why does the solution not include the preceding / following $k$ elements?**
Assume we have a correct algorithm. Given an array $A$, if it tells us that the MSS is $A[i:j]$ means that including any preceding / following elements will only reduce the sum. This results in the following rules:
1. $\forall k < i: sum(A[k:j]) \leq sum(A[i:j]) \implies sum(A[k:i-1]) \leq 0$
2. $\forall j < k: sum(A[i:k]) \leq sum(A[i:j])\implies sum(A[j+1:k]) \leq 0$
So all the elements prior and following the MSS, have a non positive sum. Which is a useful flag to look for, suppose we are scanning through the array and tracking the partial sums, if our running sum falls to zero it means we are approaching the boundary of an MSS or that we have crossed one.


## Algorithm: Kadane's Algorithm
Kadane's algorithm relies exclusively on the above discussed properties and follows intuitively once you understand how to use $\#3$ to aid your search.

We run through the array keeping a track of the running sum and the maximum seen so far. If the running sum exceeds the maximum seen so far, update the maximum sum. However, if the running sum falls below zero, reset it to zero to indicate that the current iteration caused us to *change the left endpoint of our MSS*. The right endpoint of the MSS *changes whenever we encounter a new maximum*.

### Code
```cpp
class Solution
{
    public:
    int maxSubArray(vector<int>& nums)
    {
        int sum = 0;
        int maxSoFar= nums[0];
        for(int i = 0; i < nums.size(); ++i)
        {
            sum += nums[i];
            // Update MSS end whenever maxSoFar gets updated
            maxSoFar= max(maxSoFar, sum);
            // Negative sum causes us to update the MSS starting point
            sum = max(sum, 0);
        }
        return maxSum;
    }
};
```


## Algorithm: Dynamic Programming
With dynamic programming the primary and most important task is to identify what we have to optimize. Usually it helps to write it out in a general form and discard parameters that do not add any kind of "real freedom" to the optimization procedure.

To state the MSS as an generic optimization problem for an array $A[1:n]$ numbers:

$$\max_{1 \leq i \leq j \leq n}{sum(A[i:j])}$$

This promises a quadratic solution, not really an improvement over what we could do we Kadane's algorithm.

While this captures the essence of the problem at hand, we can show that one of the indices isn't really necessary, Why? Assume we have a solution $A[i:j]$. This is also the MSS of $A[1:j]$ So if our array does have an MSS, the problem can be reduced to:

$$\max_{1 \leq j \leq n}{sum(A[1:j])}$$

This removes the unnecessary complexity of the first index while preserving the solution.

Let $m[j] = \max_{1 \leq j \leq n}{sum(A[1:j])}$

$$
m[j] = \max(m[j - 1], \max(m[j - 1] + A[j], A[j]))
$$

The equation can be read as:

While scanning upwards through the array, either the maximum can change as follows:

1. Remains the same $\rightarrow$ Do nothing
2. Increases by absorbing current element, $\rightarrow$ Update sum
3. Decreases by absorbing the next element
	4. If the reduced sum is smaller than the current element, it means our sum is negative $\rightarrow$ Reset the sum to be the current element only.
	5. Otherwise, do nothing.

### Code
```cpp
class Solution
{
    public:
    int maxSubArray(vector<int>& nums)
    {
        int maxSoFar= nums[0];
        int currentPeak = INT_MIN;
        for(int i = 0; i < nums.size(); ++i)
        {
	        currentPeak = max(currentPeak + nums[i], nums[i]);
			maxSoFar = max(maxSoFar, currentPeak);
        }
        return maxSum;
    }
};
```

## References
1. CLRS chapter 4, divide and conquer
2. [Programming ASAP - Maximum Sum Subarray](https://programmingasap.wordpress.com/2015/04/22/the-maximum-subarray/)
3. [GeeksforGeeks - Largest Sum Contiguous Subarray](http://www.geeksforgeeks.org/largest-sum-contiguous-subarray/)