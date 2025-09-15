# Subsequence Sum After Capping Elements

### Difficulty: Medium

You are given an integer array `nums` of size n and a positive integer k.

An array capped by value x is obtained by replacing every element `nums[i]` with `min(nums[i], x)`.

For each integer x from 1 to n, determine whether it is possible to choose a subsequence from the array capped by x such that the sum of the chosen elements is exactly k.

Return a 0-indexed boolean array `answer` of size n, where `answer[i]` is true if it is possible when using x = i + 1, and false otherwise.

### Example 1:

Input: nums = [4,3,2,4], k = 5

Output: [false,false,true,true]

Explanation:
- For x = 1, the capped array is [1, 1, 1, 1]. Possible sums are 1, 2, 3, 4, so it is impossible to form a sum of 5.
- For x = 2, the capped array is [2, 2, 2, 2]. Possible sums are 2, 4, 6, 8, so it is impossible to form a sum of 5.
- For x = 3, the capped array is [3, 3, 2, 3]. A subsequence [2, 3] sums to 5, so it is possible.
- For x = 4, the capped array is [4, 3, 2, 4]. A subsequence [3, 2] sums to 5, so it is possible.

### Example 2:

Input: nums = [1,2,3,4,5], k = 3

Output: [true,true,true,true,true]

Explanation:
For every value of x, it is always possible to select a subsequence from the capped array that sums exactly to 3.

### Constraints:

- 1 <= n == nums.length <= 4000
- 1 <= nums[i] <= n
- 1 <= k <= 4000

## Solution:

### Most Efficient Approach: Bit Manipulation DP with Sorting

The key insight is to use bit manipulation to efficiently track achievable sums and leverage sorting to process elements in ascending order.

#### Algorithm Steps:
1. **Sort**: Sort the array to process elements in ascending order
2. **Initialize DP**: Use bitmask DP to track achievable sums
3. **Process elements**: For each x, update DP with elements <= x
4. **Check feasibility**: Use modular arithmetic to check if sum k is achievable
5. **Return results**: Return boolean array for each x value

#### Pseudo-code:
```python
def subsequence_sum_after_capping(nums, k):
    nums.sort()  # Sort array in ascending order
    n = len(nums)
    res = [False] * n
    dp = 1  # Bitmask: dp[i] = 1 if sum i is achievable
    mask = (1 << (k + 1)) - 1  # Mask to keep only relevant bits
    i = 0  # Pointer to current element
    
    for x in range(1, n + 1):
        # Add elements <= x to DP
        while i < n and nums[i] <= x:
            dp |= (dp << nums[i]) & mask  # Update achievable sums
            i += 1
        
        # Check if sum k is achievable
        v = max(k % x, k - (n - i) * x)  # Optimization for large x
        for j in range(v, k + 1, x):
            if dp & (1 << j):  # Check if sum j is achievable
                res[x - 1] = True
                break
    
    return res
```

#### Visual Example 1:
```
nums = [4,3,2,4], k = 5
After sorting: [2,3,4,4]

x = 1: capped = [1,1,1,1]
- Process elements <= 1: none
- dp = 1 (only sum 0 achievable)
- Check k=5: not achievable → res[0] = False

x = 2: capped = [2,2,2,2]
- Process elements <= 2: nums[0]=2
- dp = 1 | (1 << 2) = 5 (sums 0,2 achievable)
- Check k=5: not achievable → res[1] = False

x = 3: capped = [3,3,2,3]
- Process elements <= 3: nums[1]=3
- dp = 5 | (5 << 3) = 45 (sums 0,2,3,5 achievable)
- Check k=5: achievable → res[2] = True

x = 4: capped = [4,3,2,4]
- Process elements <= 4: nums[2]=4, nums[3]=4
- dp = 45 | (45 << 4) = 765 (more sums achievable)
- Check k=5: achievable → res[3] = True

Result: [False, False, True, True]
```

#### Visual Example 2:
```
nums = [1,2,3,4,5], k = 3
After sorting: [1,2,3,4,5]

x = 1: capped = [1,1,1,1,1]
- Process elements <= 1: nums[0]=1
- dp = 1 | (1 << 1) = 3 (sums 0,1 achievable)
- Check k=3: not achievable yet

x = 2: capped = [2,2,2,2,2]
- Process elements <= 2: nums[1]=2
- dp = 3 | (3 << 2) = 15 (sums 0,1,2,3 achievable)
- Check k=3: achievable → res[1] = True

x = 3: capped = [3,3,3,3,3]
- Process elements <= 3: nums[2]=3
- dp = 15 | (15 << 3) = 255 (more sums achievable)
- Check k=3: achievable → res[2] = True

Result: [True, True, True, True, True]
```

#### Alternative Approach (Naive DP):
```python
def subsequence_sum_naive(nums, k):
    n = len(nums)
    res = [False] * n
    
    for x in range(1, n + 1):
        # Create capped array
        capped = [min(num, x) for num in nums]
        
        # DP to check if sum k is achievable
        dp = [False] * (k + 1)
        dp[0] = True
        
        for num in capped:
            for j in range(k, num - 1, -1):
                if dp[j - num]:
                    dp[j] = True
        
        res[x - 1] = dp[k]
    
    return res
```

#### Complexity Analysis:
- **Time Complexity**: O(n² + nk) - Sorting + DP for each x
- **Space Complexity**: O(k) - For DP bitmask
- **Optimization**: Bit manipulation reduces constant factors significantly

#### Why This Works:
1. **Sorting insight**: Process elements in ascending order to reuse DP state
2. **Bit manipulation**: Efficiently track achievable sums using bit operations
3. **Modular arithmetic**: Optimize checking for large x values
4. **DP reuse**: Build upon previous DP state instead of recomputing

#### Key Insights:
- **Sorting requirement**: Must sort to process elements in ascending order
- **Bit manipulation**: Use bitmask DP for efficient sum tracking
- **Modular optimization**: Use `k % x` and `k - (n-i)*x` for large x
- **DP state reuse**: Build upon previous state instead of recomputing

#### Edge Cases Handled:
- All elements are the same: Correctly handles uniform arrays
- k equals array length: Handles maximum possible sum
- k is 1: Handles minimum possible sum
- Large x values: Uses modular arithmetic optimization

#### Mathematical Insight:
- **Subset sum problem**: Classic NP-complete problem with DP solution
- **Bit manipulation**: Efficient representation of achievable sums
- **Modular arithmetic**: Optimization for large cap values
- **Sorting optimization**: Reduces redundant computations

#### Related Problems:
- Subset Sum
- Partition Equal Subset Sum
- Target Sum
- Coin Change

#### Pattern Recognition:
This problem demonstrates the **Dynamic Programming with Bit Manipulation** pattern:
- **DP state**: Bitmask representing achievable sums
- **State transition**: Bit shifting operations
- **Optimization**: Sorting and modular arithmetic
- **Efficiency**: O(n²) instead of O(n²k) naive approach

#### Real-World Applications:
- **Resource allocation**: Determining feasible resource combinations
- **Budget planning**: Checking if target budget is achievable
- **Inventory management**: Verifying if target quantity is reachable
- **Gaming**: Checking if target score is achievable

#### Optimization Notes:
- **Bit manipulation**: Use bit operations instead of boolean arrays
- **Sorting**: Process elements in ascending order to reuse DP state
- **Modular arithmetic**: Optimize checking for large cap values
- **Early termination**: Stop when target sum is found

#### Algorithm Correctness:
1. **DP correctness**: Bitmask correctly represents achievable sums
2. **Sorting correctness**: Ascending order ensures proper element processing
3. **Modular optimization**: Mathematical correctness of optimization
4. **Boundary handling**: Correctly handles edge cases

#### Constraint Analysis:
- **Array length**: 1 <= n <= 4000 (moderate size, DP feasible)
- **Element range**: 1 <= nums[i] <= n (bounded range)
- **Target sum**: 1 <= k <= 4000 (bounded target)
- **Result size**: n boolean values

#### Code Variations:
```python
# Using set for DP (more readable)
def subsequence_sum_set(nums, k):
    nums.sort()
    n = len(nums)
    res = [False] * n
    achievable = {0}
    i = 0
    
    for x in range(1, n + 1):
        while i < n and nums[i] <= x:
            new_sums = set()
            for s in achievable:
                if s + nums[i] <= k:
                    new_sums.add(s + nums[i])
            achievable.update(new_sums)
            i += 1
        
        res[x - 1] = k in achievable
    
    return res

# Using list for DP (space efficient)
def subsequence_sum_list(nums, k):
    nums.sort()
    n = len(nums)
    res = [False] * n
    dp = [False] * (k + 1)
    dp[0] = True
    i = 0
    
    for x in range(1, n + 1):
        while i < n and nums[i] <= x:
            for j in range(k, nums[i] - 1, -1):
                if dp[j - nums[i]]:
                    dp[j] = True
            i += 1
        
        res[x - 1] = dp[k]
    
    return res
```

#### Performance Characteristics:
- **Time complexity**: O(n² + nk) - dominated by DP operations
- **Space complexity**: O(k) - for DP bitmask
- **Cache efficiency**: Good due to sequential access after sorting
- **Branch prediction**: Excellent due to simple bit operations

#### Advanced Optimizations:
- **Bit manipulation**: Use bit operations for O(1) updates
- **Modular arithmetic**: Reduce checking range for large x
- **Early termination**: Stop when target sum is found
- **Memory optimization**: Use bitmask instead of boolean array

#### Mathematical Properties:
- **Subset sum**: Classic NP-complete problem
- **Bit manipulation**: Efficient representation of sets
- **Modular arithmetic**: Mathematical optimization technique
- **Sorting**: Reduces redundant computations

#### Algorithm Elegance:
The solution demonstrates several advanced techniques:
- **Bit manipulation DP**: Efficient state representation
- **Sorting optimization**: Reduces redundant computations
- **Modular arithmetic**: Mathematical optimization
- **State reuse**: Builds upon previous computations

#### Debugging Tips:
- **Bit visualization**: Print binary representation of DP state
- **Step-by-step**: Trace through each x value
- **Edge cases**: Test with small arrays first
- **Modular check**: Verify modular arithmetic optimization

#### Testing Strategy:
- **Small cases**: Test with n=1,2,3
- **Edge cases**: Test with k=1, k=n
- **Uniform arrays**: Test with all elements equal
- **Large arrays**: Test with maximum constraints

#### Memory Analysis:
- **Bitmask size**: O(k) bits for DP state
- **Result array**: O(n) boolean values
- **Temporary variables**: O(1) additional space
- **Total space**: O(n + k) overall

#### Time Analysis:
- **Sorting**: O(n log n) for array sorting
- **DP updates**: O(nk) for all x values
- **Modular checks**: O(k) for each x value
- **Total time**: O(n log n + nk) overall

#### Optimization Trade-offs:
- **Space vs Time**: Bit manipulation saves space but adds complexity
- **Sorting vs DP**: Sorting reduces DP computations
- **Modular vs Naive**: Modular arithmetic optimizes large x values
- **Readability vs Performance**: Bit manipulation is faster but less readable
