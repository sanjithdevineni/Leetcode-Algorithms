# Remove Duplicates from Sorted Array

### Difficulty: Easy

Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in `nums`.

Consider the number of unique elements of `nums` to be `k`, to get accepted, you need to do the following things:
- Change the array `nums` such that the first `k` elements of `nums` contain the unique elements in the order they were present in `nums` initially. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

### Example 1:

Input: nums = [1,1,2]

Output: 2, nums = [1,2,_]

### Example 2:

Input: nums = [0,0,1,1,1,2,2,3,3,4]

Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]

### Constraints:

- 1 <= nums.length <= 3 * 10^4
- -100 <= nums[i] <= 100
- nums is sorted in non-decreasing order.

### Follow up:

Can you solve it in O(1) extra space?

## Solution:

### Most Efficient Approach: Two-Pointer Technique

The two-pointer technique efficiently removes duplicates in-place with O(1) extra space by using a slow pointer to track the next unique element position.

#### Algorithm Steps:
1. **Initialize**: Set slow pointer `i` to 1 (first position for next unique element)
2. **Traverse**: Use fast pointer `j` to scan the array from index 1
3. **Compare**: If `nums[j] != nums[i-1]`, place `nums[j]` at position `i`
4. **Increment**: Move `i` forward only when we find a unique element
5. **Return**: `i` represents the count of unique elements

#### Pseudo-code:
```python
def remove_duplicates(nums):
    i = 1  # Slow pointer for next unique element position
    
    for j in range(1, len(nums)):  # Fast pointer
        if nums[j] != nums[i - 1]:  # Found unique element
            nums[i] = nums[j]       # Place it at position i
            i += 1                  # Move slow pointer
    
    return i  # Number of unique elements
```

#### Visual Example:
```
Array: [0,0,1,1,1,2,2,3,3,4]
Step 1: i=1, j=1, nums[1]=0, nums[0]=0 → skip
Step 2: i=1, j=2, nums[2]=1, nums[0]=0 → nums[1]=1, i=2
Step 3: i=2, j=3, nums[3]=1, nums[1]=1 → skip
Step 4: i=2, j=4, nums[4]=1, nums[1]=1 → skip
Step 5: i=2, j=5, nums[5]=2, nums[1]=1 → nums[2]=2, i=3
Step 6: i=3, j=6, nums[6]=2, nums[2]=2 → skip
Step 7: i=3, j=7, nums[7]=3, nums[2]=2 → nums[3]=3, i=4
Step 8: i=4, j=8, nums[8]=3, nums[3]=3 → skip
Step 9: i=4, j=9, nums[9]=4, nums[3]=3 → nums[4]=4, i=5
Result: 5 unique elements [0,1,2,3,4,_,_,_,_,_]
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(1) - Only using two variables
- **In-place**: Yes, modifies the original array

#### Why This Works:
The key insight is that since the array is sorted, duplicates will always be adjacent. The slow pointer `i` tracks the position for the next unique element, while the fast pointer `j` finds unique elements by comparing with the previous unique element at `i-1`.

#### Edge Cases Handled:
- Single element array: Returns 1
- Array with all same elements: Returns 1
- Array with no duplicates: Returns original length
- Empty array: Handled by constraints (length >= 1)

#### Related Problems:
- Remove Duplicates from Sorted Array II (allow at most 2 duplicates)
- Remove Element (remove specific value)
- Move Zeroes (move zeros to end)
