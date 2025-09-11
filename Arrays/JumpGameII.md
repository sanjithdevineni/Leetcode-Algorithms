# Jump Game II

### Difficulty: Medium

You are given a 0-indexed array of integers `nums` of length `n`. You are initially positioned at index 0.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at index `i`, you can jump to any index `(i + j)` where:

- 0 <= j <= nums[i] and
- i + j < n

Return the minimum number of jumps to reach index `n - 1`. The test cases are generated such that you can reach index `n - 1`.

### Example 1:

Input: nums = [2,3,1,1,4]

Output: 2

Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

### Example 2:

Input: nums = [2,3,0,1,4]

Output: 2

### Constraints:

- 1 <= nums.length <= 10^4
- 0 <= nums[i] <= 1000
- It's guaranteed that you can reach nums[n - 1]

## Solution:

### Most Efficient Approach: Range-Based Greedy

The key insight is to track the current range we can reach and only jump when we've exhausted that range.

#### Algorithm Steps:
1. **Initialize**: Set farthest reach to 0, current range end to 0, jumps to 0
2. **Process each position**: For each position, update the farthest reachable position
3. **Jump when needed**: When we reach the end of current range, increment jumps and update range
4. **Early termination**: If we can reach the last index, return jumps

#### Pseudo-code:
```python
def jump(nums):
    if len(nums) == 1:
        return 0  # Already at last index
    
    farthest = 0      # Farthest position we can reach
    current_end = 0   # End of current range
    jumps = 0         # Number of jumps taken
    
    for i in range(len(nums)):
        farthest = max(farthest, i + nums[i])  # Update farthest reach
        
        if i == current_end:  # Reached end of current range
            jumps += 1        # Must jump
            current_end = farthest  # Update to new range
        
        if current_end >= len(nums) - 1:  # Can reach last index
            return jumps
    
    return jumps
```

#### Visual Example 1:
```
nums = [2,3,1,1,4]
Index: 0 1 2 3 4
Value: 2 3 1 1 4

Step 0: i=0, farthest=0+2=2, current_end=0, jumps=0
        i == current_end? Yes → jumps=1, current_end=2

Step 1: i=1, farthest=max(2,1+3)=4, current_end=2, jumps=1
        i == current_end? No → no jump

Step 2: i=2, farthest=max(4,2+1)=4, current_end=2, jumps=1
        i == current_end? Yes → jumps=2, current_end=4

Step 3: i=3, farthest=max(4,3+1)=4, current_end=4, jumps=2
        i == current_end? Yes → jumps=3, current_end=4

Step 4: i=4, farthest=max(4,4+4)=8, current_end=4, jumps=2
        current_end >= 4? Yes → return 2

Result: 2 jumps
```

#### Visual Example 2:
```
nums = [2,3,0,1,4]
Index: 0 1 2 3 4
Value: 2 3 0 1 4

Step 0: i=0, farthest=0+2=2, current_end=0, jumps=0
        i == current_end? Yes → jumps=1, current_end=2

Step 1: i=1, farthest=max(2,1+3)=4, current_end=2, jumps=1
        i == current_end? No → no jump

Step 2: i=2, farthest=max(4,2+0)=4, current_end=2, jumps=1
        i == current_end? Yes → jumps=2, current_end=4

Step 3: i=3, farthest=max(4,3+1)=4, current_end=4, jumps=2
        i == current_end? Yes → jumps=3, current_end=4

Step 4: i=4, farthest=max(4,4+4)=8, current_end=4, jumps=2
        current_end >= 4? Yes → return 2

Result: 2 jumps
```

#### Alternative Approach (BFS-like):
```python
def jump_bfs(nums):
    if len(nums) == 1:
        return 0
    
    jumps = 0
    current_start = 0
    current_end = 0
    
    while current_end < len(nums) - 1:
        farthest = 0
        for i in range(current_start, current_end + 1):
            farthest = max(farthest, i + nums[i])
        
        current_start = current_end + 1
        current_end = farthest
        jumps += 1
    
    return jumps
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(1) - Only using three variables
- **In-place**: No modification of original array

#### Why This Works:
1. **Greedy choice**: Always jump to the farthest reachable position
2. **Range tracking**: Only jump when we've exhausted the current range
3. **Optimal substructure**: Minimum jumps to reach any position is optimal
4. **Early termination**: Stop as soon as we can reach the last index

#### Key Insights:
- **Range-based approach**: Track current range and farthest reachable position
- **Jump timing**: Only jump when we reach the end of current range
- **Farthest update**: Always update to the maximum reachable position
- **Early termination**: Return immediately when last index is reachable

#### Edge Cases Handled:
- Single element: Returns 0 (already at last index)
- All zeros except last: Returns 1 (must jump directly)
- All non-zeros: Returns optimal number of jumps
- Guaranteed reachability: Problem states we can always reach the last index

#### Related Problems:
- Jump Game (can we reach the last index?)
- Jump Game III (jump to zero)
- Jump Game IV (jump to equal values)
- Jump Game V (jump with restrictions)

#### Pattern Recognition:
This problem demonstrates the **Greedy Algorithm** pattern with **Range Tracking**:
- **Range-based processing**: Process elements in ranges rather than individually
- **Farthest reach tracking**: Always maintain the maximum reachable position
- **Jump optimization**: Only jump when necessary (end of current range)
- **Early termination**: Stop as soon as the goal is reached

#### Comparison with Jump Game:
- **Jump Game**: Can we reach the last index? (Boolean)
- **Jump Game II**: What's the minimum number of jumps? (Integer)
- **Strategy**: Both use greedy approaches but with different goals
- **Complexity**: Both O(n) time, O(1) space
