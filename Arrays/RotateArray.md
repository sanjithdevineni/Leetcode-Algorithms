# Rotate Array

### Difficulty: Medium

Given an integer array nums, rotate the array to the right by k steps, where k is non-negative.

### Example 1:

Input: nums = [1,2,3,4,5,6,7], k = 3

Output: [5,6,7,1,2,3,4]

### Explanation:

rotate 1 steps to the right: [7,1,2,3,4,5,6]

rotate 2 steps to the right: [6,7,1,2,3,4,5]

rotate 3 steps to the right: [5,6,7,1,2,3,4]

### Example 2:

Input: nums = [-1,-100,3,99], k = 2

Output: [3,99,-1,-100]

### Explanation: 

rotate 1 steps to the right: [99,-1,-100,3]

rotate 2 steps to the right: [3,99,-1,-100]
 

### Constraints:

1 <= nums.length <= 105

-231 <= nums[i] <= 231 - 1

0 <= k <= 105
 

### Follow up:

Try to come up with as many solutions as you can. There are at least three different ways to solve this problem.
Could you do it in-place with O(1) extra space?



## Solution:

### Most Efficient Approach: Array Reversal Algorithm

The most efficient solution uses the **reversal technique** with O(1) extra space.

#### Algorithm Steps:
1. **Normalize k**: Handle cases where k > array length
2. **Reverse last k elements**: [1,2,3,4,5,6,7] → [1,2,3,4,7,6,5]
3. **Reverse first n-k elements**: [1,2,3,4,7,6,5] → [4,3,2,1,7,6,5]
4. **Reverse entire array**: [4,3,2,1,7,6,5] → [5,6,7,1,2,3,4]

#### Pseudo-code:
```python
def rotate(nums, k):
    n = len(nums)
    k = k % n  # Normalize k to handle k > n
    
    # Reverse last k elements
    reverse(nums, n-k, n-1)
    
    # Reverse first n-k elements  
    reverse(nums, 0, n-k-1)
    
    # Reverse entire array
    reverse(nums, 0, n-1)

def reverse(arr, start, end):
    while start < end:
        arr[start], arr[end] = arr[end], arr[start]  # Python swap
        start += 1
        end -= 1
```

#### Visual Example (k=3):
```
Original: [1,2,3,4,5,6,7]
Step 1:   [1,2,3,4,7,6,5]  // Reverse last 3
Step 2:   [4,3,2,1,7,6,5]  // Reverse first 4  
Step 3:   [5,6,7,1,2,3,4]  // Reverse entire
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Each element is touched exactly twice
- **Space Complexity**: O(1) - Only using constant extra space
- **In-place**: Yes, modifies the original array

#### Why This Works:
The key insight is that rotating right by k positions is equivalent to:
1. Moving the last k elements to the front
2. Moving the first n-k elements to the back

The reversal technique achieves this by strategically reversing different segments of the array.

#### Edge Cases Handled:
- k = 0: No rotation needed
- k = n: Full rotation, array remains same
- k > n: Normalized using modulo operation
- Single element array: Works correctly

