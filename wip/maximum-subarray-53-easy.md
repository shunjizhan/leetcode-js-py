# 53. Maximum Subarray

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example 1:
```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```
Example 2:
```
Input: nums = [1]
Output: 1
```
Example 3:
```
Input: nums = [5,4,-1,7,8]
Output: 23
```

Constraints:
```
1 <= nums.length <= 3 * 104
-105 <= nums[i] <= 105
```

# Solutions
## brute force
time: O(n^2)  
space: O(n)  

simply check all subarrays.

## dp
time: O(n)
space: O(1)

define `f(x)` to be the maximun subarray using nums[x] as the subarray end. Then `globaMax` is the maximun of all `f(x)`.

**transition function**:
f(x) = nums[x] + max(f(x - 1), 0)  
globMax = max(f(x)) for all x

```ts
var maxSubArray = function(nums) {
    let globMax = nums[0];
    let curMax = globMax;         // max using cur num as substring end
    for (let i = 1; i < nums.length; i++) {
        curMax = nums[i] + Math.max(0, curMax);
        globMax = Math.max(globMax, curMax);
    }

    return globMax;
};
```