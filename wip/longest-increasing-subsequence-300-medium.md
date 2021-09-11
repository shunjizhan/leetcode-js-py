# 300. Longest Increasing Subsequence
https://leetcode-cn.com/problems/longest-increasing-subsequence/

Given an integer array nums, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].

Example 1:
```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```
Example 2:
```
Input: nums = [0,1,0,3,2,3]
Output: 4
```
Example 3:
```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

Constraints:
```
1 <= nums.length <= 2500
-104 <= nums[i] <= 104
```

# Solutions
## brute force
time: O(2^2)
space: O(n)

brute force need to check all possible **subsequence**, not **substrings**. So there are 2^n possibilities. 

## dp
time: O(n^2)  
space: O(n)  

**dp transition function:**  f(x) = max(f(y)) + 1 for all (y < x &&  f(y) < f(x))

dp[i] means longest increasing subsequence using nums[i] as end
why we shouldn't denote global max LIS up until nums[i]?
since we care about *subsequence*, not *substring*, so we sill need to refer back for
information about the middle, not only the end of a substring.
```ts
var lengthOfLIS = function(nums) {
  let gmax = 1;
  const dp = new Array(nums.length).fill(1);

  for (let i = 1; i < nums.length; i++) {
    let curMax = 1;
    for (let j = 0; j < i; j++) {
      if (nums[i] > nums[j]) {
        const curLen = dp[j] + 1;
        curMax = Math.max(curMax, curLen);
      }
    }
    
    dp[i] = curMax;
    gmax = Math.max(gmax, curMax);
  }

  return gmax;
};
```

## greedy
time: O(nlogn)  
space: O(n)  

idea: in each iteration, find the current optimization.

in this specific case, we can see that:
[1, 2] is strictly better than [1, 3]

so we can keep track of an array `lastDigits` of the last digits of subsequence of length i, where `lastDigits[i]` is the smallest possible number for an increasing subsequence of length i.

then for each x, greedyly update `lastDigits` replacing a IS's last digit by this x, using binary search.

```ts
var lengthOfLIS = function(nums) {
  const lastDigits = [nums[0]];

  for (let i = 1; i < nums.length; i++) {
    const cur = nums[i];
    if (cur > lastDigits[lastDigits.length - 1]) {
      lastDigits.push(cur);
    } else {
      // binary search to find the best place to put cur
      // which is the smallest number in lastDigits that >= cur
      let l = 0;
      let r = lastDigits.length - 1;
      let mid;

       // 不要总想着general的解法，想着最后刚好insertIndex = r | l | mid whatever, 可以稍微hardcode一点，一直更新inserIndex
      let insertIndex = r;   
      while(l <= r) {
        mid = parseInt((l + r) / 2);
        if (cur <= lastDigits[mid]) {
          insertIndex = mid;
          r = mid - 1;
        } else {
          l = mid + 1;
        }
      }

      // now insertIndex is the smallest index that lastDigits[insertIndex] >= cur
      lastDigits[insertIndex] = cur;
    }
  }

  return lastDigits.length;
};
```