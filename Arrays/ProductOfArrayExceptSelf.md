# Product of Array Except Self

### Difficulty: Medium

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

### Example 1:

Input: nums = [1,2,3,4]

Output: [24,12,8,6]

### Example 2:

Input: nums = [-1,1,0,-3,3]

Output: [0,0,9,0,0]

### Constraints:

- 2 <= nums.length <= 10^5
- -30 <= nums[i] <= 30
- The input is generated such that answer[i] is guaranteed to fit in a 32-bit integer.

### Follow up:
Can you solve the problem in O(1) extra space complexity? (The output array does not count as extra space for space complexity analysis.)

## Solution:

### Most Efficient Approach: Two-Pass Prefix and Suffix Products

The key insight is to calculate prefix and suffix products separately, then combine them.

#### Algorithm Steps:
1. **First pass (left to right)**: Calculate prefix products and store in answer array
2. **Second pass (right to left)**: Calculate suffix products and multiply with existing values
3. **Result**: Each answer[i] = prefix[i] * suffix[i]

#### Pseudo-code:
```python
def product_except_self(nums):
    answer = [1] * len(nums)  # Initialize with 1s
    prefix = 1
    
    # First pass: calculate prefix products
    for i in range(len(nums)):
        answer[i] *= prefix    # Store prefix product
        prefix *= nums[i]      # Update prefix for next iteration
    
    suffix = 1
    
    # Second pass: calculate suffix products
    for i in range(len(nums) - 1, -1, -1):
        answer[i] *= suffix    # Multiply with suffix product
        suffix *= nums[i]      # Update suffix for next iteration
    
    return answer
```

#### Visual Example 1:
```
nums = [1,2,3,4]

First pass (prefix products):
i=0: answer[0] = 1, prefix = 1*1 = 1
i=1: answer[1] = 1, prefix = 1*2 = 2
i=2: answer[2] = 2, prefix = 2*3 = 6
i=3: answer[3] = 6, prefix = 6*4 = 24
After first pass: answer = [1,1,2,6]

Second pass (suffix products):
i=3: answer[3] = 6*1 = 6, suffix = 1*4 = 4
i=2: answer[2] = 2*4 = 8, suffix = 4*3 = 12
i=1: answer[1] = 1*12 = 12, suffix = 12*2 = 24
i=0: answer[0] = 1*24 = 24, suffix = 24*1 = 24
Final result: answer = [24,12,8,6]
```

#### Visual Example 2:
```
nums = [-1,1,0,-3,3]

First pass (prefix products):
i=0: answer[0] = 1, prefix = 1*(-1) = -1
i=1: answer[1] = -1, prefix = -1*1 = -1
i=2: answer[2] = -1, prefix = -1*0 = 0
i=3: answer[3] = 0, prefix = 0*(-3) = 0
i=4: answer[4] = 0, prefix = 0*3 = 0
After first pass: answer = [1,-1,-1,0,0]

Second pass (suffix products):
i=4: answer[4] = 0*1 = 0, suffix = 1*3 = 3
i=3: answer[3] = 0*3 = 0, suffix = 3*(-3) = -9
i=2: answer[2] = -1*(-9) = 9, suffix = -9*0 = 0
i=1: answer[1] = -1*0 = 0, suffix = 0*1 = 0
i=0: answer[0] = 1*0 = 0, suffix = 0*(-1) = 0
Final result: answer = [0,0,9,0,0]
```

#### Alternative Approach (Using Extra Space):
```python
def product_except_self_extra_space(nums):
    n = len(nums)
    prefix = [1] * n
    suffix = [1] * n
    answer = [1] * n
    
    # Calculate prefix products
    for i in range(1, n):
        prefix[i] = prefix[i-1] * nums[i-1]
    
    # Calculate suffix products
    for i in range(n-2, -1, -1):
        suffix[i] = suffix[i+1] * nums[i+1]
    
    # Combine prefix and suffix
    for i in range(n):
        answer[i] = prefix[i] * suffix[i]
    
    return answer
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Two passes through the array
- **Space Complexity**: O(1) - Only using constant extra space (excluding output)
- **In-place**: Modifies the output array

#### Why This Works:
1. **Prefix products**: answer[i] contains product of all elements to the left
2. **Suffix products**: answer[i] gets multiplied by product of all elements to the right
3. **Combination**: Each answer[i] = product of all elements except nums[i]
4. **No division**: Avoids division operation as required

#### Key Insights:
- **Two-pass approach**: Separate calculation of prefix and suffix products
- **In-place calculation**: Use output array to store intermediate results
- **Constant space**: Only need two variables (prefix, suffix)
- **No division**: Avoids division operation completely

#### Edge Cases Handled:
- Single zero: All products become 0 except at zero position
- Multiple zeros: All products become 0
- Negative numbers: Handled correctly with sign changes
- Large numbers: Guaranteed to fit in 32-bit integer

#### Mathematical Insight:
For each position i:
- answer[i] = product of all elements except nums[i]
- answer[i] = (nums[0] * nums[1] * ... * nums[i-1]) * (nums[i+1] * nums[i+2] * ... * nums[n-1])
- answer[i] = prefix[i] * suffix[i]

#### Related Problems:
- Product of Array Except Self (with division allowed)
- Trapping Rain Water
- Maximum Product Subarray
- Subarray Product Less Than K

#### Pattern Recognition:
This problem demonstrates the **Prefix and Suffix Products** pattern:
- **Prefix calculation**: Calculate products from left to right
- **Suffix calculation**: Calculate products from right to left
- **Combination**: Combine prefix and suffix for final result
- **Space optimization**: Use output array for intermediate storage

#### Follow-up Solution:
The provided solution already achieves O(1) extra space complexity by:
- Using the output array to store prefix products
- Using only two variables (prefix, suffix) for calculations
- Not counting the output array in space complexity analysis

#### Real-World Applications:
- **Signal processing**: Filtering operations
- **Image processing**: Convolution operations
- **Statistics**: Moving averages and products
- **Cryptography**: Hash function calculations
