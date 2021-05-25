# Problem
https://leetcode.com/problems/container-with-most-water/

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

### Example
Input: [1,8,6,2,5,4,8,3,7]

Output: 49

# Solutions
Time: O(n)
Space: O(1)

We want to start with the largest bottom length `n` (be greedy!), which corresponds to the largest area for `n`, then compute the largest possible area for `n - 1`, `n - 2`, ...  At each iteration by moving left pointer to right, or moving right pointer to left, we make sure this it the best move from step `x` to step `x - 1`.

### Python
```py
def maxArea(self, height):
    left = 0
    right = len(height) - 1
    maxArea = 0
    
    while(left < right):
      leftH = height[left]
      rightH = height[right]

      bottom = right - left
      h = min(leftH, rightH)
      area = bottom * h
      maxArea = max(area, maxArea)
      
      if (leftH > rightH):
        right -= 1
      else:
        left += 1
    
    return maxArea
```

### JS
```ts
var maxArea = function(height) {
  let left = 0;
  let right = height.length - 1;
  let maxArea = 0;
  
  while (left < right) {
    let leftH = height[left];
    let rightH = height[right];
    let bottom = right - left;

    let area = bottom * Math.min(leftH, rightH);
    maxArea = Math.max(maxArea, area);
    
    if (leftH < rightH) {
      left++;
    } else {
      right --;
    }
  }
  
  return maxArea;
};
```

### TS
```ts
function maxArea(height: number[]): number {
  let l: number = 0;
  let r: number = height.length - 1;
  let maxArea: number = 0;
  
  while (l < r) {
    // calculate cur area and update max area
    let lh: number = height[l];
    let rh: number = height[r];
    let curArea: number = (r - l) * Math.min(lh, rh);
    maxArea = Math.max(maxArea, curArea);
    
    // move pointer
    lh < rh
      ? l++
      : r--;
  }
  
  return maxArea;
};
```

