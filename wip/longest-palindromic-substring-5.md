# 5. Longest Palindromic Substring
https://leetcode-cn.com/problems/longest-palindromic-substring

Given a string s, return the longest palindromic substring in s.

### Examples
Example 1:
```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```
Example 2:
```
Input: s = "cbbd"
Output: "bb"
```
Example 3:

```
Input: s = "a"
Output: "a"
```
Example 4:

```
Input: s = "ac"
Output: "a"
```

# Solutions
first we need to know that palindrom can be either even length ('abba') or odd length ('aba').

### brute force
time: O(n^3)
space: O(1)

check all substring: total O(n^2) substring, to check if it's a palindrom, O(n), so total O(n^3)

### expand from center
time: O(n^2)
space: O(n)

in order to reduce palindrom checking to O(1), we can check xxx, if xxx is a palindrom, then check axxxb, if a === b, then this is a palindrome, then keep checking...

so we can reduce the total time to O(n^2).

```ts
var longestPalindrome = function(s) {
  let longest = '';
  // this loop can be further optimized:
  // for from center first, if curLongest already >= maximum possible of unchecked center, return directly 
  for (let i = 0; i < s.length; i += 0.5) {
    const curLongest = findSubLongest(s, i);
    if (curLongest.length > longest.length) {
      longest = curLongest;
    }
  }

  return longest;
};

/* -----
  find longest from center i
  when i === x.0: odd length palindrome
  when i === x.5: even length palindrome
                                    ----- */
const findSubLongest = (s, i) => {
  let l = Math.floor(i);
  let r = Math.ceil(i);

  while(l >= 0 && r < s.length && s[l] === s[r]) {
    --l;
    ++r;
  }
  const subLongest = s.slice(l + 1, r);
  
  return subLongest;
};
```

### dp
classical dp solution for substrings is using a n * n matrix.
  d a b a c
d 1
a 
b
a
c