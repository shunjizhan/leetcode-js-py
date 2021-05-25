# 42. Trapping Rain Water
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

## example
Input: [0,1,0,2,1,0,1,3,2,1,2,1]

Output: 6

# Solution
Key point is, for each element, the rain it can trap is
```py
min(max(left bars), max(right bars)) - it's own height
```

## brute force
Time: O(n^2)
Space: O(1)

For each element, find `max(left bars)` and `max(right bars)`.

## memo and DP
Time: O(n)
Space: O(n)

Since in brute force, we can notice that we need to iterate through bars repeatly. So we can think about memoization to save some duplicated computation.

We can iterate the array twice (with memo and DP) to save the `max(left bars)` and `max(right bars)` for each item, so don't need to calculate them every time.

### Python
```py
def trap(self, height):
  l = len(height)
  if (l < 2):
    return 0
  
  total = 0
  lMax = {}
  rMax = {}
  
  lMax[0] = height[0]
  for i in range(1, l):
    lMax[i] = max(lMax[i - 1], height[i])
  
  rMax[l - 1] = height[l - 1]
  for i in range(l - 2, -1, -1):
    rMax[i] = max(rMax[i + 1], height[i])
    
  for i in range(1, l - 1):
    water = max(0, min(lMax[i - 1], rMax[i + 1]) - height[i])
    total += water
  
  return total
```

## two pointer
Time: O(n)
Space: O(1)

Key point is we kind of genereate the `max(left bars)` and `max(right bars)` on the fly. We have to move the pointer with shorter height, since water depends on the shorter bar. So that if next height is less, we can calculate the water directly, without worrying about the other side bars. 

### Python
```py
def trap(self, height):
  if (len(height) < 2):
    return 0
  
  l = 0
  r = len(height) - 1
  lh = height[l]
  rh = height[r]
  lmax = lh
  rmax = rh
  total = 0
  
  while (l < r):
    if (lh < rh):
      l += 1
      lh = height[l]
      if (lh > lmax):
        lmax = lh
      else:
        total += lmax - lh
    else:
      r -= 1
      rh = height[r]
      if (rh > rmax):
        rmax = rh
      else:
        total += rmax - rh
  
  return total
```

### JS
```js
var trap = function(height) {
  let l = 0;
  let r = height.length - 1;
  let lmax = height[l];
  let rmax = height[r];
  let total = 0;
  
  while (l < r) {
    if (height[l] < height[r]) {
      l++;
      let cur = height[l];
      if (cur < lmax) {
        water = lmax - cur;
        total += water;
      } else {
        lmax = cur;
      }
    } else {
      r--;
      let cur = height[r];
      if (cur < rmax) {
        water = rmax - cur;
        total += water;
      } else {
        rmax = cur;
      }
    }
  }
  
  return total;
};
```

```ts
function trap(height: number[]): number {  
  let total: number = 0;
  let l: number = 0;
  let r: number = height.length - 1;
  let lMax: number = height[l];
  let rMax: number = height[r];
  
  while(l < r) {
    let lh: number = height[l]
    let rh: number = height[r];
    let water: number;
    let cur: number;

    if (lh < rh) {
      l++;
      cur = height[l]
      if (cur < lMax) {
        water = lMax - cur;
        total += water;
      } else {
        lMax = cur
      }
    } else {
      r--;
      cur = height[r]
      if (cur < rMax) {
        water = rMax - cur;
        total += water;
      } else {
        rMax = cur
      }
    }
  }
  
  return total;
};
```