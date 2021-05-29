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
### start with the transition function
for dp solutions, first think about what is the transition function? After getting the transition function, we already succeeded 50%.

### dp framwork for substring
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