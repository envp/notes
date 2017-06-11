
# Sorting
> Given an array $A$ of numbers, find a permutation of it so that the array's elements are monotonic in its indices.

## Discussion
Sorting is a classic algorithms problem that has been extensively explored by various authors some of whose approach to the problem is truly novel and path breaking.

Without digressing too much from the primary purpose of this essay (more of a personal reminder about concepts for interview prep, )

### Properties

## Algorithm

### Code

```cpp
#include <climits>

using namespace std;

class Solution 
{
    public:
    void mergeSort(vector<int>& nums, int p, int r)
    {
        if(p < r)
        {
            // Pivot index
            int q = (p + r) / 2;
            // Left half
            mergeSort(nums, p, q);
            // Right half
            mergeSort(nums, q + 1, r);
            // merge sorted arrays
            merge(nums, p, q, r);
        }
    }
    
    // Merges two sorted segments
    private:
    void merge(vector<int>& nums, int p, int q, int r)
    {
        int lsize = q - p + 1;
        int rsize = r - q;
        int lt = 0;
        int rt = 0;
        
        // Allocate temp storage for merging
        vector<int> ltemp(lsize + 1);
        vector<int> rtemp(rsize + 1);
        
        for(int i = 0; i < lsize; ++i)
        {
            ltemp[i] = nums[i + p];
        }
        
        for(int i = 0; i < rsize; ++i)
        {
            rtemp[i] = nums[i + q + 1];
        }
        
        // Poor man's infinity
        // Sentinel value so that 
        // bounds checking is simplified for us
        ltemp[lsize] = INT_MAX;
        rtemp[rsize] = INT_MAX;
        
        for(int i = p; i <= r; ++i)
        {
            if(ltemp[lt] <= rtemp[rt])
            {
                // Ties broken towards left
                nums[i] = ltemp[lt];
                lt += 1;
            }
            else
            {
                nums[i] = rtemp[rt];
                rt += 1;
            }
        }
    }
};
```

