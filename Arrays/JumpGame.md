# Jump Game

### Difficulty: Medium

You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

### Example 1:

Input: nums = [2,3,1,1,4]

Output: true

Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

### Example 2:

Input: nums = [3,2,1,0,4]

Output: false

Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

### Constraints:

- 1 <= nums.length <= 10^4
- 0 <= nums[i] <= 10^5

## Solution:

### Most Efficient Approach: Backwards Greedy

The key insight is to work backwards from the target (last index) and check if we can reach the starting position.

#### Algorithm Steps:
1. **Initialize**: Set target to the last index
2. **Work backwards**: Iterate from second-to-last index to first index
3. **Check reachability**: If current position can reach target, update target
4. **Final check**: If target becomes 0, we can reach the last index

#### Pseudo-code:
```python
def can_jump(nums):
    target = len(nums) - 1  # Start from last index
    
    # Work backwards from second-to-last index
    for i in range(len(nums) - 2, -1, -1):
        if nums[i] >= target - i:  # Can reach target from position i
            target = i  # Update target to current position
    
    return target == 0  # Can we reach from index 0?
```

#### Visual Example 1 (Success):
```
nums = [2,3,1,1,4]
Index: 0 1 2 3 4
Value: 2 3 1 1 4

Step 1: target = 4, i = 3
        nums[3] = 1, target - i = 4 - 3 = 1
        1 >= 1? Yes → target = 3

Step 2: target = 3, i = 2  
        nums[2] = 1, target - i = 3 - 2 = 1
        1 >= 1? Yes → target = 2

Step 3: target = 2, i = 1
        nums[1] = 3, target - i = 2 - 1 = 1
        3 >= 1? Yes → target = 1

Step 4: target = 1, i = 0
        nums[0] = 2, target - i = 1 - 0 = 1
        2 >= 1? Yes → target = 0

Result: target == 0? Yes → True
```

#### Visual Example 2 (Failure):
```
nums = [3,2,1,0,4]
Index: 0 1 2 3 4
Value: 3 2 1 0 4

Step 1: target = 4, i = 3
        nums[3] = 0, target - i = 4 - 3 = 1
        0 >= 1? No → target stays 4

Step 2: target = 4, i = 2
        nums[2] = 1, target - i = 4 - 2 = 2
        1 >= 2? No → target stays 4

Step 3: target = 4, i = 1
        nums[1] = 2, target - i = 4 - 1 = 3
        2 >= 3? No → target stays 4

Step 4: target = 4, i = 0
        nums[0] = 3, target - i = 4 - 0 = 4
        3 >= 4? No → target stays 4

Result: target == 0? No → False
```

#### Alternative Approach (Forward Greedy):
```python
def can_jump_forward(nums):
    max_reach = 0  # Maximum index we can reach
    
    for i in range(len(nums)):
        if i > max_reach:  # Can't reach current position
            return False
        max_reach = max(max_reach, i + nums[i])  # Update max reach
    
    return max_reach >= len(nums) - 1  # Can reach last index
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(1) - Only using one variable
- **In-place**: No modification of original array

#### Why This Works:
1. **Backwards approach**: If we can reach the target from position i, then i becomes our new target
2. **Greedy choice**: We always choose the furthest position that can reach the current target
3. **Optimal substructure**: If we can reach the last index, we can reach any intermediate target
4. **Single pass**: We only need to check each position once

#### Key Insights:
- **Reachability check**: `nums[i] >= target - i` means position i can reach target
- **Target update**: When position i can reach target, i becomes the new target
- **Final validation**: If target becomes 0, we can reach the last index from start

#### Edge Cases Handled:
- Single element: Returns true (already at last index)
- All zeros except last: Returns false (can't move)
- All non-zeros: Returns true (can always move forward)
- Zero at last position: Returns true (can reach it)

#### Related Problems:
- Jump Game II (minimum number of jumps)
- Jump Game III (jump to zero)
- Jump Game IV (jump to equal values)
- Jump Game V (jump with restrictions)

#### Pattern Recognition:
This problem demonstrates the **Greedy Algorithm** pattern where we make locally optimal choices that lead to a globally optimal solution. The backwards approach is particularly elegant because it reduces the problem to checking if we can reach the starting position from the target.
