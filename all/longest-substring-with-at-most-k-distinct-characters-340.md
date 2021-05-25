# 340. Longest Substring with At Most K Distinct Characters
https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/

Given a string, find the length of the longest substring T that contains at most k distinct characters.

## Example 1:
Input: s = "eceba", k = 2\
Output: 3\
Explanation: T is "ece" which its length is 3.

## Example 2:
Input: s = "aa", k = 1\
Output: 2\
Explanation: T is "aa" which its length is 2.


# Solutions
Time: O(2n) = O(n)\  
Space: O(k)

Idea is to use two pointers to maintain a sliding window which is valid (at most k distinct chars). At each step when right pointer `r` changes, we calculate the maximun valid subarray up until `r`.

```py
from collections import defaultdict
def lengthOfLongestSubstringKDistinct(self, s, k):
  n = len(s)
  l = 0
  r = 0
  chars = defaultdict(int)
  longest = 0

  for r in range(n):
    cur = s[r]
    chars[cur] += 1

    while (len(chars) == k + 1):
      leftChar = s[l]
      chars[leftChar] -= 1
      if chars[leftChar] == 0:
        del chars[leftChar]
      l += 1

    longest = max(longest, r - l + 1)

  return longest
``` 

We can further improve the running time by recording the rightmost index of each seen chars, so when the subarray is not valid, we can delete the earliest inserted element of current subarray, and jump left pointer directly, instead of moving left pointer to right one position at a time.

How to know the first inserted char? Trick is to use orderedDict so that it can pop the last or first inserted element with O(1) time.

```py
from collections import OrderedDict
def lengthOfLongestSubstringKDistinct(self, s, k):
  n = len(s)
  l = 0
  r = 0
  cToi = OrderedDict()  # last position of a char
  longest = 0

  for r in range(n):
    cur = s[r]

    # if cur already seen
    # delete and re-insert to update the order (make this char last inserted)
    if cur in cToi:
      del cToi[cur]   
    cToi[cur] = r

    if (len(cToi) == k + 1):
      c, index2Delete = cToi.popitem(last = False)
      l = index2Delete + 1

    longest = max(longest, r - l + 1)

  return longest
```

## JS
```ts
class DefaultDict {
  constructor(defaultVal) {
    return new Proxy({}, {
      get: (target, name) => name in target ? target[name] : defaultVal
    })
  }
}

var lengthOfLongestSubstringKDistinct = function(s, k) {
  const n = s.length;
  let l = 0;
  let longest = 0;
  let chars = new DefaultDict(0);
  
  for (let r = 0; r < n; r++) {
    let cur = s[r];
    chars[cur] += 1;
    
    while (Object.keys(chars).length === k + 1) {
      let leftChar = s[l];
      chars[leftChar] -= 1;
      l++;
      if (chars[leftChar] === 0) {
        delete chars[leftChar];
      }
    }
    
    longest = Math.max(longest, r - l + 1);
  }
  
  return longest;
};
```