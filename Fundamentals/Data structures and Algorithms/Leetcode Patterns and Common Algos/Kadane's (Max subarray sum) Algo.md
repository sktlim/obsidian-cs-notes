For **Maximum Subarray Sum**

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int curMax = 0, maxTillNow = INT_MIN;
        for(int& num : nums){
	        curMax = max(num, curMax + num),
            maxTillNow = max(maxTillNow, curMax);
        }
        return maxTillNow;
    }
};
```

