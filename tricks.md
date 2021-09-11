# Tricks
This is a summary of some common tricks that can be used to solve a problem.

## General Ideas
### start with brute force
brute force version is usually a great start for thinking about more optimal solution, since brote force will give us some sense of what a basic and essential unit of this porblem looks like. For example, in 42-trapping-rain-water, if we don't know the brute force, we won't know that for each element, the rain it can trap is
```py
min(max(left bars), max(right bars)) - it's own height
```
This is essential for other solutions. If we start thinking about optimal ones without knowthing this basic unit, it would be really hard.
**Optimal solution is usually built from the ground (basic solution), not from the air.**

## Sliding Window / Two Pointers
### sliding window framework
solutions/3.-Longest-Substring-Without-Repeating-Characters.md
solutions/340-Longest-Substring-with-At-Most-K-Distinct-Characters.md

One general solution for sliding window to find longest valid substring:
```py
longest = 0
l = 0

for r in range(n):
  cur = string[r]
  # move left pointer to make sure substring is valid if we add cur

  longest = max(longest, r - l + 1)

return longest

```

## DP
### idea
DP is like a advanced version of recursion, and it optimized recursion process by saving result of duplicated computing in recursions.

For example, if we define f(x) = f(x - 1) + f(x - 2) + ... + f(x - 10), by doing recursion, f(x - 1) needs to compute f(x - 2), ..., f(x - 10) again.

By doing DP, every f(x) is only computed once, and the result was saved for looking.

Also it saves more memory than recursion, recursion is like `top => bottom => top`, which requires many levels of stacks. DP replaces the `top => bottom` process by "thinking", then implement the second half `bottom => top` directly.
### general framework
start with the transition function:
1) define what is `f(x)`
2) find transition function `f(x) = ...`
  
After getting the transition function, we already succeeded 50%.

### framwork for substring
a n * n matrix. For example, when string is `abcd`, we should generate something like

|   | a | b | c | d |
|---|---|---|---|---|
| a |   |   |   |   |
| b |   |   |   |   |
| c |   |   |   |   |
| d |   |   |   |   |

and then fill in the blank.

### 滚动数组优化
- 2d array
  when filling in an array from left to right, usually only need to keep track of a fixed size sliding window of this array. 

- 3d matrix
  when dilling in a matrix from left to right, usually only need to keep track of a fixed size sliding window of a couple arrays.

## Binary Search
### farmwork
之前犯了一个错误，就是太meta programming的思想，总觉得不管需要找什么，只要算法够精细，那个target index永远会等于`l`或者`r`或者`mid`。其实这样是没有必要的，在复杂的场景中，可以稍微hardcode一点, keep track of another variable that reflects our target index.

basic framework
```ts
// when l > r means we already searched everything
while(l <= r) {
  mid = parseInt((l + r) / 2);
  if (cur <= nums[mid]) {
    r = mid - 1;
  } else {
    l = mid + 1;
  }
}
```

advanced framework that needs to keep track of something else
```ts
let targetIndex = r;   
while(l <= r) {
  mid = parseInt((l + r) / 2);
  if (cur <= nums[mid]) {
     // at certain circumstances, calculate and update targetIndex
    targetIndex = f(l, r, mid);   
    r = mid - 1;
  } else {
    l = mid + 1;
  }
}
```