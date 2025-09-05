# Remove Duplicates from Sorted Array II

### Difficulty: Medium

Given an integer array `nums` sorted in non-decreasing order, remove some duplicates in-place such that each unique element appears at most twice. The relative order of the elements should be kept the same.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` after placing the final result in the first `k` slots of `nums`.

### Example 1:

Input: nums = [1,1,1,2,2,3]

Output: 5, nums = [1,1,2,2,3,_]

### Example 2:

Input: nums = [0,0,1,1,1,1,2,3,3]

Output: 7, nums = [0,0,1,1,2,3,3,_,_]

### Constraints:

- 1 <= nums.length <= 3 * 10^4
- -10^4 <= nums[i] <= 10^4
- nums is sorted in non-decreasing order.

### Follow up:

Can you solve it in O(1) extra space?

## Solution:

### Most Efficient Approach: Two-Pointer Technique (Extended)

The two-pointer technique is extended to allow at most 2 duplicates by comparing with the element at position `k-2`.

#### Algorithm Steps:
1. **Initialize**: Set `k = 2` (first position for next unique element, allowing 2 duplicates)
2. **Traverse**: Use pointer `i` to scan the array from index 2
3. **Compare**: If `nums[i] != nums[k-2]`, place `nums[i]` at position `k`
4. **Increment**: Move `k` forward only when we find a valid element
5. **Return**: `k` represents the count of elements (allowing at most 2 duplicates)

#### Pseudo-code:
```python
def remove_duplicates(nums):
    k = 2  # Allow at most 2 duplicates
    
    for i in range(2, len(nums)):  # Start from index 2
        if nums[i] != nums[k - 2]:  # Compare with element at k-2
            nums[k] = nums[i]       # Place it at position k
            k += 1                  # Move k forward
    
    return k  # Number of elements (at most 2 duplicates each)
```

#### Visual Example:
```
Array: [0,0,1,1,1,1,2,3,3]
Step 1: k=2, i=2, nums[2]=1, nums[0]=0 → nums[2]=1, k=3
Step 2: k=3, i=3, nums[3]=1, nums[1]=0 → nums[3]=1, k=4
Step 3: k=4, i=4, nums[4]=1, nums[2]=1 → skip (duplicate)
Step 4: k=4, i=5, nums[5]=1, nums[2]=1 → skip (duplicate)
Step 5: k=4, i=6, nums[6]=2, nums[2]=1 → nums[4]=2, k=5
Step 6: k=5, i=7, nums[7]=3, nums[3]=1 → nums[5]=3, k=6
Step 7: k=6, i=8, nums[8]=3, nums[4]=2 → nums[6]=3, k=7
Result: 7 elements [0,0,1,1,2,3,3,_,_]
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(1) - Only using one variable
- **In-place**: Yes, modifies the original array

#### Why This Works:
The key insight is that by comparing with `nums[k-2]`, we ensure that:
1. If we place an element at position `k`, the elements at positions `k-2` and `k-1` are different
2. This guarantees at most 2 duplicates of any element
3. The first two elements are always valid (no comparison needed)

#### Edge Cases Handled:
- Array with all same elements: Returns 2
- Array with no duplicates: Returns original length
- Array with exactly 2 of each element: Returns original length
- Single element array: Returns 1
- Two element array: Returns 2

#### Related Problems:
- Remove Duplicates from Sorted Array (allow at most 1 duplicate)
- Remove Element (remove specific value)
- Move Zeroes (move zeros to end)
