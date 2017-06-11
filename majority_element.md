# Majority Element

>Given an array of size $n$, find the majority element. The majority element is the element that appears more than $\lfloor\frac{n}{2}\rfloor$ times. You may assume that the array is non-empty and the majority element always exist in the array.

## Discussion

The minimum time required to do this is a single pass. The lower bound is therefore $\Omega(n)$ since we need to know the most frequent element, and for that we need to go through all of the array.

If the array has more than $\lfloor\frac{n}{2}\rfloor$ of an element, that element is the majority element.

Another property to notice it is that the majority element can form pairs with every other element in the array and still have some count left over. We will use this property to develop a linear time, constant space algorithm to identify such an element given it exists.

We will go through 2 algorithms to address this problem, both linear time, but relying on different approaches.

### Algorithm 1 (Map based approach)

The idea here is to use an associative map (in the general case where we don't know the range of numbers that we will be dealing with) to store the counts of numbers we encounter. Alternatively, if we are aware of the range of numbers and are guranteed that the mapping is dense (more than n/2 numbers is likely to appear at least once) we can simply allocate space for an array to store all of these and treat the numbers as addresses themselves.

```cpp
#include <map>

using namespace std;

class Solution
{
    public:
    int majorityElement(vector<int>& nums)
    {
        map<int, int> elCount;
        int n = nums.size();
        for(int num : nums)
        {
            if(elCount.find(num) == elCount.end())
            {
                elCount[num] = 1;
            }
            else
            {
                elCount[num] += 1;
            }
        }
        // Scan through map to find which one was the largest
        for(const auto& pair : elCount)
        {
            if(2 * pair.second > n)
            {
                return pair.first;
            }
        }
        // There was no majority element
        return -1;
    }
}
``` 

### Algorithm 2 (Boyer-Moore)

This one is the Boyer-Moore majority vote algorithm. And works well in a streaming scenario, definitely preferred to the previous database like approach.



One pitfall of this algorithm is that if the sequence has no majority element, it will report one of the elements as an answer hence a second pass is required if the majority element is not guaranteed ot exist.

```cpp
using namespace std;

class Solution
{
    public:
    int majorityElement(vector<int>& nums)
    {
        int n = nums.size();
        // Assume nums[0] as candidate for majority element
        int maj;
        int count = 0;
        // Scan the **rest** of the array for updating the candidate
        for(int i = 1; i < n; ++i)
        {
            // Entries cancelled each other out so far, 
            // set a new majority element
            if(count == 0)
            {
                maj = nums[i];
                count = 1;
            }
            else
            {
                // Current element matches hypothesized candidate, keep going
                if(maj == nums[i])
                {
                    count += 1;
                }
                // Found an element not matching candidate, account for it
                else
                {
                    count -= 1;
                }
            }
        }
        // At this point we have determined a likely majority
        // element candidate, test it
        // Since the algorithm detects the majority element if it exists,
        // but cannot determine if it exists at all.
        int count = 0;
        // Scan through map to find which one was the largest
        for(const int& num : nums)
        {
            if(num == maj)
            {
                count += 1;
            }
        }
        // Return answer if it exists
        if(2 * count > n)
        {
            return maj;
        }
        // There was no majority element
        return -1;
    }
}

```



#### Analysis, Correctness:

The majority element is one that can form pairs with the rest of the array elements and still have some counts of itself left over.



**Suppose there is a majority element**, we have to show that the final count is positive and the final element selected is the majority element.



Since we increment the counter for every repetition and decrement the counter for every distinct element, the counter drops at most $\lfloor\frac{n}{2}\rfloor - 1$ times and it will increment at least $\lfloor\frac{n}{2}\rfloor$ times, due to there being a majority element. This establishes that the end count is indeed positive.



There are at most $\lfloor\frac{n}{2}\rfloor- 1$ re-assignments (staggered elements) in the algorithm corresponding to candidate elements which were not the majority. However due to the existence of a majority element, there would be at most $\lfloor\frac{n}{2}\rfloor$ cases where no reassignment would occur (clumped together). Therefore if we encounter $m$ the majority element in the array; this element would persist till the end due to fewer cases where the reassignment occurs than not. 



There is no guarantee on the answer if it does not exist, hence we choose to verify the solution before returning it.



## References

1. [LeetCode - Majority Element](https://leetcode.com/problems/majority-element/#/description)

2. [Boyer–Moore majority vote algorithm](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_majority_vote_algorithm)

3. [Notes on streaming algorithms](http://theory.stanford.edu/~trevisan/cs154-12/notestream.pdf)