# 746. Min Cost Climbing Stairs
You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index 0, or the step with index 1.

Return the minimum cost to reach the top of the floor.

Example 1:
```
Input: cost = [10,15,20]
Output: 15
Explanation: Cheapest is: start on cost[1], pay that cost, and go to the top.
```
Example 2:
```
Input: cost = [1,100,1,1,1,100,1,1,100,1]
Output: 6
Explanation: Cheapest is: start on cost[0], and only step on 1s, skipping cost[3].
```

Constraints:
```
2 <= cost.length <= 1000
0 <= cost[i] <= 999
```

# Solutions
## DP
time: O(n)  
space: O(1) 
 
Note that the definition of `top` is: when array length is n, top is reaching n + 1.
So we finaly min cost is the min cost of reaching n or n -1, which can then reach n + 1 in one step without extra cost.

**DP transition function**: f(x) = min(f(x - 1), f(x - 2)) + cost(x)

```ts
var minCostClimbingStairs = function(cost) {
    let prev2 = cost[0];
    let prev1 = cost[1];
    let cur;

    for (let i = 2; i < cost.length; i++) {
        cur = Math.min(prev1, prev2) + cost[i];
        prev2 = prev1;
        prev1 = cur;
    }

    // we can end in either last 1 or 2 stairs, and go to top.
    return Math.min(prev1, prev2);
};
```
