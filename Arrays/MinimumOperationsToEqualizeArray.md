# Minimum Operations to Equalize Array

### Difficulty: Easy

You are given an integer array `nums` of length `n`.

In one operation, choose any subarray `nums[l...r]` (0 <= l <= r < n) and replace each element in that subarray with the bitwise AND of all elements.

Return the minimum number of operations required to make all elements of `nums` equal.

A subarray is a contiguous non-empty sequence of elements within an array.

### Example 1:

Input: nums = [1,2]

Output: 1

Explanation: Choose nums[0...1]: (1 AND 2) = 0, so the array becomes [0, 0] and all elements are equal in 1 operation.

### Example 2:

Input: nums = [5,5,5]

Output: 0

Explanation: nums is [5, 5, 5] which already has all elements equal, so 0 operations are required.

### Constraints:

- 1 <= n == nums.length <= 100
- 1 <= nums[i] <= 10^5

## Solution:

### Most Efficient Approach: Binary Check

The key insight is that we only need to check if all elements are already equal. If they are, return 0. If not, return 1.

#### Algorithm Steps:
1. **Check equality**: Determine if all elements in the array are the same
2. **Return result**: If all equal, return 0; otherwise, return 1

#### Pseudo-code:
```python
def min_operations(nums):
    if len(set(nums)) == 1:  # All elements are equal
        return 0
    return 1  # Can always make equal in one operation
```

#### Why This Works:
The key insight is that if all elements are already equal, no operations are needed (return 0). If they're not equal, we can always make them equal in exactly 1 operation by:
1. Choosing the entire array as the subarray `nums[0...n-1]`
2. Computing the bitwise AND of all elements
3. Replacing all elements with this result

This is always possible because the bitwise AND operation will produce a single value that can replace all elements.

#### Visual Example:
```
Array: [1, 2]
All equal? No (1 ≠ 2)
Choose entire array: nums[0...1]
Compute: 1 AND 2 = 0
Replace all with 0: [0, 0]
Answer: 1 operation

Array: [5, 5, 5]  
All equal? Yes (5 = 5 = 5)
Answer: 0 operations

Array: [3, 7, 3]
All equal? No (3 ≠ 7 ≠ 3)
Choose entire array: nums[0...2]
Compute: 3 AND 7 AND 3 = 3
Replace all with 3: [3, 3, 3]
Answer: 1 operation
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Need to check all elements to determine if they're equal
- **Space Complexity**: O(n) - `set()` creates a set of unique elements
- **Alternative**: O(1) space if we check elements one by one

#### Edge Cases Handled:
- Single element array: Returns 0 (already equal)
- All elements same: Returns 0
- All elements different: Returns 1
- Mixed duplicates: Returns 1 if not all same

#### Related Problems:
- Bitwise AND of Numbers Range
- Single Number (bitwise operations)
- Maximum AND Sum of Array
