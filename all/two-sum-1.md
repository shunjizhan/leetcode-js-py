# 1 - Two Sum
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

### Example 1:
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].

### Example 2:
Input: nums = [3,2,4], target = 6
Output: [1,2]

### Example 3:
Input: nums = [3,3], target = 6
Output: [0,1]
 
### Constraints:
2 <= nums.length <= 105
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.


# Solutions
Time: O(n)
Space: O(n)

### Python
```py
def twoSum(self, nums, target):
  numToIdx = {}

  for i in range(len(nums)):
    cur = nums[i]
    diff = target - cur

    if (diff in numToIdx):
      return [i, numToIdx[diff]]
    else:
      numToIdx[cur] = i

  return [-1, -1]
```

### JS
```ts
var twoSum = function(nums, target) {
  const numToIdx = {};

  for (let i = 0; i < nums.length; i++) {
    const cur = nums[i];
    const diff = target - cur;

    if (diff in numToIdx) {
      return [i, numToIdx[diff]];
    } else {
      numToIdx[cur] = i;
    }
  }
  
  return [-1, -1];
};
```

### TS
```ts
function twoSum(nums: number[], target: number): number[] {
  const numToIdx: {
    [key: number]: number,
  } = {};

  for (let i: number = 0; i < nums.length; i++) {
    const cur: number = nums[i];
    const diff: number = target - cur;

    if (diff in numToIdx) {
      return [i, numToIdx[diff]];
    } else {
      numToIdx[cur] = i;
    }
  }

  return [-1, -1];
};
```

