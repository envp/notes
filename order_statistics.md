
# Order Statistics


```cpp
#include <algorithm>

using namespace std;

class Solution 
{
    public:
    int findKthLargest(vector<int>& nums, int k) 
    {
        // Sort array
        std::sort(nums.begin(), nums.end());
        
        // K-th largest = index |A| - k
        return *(nums.end() - k);
    }
};
```