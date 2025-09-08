# Count Bowl Subarrays

### Difficulty: Medium

You are given an integer array `nums` with distinct elements.

A subarray `nums[l...r]` of `nums` is called a bowl if:

1. The subarray has length at least 3. That is, `r - l + 1 >= 3`.
2. The minimum of its two ends is strictly greater than the maximum of all elements in between. That is, `min(nums[l], nums[r]) > max(nums[l + 1], ..., nums[r - 1])`.

Return the number of bowl subarrays in `nums`.

### Example 1:

Input: nums = [2,5,3,1,4]

Output: 2

Explanation: The bowl subarrays are [3, 1, 4] and [5, 3, 1, 4].

[3, 1, 4] is a bowl because min(3, 4) = 3 > max(1) = 1.

[5, 3, 1, 4] is a bowl because min(5, 4) = 4 > max(3, 1) = 3.

### Example 2:

Input: nums = [5,1,2,3,4]

Output: 3

Explanation: The bowl subarrays are [5, 1, 2], [5, 1, 2, 3] and [5, 1, 2, 3, 4].

### Example 3:

Input: nums = [1000000000,999999999,999999998]

Output: 0

Explanation: No subarray is a bowl.

### Constraints:

- 3 <= nums.length <= 10^5
- 1 <= nums[i] <= 10^9
- nums consists of distinct elements.

## Solution:

### Most Efficient Approach: Monotonic Stack

The key insight is to use a monotonic stack to efficiently find all bowl subarrays by maintaining elements in decreasing order.

#### Algorithm Steps:
1. **Initialize**: Use a stack to maintain indices in decreasing order of values
2. **Process each element**: For each element, pop smaller elements from stack
3. **Check bowl condition**: When popping an element (mid), check if it forms a bowl with previous element (left) and current element (right)
4. **Count bowls**: If `min(nums[left], nums[right]) > nums[mid]`, increment count

#### Pseudo-code:
```python
def bowl_subarrays(nums):
    ans = 0
    stack = []
    
    for i in range(len(nums)):
        # Pop smaller elements and check for bowls
        while stack and nums[stack[-1]] < nums[i]:
            mid = stack.pop()
            if stack:  # There's a left element
                left = stack[-1]
                if min(nums[left], nums[i]) > nums[mid]:
                    ans += 1
        stack.append(i)
    
    return ans
```

#### Visual Example:
```
Array: [2,5,3,1,4]
Stack: []
i=0: stack=[0], nums[0]=2
i=1: nums[1]=5 > nums[0]=2, pop 0, check bowl
      left=None, no bowl formed
      stack=[1], nums[1]=5
i=2: nums[2]=3 < nums[1]=5, no pop
      stack=[1,2], nums[2]=3
i=3: nums[3]=1 < nums[2]=3, no pop
      stack=[1,2,3], nums[3]=1
i=4: nums[4]=4 > nums[3]=1, pop 3, check bowl
      left=2, min(nums[2],nums[4])=min(3,4)=3 > nums[3]=1 ✓
      nums[4]=4 > nums[2]=3, pop 2, check bowl
      left=1, min(nums[1],nums[4])=min(5,4)=4 > nums[2]=3 ✓
      stack=[1,4], nums[4]=4
Result: 2 bowls found
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Each element is pushed and popped at most once
- **Space Complexity**: O(n) - Stack can contain at most n elements
- **In-place**: No modification of original array

#### Why This Works:
The key insight is that:
1. **Monotonic stack**: Maintains elements in decreasing order
2. **Bowl detection**: When popping an element, it becomes the "middle" of a potential bowl
3. **Efficient checking**: We only check bowls when we have a valid left and right boundary
4. **Distinct elements**: Ensures no duplicate comparisons

#### Edge Cases Handled:
- Array with no bowls: Returns 0
- Array with all increasing elements: Returns 0
- Array with all decreasing elements: Returns 0
- Single bowl: Correctly counts 1

#### Related Problems:
- Largest Rectangle in Histogram (monotonic stack)
- Next Greater Element (monotonic stack)
- Trapping Rain Water (monotonic stack)
