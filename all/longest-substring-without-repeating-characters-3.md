# 3. Longest Substring Without Repeating Characters
Given a string s, find the length of the longest substring without repeating characters.

### Example 1
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

### Example 2
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

### Example 3
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

### Example 4
Input: s = ""
Output: 0

# Solutions

## brute force
Time: O(n^3)
Space: O(n)

Basically iterate all substrings O(n^2), and check if no duplicate chars in it O(n)

## two pointer
Time: O(n)
Space: O(n)

Idea is to maintain a sliding window which has no duplicate chars. Suppose left pointer is at `x`, we Keep expanding the right boundary to find the max possible substring until we hit a repeat char, so this is the max substring from index `x`. Then we move left pointer to the right , until there is no repeat (suppose moved `a` positions), now the window is valid again. We can then repeat the above process to find max substring from index `x + a`, ...

```py
def lengthOfLongestSubstring(self, s):
  n = len(s)
  l = 0
  r = 0
  curMax = 0
  curChars = set()
  
  while ((r < n) and (l < n - curMax)):
    c = s[r]
    if c not in curChars:
      # expand right boundary
      curChars.add(c)
      r += 1
    else:
      # remove from the left
      curMax = max(curMax, r - l)   # not r - l + 1 since length not includes r
      curChars.remove(s[l])
      l += 1
  
  curMax = max(curMax, r - l)

  return curMax
```

Another way to further make it faster is, instead of saving the chars, we also save their positions using a map, so we can jump to the index directly. Note that we don't clean previously saved chars, so when `c in cToi`, we don't know if `c` is in current substring, or is it in previous string. So instead of `l = cToi[c] + 1`, we need to do `l = max(l, cToi[c] + 1)` to make sure only jump forward (only jump when `c` is in current substring).
```py
def lengthOfLongestSubstring(self, s):
  n = len(s)
  l = 0
  curMax = 0
  cToi = {}
  
  for r in range(n):
    c = s[r]
    if c in cToi:
      # make sure don't jump back
      l = max(l, cToi[c] + 1)
    
    curMax = max(curMax, r - l + 1)
    cToi[c] = r

  return curMax
```

## JS
```js
var lengthOfLongestSubstring = function(s) {
  let n = s.length;
  let l = 0;
  let r = 0;
  let chars = new Set();
  let longest = 0;
  
  while (r < n) {
    let cur = s[r];
    if (chars.has(cur)) {
      // remove from left
      longest = Math.max(longest, r - l);
      chars.delete(s[l]);
      l++;
    } else {
      // expand right boundary
      chars.add(cur);
      r++;
    }
  }
  
  // final check for last iteration
  longest = Math.max(longest, r - l);
  
  return longest;
};
```

```JS
var lengthOfLongestSubstring = function(s) {
  let n = s.length;
  let l = 0;
  const cToi = new Map();
  let longest = 0;
  
  for (let r = 0; r < n; r++) {
    let cur = s[r];
    if (cToi.has(cur)) {
      let lastIndex = cToi.get(cur);
      // jump left pointer if last seen of the repetition is after left pointer
      l = Math.max(l, lastIndex + 1)
    }
    cToi.set(cur, r);
    longest = Math.max(longest, r - l + 1);
  }
  
  return longest;
};
```

## TS
A slightly different solution is for each new seen char, first truncate left part of subarray to make sure after adding current char, the substring is valid.

```ts
function lengthOfLongestSubstring(s: string): number {
  const n: number = s.length;
  let l: number = 0;
  let longest: number = 0;
  let chars: Set<string> = new Set();
  
  for (let r: number = 0; r < n; r++) {
    let cur: string = s[r];
    while (chars.has(cur)) {
      let leftChar: string = s[l];
      chars.delete(leftChar);
      l++;
    }
    chars.add(cur);
    
    longest = Math.max(longest, r - l + 1)
  }
  
  return longest;
};
```

```ts
interface SeenIndex {
  [key: string]: number;
}
function lengthOfLongestSubstring(s: string): number {
  const n: number = s.length;
  let l: number = 0;
  let longest: number = 0;
  let cToi: SeenIndex = {};     // last seen index of a char
  
  for (let r: number = 0; r < n; r++) {
    let cur: string = s[r];
    if (cur in cToi) {
      l = Math.max(l, cToi[cur] + 1);
    } 
    cToi[cur] = r;
    
    longest = Math.max(longest, r - l + 1)
  }
  
  return longest;
};
```