# Tricks
This is a summary of some common tricks that can be used to solve a problem.

## Sliding Window / Two Pointers

# Learned

## start with brute force
brute force version is usually a great start for thinking about more optimal solution, since brote force will give us some sense of what a basic and essential unit of this porblem looks like. For example, in 42-trapping-rain-water, if we don't know the brute force, we won't know that for each element, the rain it can trap is
```py
min(max(left bars), max(right bars)) - it's own height
```
This is essential for other solutions. If we start thinking about optimal ones without knowthing this basic unit, it would be really hard.
**Optimal solution is usually built from the ground (basic solution), not from the air.**

## sliding window framework
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