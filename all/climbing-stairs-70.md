# 70. Climbing Stairs
https://leetcode-cn.com/problems/climbing-stairs/

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Example 1:
```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```
Example 2:
```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

# Solutions
### dp
time: O(n)
space: O(1)

dp transition function: `f(x) = f(x - 1) + f(x - 2)`.
```ts
// for stair x, both stair x - 2 and x - 1 can reach this stair in 1 step, so
// f(x) = f(x - 1) + f(x - 2)
var climbStairs = function(n) {
  // base case
  if (n === 1) return 1;
  if (n === 2) return 2;

  // dp case
  let prev2 = 1;    // # of ways to reach cur - 2 stairs
  let prev1 = 2;    // # of ways to reach cur - 1 stairs
  let cur;
  for (let i = 2; i < n; i++) {
    cur = prev1 + prev2;
    prev2 = prev1;
    prev1 = cur;
  }

  return cur;
};
```

### formula
time: O(1)
space: O(1)
since we know that `f(x) = f(x - 1) + f(x - 2)`, we can solve for `f(x) = ...`, then calculate it really fast. But this method is now that general since some formula might not be able to be solved easily.