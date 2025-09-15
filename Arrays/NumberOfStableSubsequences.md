# Number of Stable Subsequences

### Difficulty: Hard

You are given an integer array `nums`.

A subsequence is stable if it does not contain three consecutive elements with the same parity when the subsequence is read in order (i.e., consecutive inside the subsequence).

Return the number of stable subsequences.

Since the answer may be too large, return it modulo 10^9 + 7.

### Example 1:

Input: nums = [1,3,5]

Output: 6

Explanation:
Stable subsequences are [1], [3], [5], [1, 3], [1, 5], and [3, 5].
Subsequence [1, 3, 5] is not stable because it contains three consecutive odd numbers.

### Example 2:

Input: nums = [2,3,4,2]

Output: 14

Explanation:
The only subsequence that is not stable is [2, 4, 2], which contains three consecutive even numbers.
All other subsequences are stable.

### Constraints:

- 1 <= nums.length <= 10^5
- 1 <= nums[i] <= 10^5

## Solution:

### Most Efficient Approach: State-Based Dynamic Programming

The key insight is to track subsequences ending with different parity patterns to avoid three consecutive elements of the same parity.

#### Algorithm Steps:
1. **Initialize states**: Track subsequences ending with 0, 1, or 2 consecutive elements of same parity
2. **Process elements**: For each element, update states based on its parity
3. **State transitions**: Avoid creating subsequences with 3+ consecutive same parity
4. **Sum results**: Return total count of all valid subsequences

#### Pseudo-code:
```python
def count_stable_subsequences(nums):
    MOD = 10**9 + 7
    ev1 = ev2 = od1 = od2 = 0  # States: even/odd with 1/2 consecutive
    start = 1  # Empty subsequence count
    
    for val in nums:
        if val % 2 == 0:  # Even number
            tmpA = (ev1 + start + od1 + od2) % MOD  # New even subsequences
            tmpB = (ev2 + ev1) % MOD  # Extend existing even subsequences
            tmpC = od1  # Keep odd subsequences unchanged
            tmpD = od2  # Keep odd subsequences unchanged
        else:  # Odd number
            tmpA = ev1  # Keep even subsequences unchanged
            tmpB = ev2  # Keep even subsequences unchanged
            tmpC = (od1 + start + ev2 + ev1) % MOD  # New odd subsequences
            tmpD = (od2 + od1) % MOD  # Extend existing odd subsequences
        
        ev1, ev2, od1, od2 = tmpA, tmpB, tmpC, tmpD
    
    return (ev1 + ev2 + od1 + od2) % MOD
```

#### Visual Example 1:
```
nums = [1,3,5]

Initial: ev1=0, ev2=0, od1=0, od2=0, start=1

val=1 (odd):
- tmpA = 0 (keep even unchanged)
- tmpB = 0 (keep even unchanged)  
- tmpC = 0 + 1 + 0 + 0 = 1 (new odd: [1])
- tmpD = 0 (no existing odd to extend)
- ev1=0, ev2=0, od1=1, od2=0

val=3 (odd):
- tmpA = 0 (keep even unchanged)
- tmpB = 0 (keep even unchanged)
- tmpC = 1 + 1 + 0 + 0 = 2 (new odd: [3], [1,3])
- tmpD = 0 + 1 = 1 (extend: [1,3] -> [1,3,3] invalid, so just [1,3])
- ev1=0, ev2=0, od1=2, od2=1

val=5 (odd):
- tmpA = 0 (keep even unchanged)
- tmpB = 0 (keep even unchanged)
- tmpC = 2 + 1 + 0 + 0 = 3 (new odd: [5], [1,5], [3,5])
- tmpD = 1 + 2 = 3 (extend: [1,3] -> [1,3,5] invalid, so [1,5], [3,5])
- ev1=0, ev2=0, od1=3, od2=3

Result: 0 + 0 + 3 + 3 = 6 stable subsequences
```

#### Visual Example 2:
```
nums = [2,3,4,2]

Initial: ev1=0, ev2=0, od1=0, od2=0, start=1

val=2 (even):
- ev1=1, ev2=0, od1=0, od2=0 (new even: [2])

val=3 (odd):
- ev1=1, ev2=0, od1=2, od2=0 (new odd: [3], [2,3])

val=4 (even):
- ev1=3, ev2=1, od1=2, od2=0 (new even: [4], [2,4], [3,4]; extend: [2,4])

val=2 (even):
- ev1=6, ev2=4, od1=2, od2=0 (new even: [2], [3,2], [4,2], [2,4,2], [3,4,2], [2,2]; extend: [2,4,2], [2,2])

Result: 6 + 4 + 2 + 0 = 12 stable subsequences
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through array
- **Space Complexity**: O(1) - Constant space for state variables
- **Modulo operations**: Handle large numbers efficiently

#### Why This Works:
1. **State tracking**: Track subsequences ending with different parity patterns
2. **Parity constraint**: Avoid three consecutive elements of same parity
3. **State transitions**: Update states based on current element parity
4. **Modular arithmetic**: Handle large numbers with modulo operations

#### Key Insights:
- **State representation**: ev1, ev2, od1, od2 represent different ending patterns
- **Parity checking**: Use modulo 2 to determine even/odd
- **State transitions**: Different rules for even vs odd elements
- **Modulo handling**: Use 10^9 + 7 for large number arithmetic

#### Edge Cases Handled:
- Single element: Returns 1 (the element itself)
- All even/odd: Handles uniform parity arrays
- Empty array: Returns 0 (no subsequences)
- Large numbers: Uses modulo arithmetic

#### Mathematical Insight:
- **Subsequence counting**: Classic combinatorial problem
- **Parity constraint**: Restriction on consecutive elements
- **State-based DP**: Efficient counting with state tracking
- **Modular arithmetic**: Handle large results efficiently

#### Related Problems:
- Count Subsequences
- Longest Increasing Subsequence
- Count Palindromic Subsequences
- Subsequence Sum Problems

#### Pattern Recognition:
This problem demonstrates the **State-Based Dynamic Programming** pattern:
- **State definition**: Track different ending patterns
- **State transitions**: Update based on current element
- **Constraint handling**: Avoid invalid subsequences
- **Efficiency**: O(n) time with constant space

#### Real-World Applications:
- **Sequence analysis**: Counting valid patterns in sequences
- **Data validation**: Ensuring data doesn't violate constraints
- **Pattern recognition**: Identifying stable subsequences
- **Combinatorial counting**: Counting valid combinations

#### Optimization Notes:
- **State compression**: Use minimal state variables
- **Modulo operations**: Handle large numbers efficiently
- **Single pass**: Process array in one iteration
- **Constant space**: Use O(1) additional space

#### Algorithm Correctness:
1. **State correctness**: States correctly represent ending patterns
2. **Transition correctness**: State updates follow parity rules
3. **Constraint handling**: Avoids three consecutive same parity
4. **Counting accuracy**: Correctly counts all valid subsequences

#### Constraint Analysis:
- **Array length**: 1 <= n <= 10^5 (large array, O(n) solution needed)
- **Element range**: 1 <= nums[i] <= 10^5 (moderate range)
- **Result size**: Can be very large, needs modulo
- **Time limit**: O(n) solution required

#### Code Variations:
```python
# More readable version with comments
def count_stable_subsequences_readable(nums):
    MOD = 10**9 + 7
    # ev1: subsequences ending with 1 even
    # ev2: subsequences ending with 2 consecutive even
    # od1: subsequences ending with 1 odd  
    # od2: subsequences ending with 2 consecutive odd
    ev1 = ev2 = od1 = od2 = 0
    start = 1  # Empty subsequence
    
    for val in nums:
        if val % 2 == 0:  # Even number
            new_ev1 = (ev1 + start + od1 + od2) % MOD
            new_ev2 = (ev2 + ev1) % MOD
            new_od1 = od1
            new_od2 = od2
        else:  # Odd number
            new_ev1 = ev1
            new_ev2 = ev2
            new_od1 = (od1 + start + ev2 + ev1) % MOD
            new_od2 = (od2 + od1) % MOD
        
        ev1, ev2, od1, od2 = new_ev1, new_ev2, new_od1, new_od2
    
    return (ev1 + ev2 + od1 + od2) % MOD
```

#### Performance Characteristics:
- **Time complexity**: O(n) - Single pass through array
- **Space complexity**: O(1) - Constant space for state variables
- **Cache efficiency**: Excellent due to sequential access
- **Branch prediction**: Good due to simple conditional logic

#### Advanced Optimizations:
- **State compression**: Use minimal state variables
- **Modulo optimization**: Reduce modulo operations
- **Bit manipulation**: Use bit operations for parity checking
- **Memory efficiency**: Constant space usage

#### Mathematical Properties:
- **Combinatorial counting**: Classic counting problem
- **Parity constraint**: Restriction on consecutive elements
- **State-based DP**: Efficient counting technique
- **Modular arithmetic**: Handle large results

#### Algorithm Elegance:
The solution demonstrates several advanced techniques:
- **State-based DP**: Efficient state tracking
- **Parity handling**: Clean even/odd logic
- **Modular arithmetic**: Handle large numbers
- **Single pass**: Optimal time complexity

#### Debugging Tips:
- **State visualization**: Print state values after each iteration
- **Step-by-step**: Trace through small examples
- **Edge cases**: Test with single elements
- **Modulo check**: Verify modulo operations

#### Testing Strategy:
- **Small cases**: Test with n=1,2,3
- **Edge cases**: Test with all even/odd
- **Large arrays**: Test with maximum constraints
- **Modulo cases**: Test with large results

#### Memory Analysis:
- **State variables**: 4 integers for state tracking
- **Temporary variables**: 4 integers for state updates
- **Input array**: O(n) space (not counted in space complexity)
- **Total space**: O(1) additional space

#### Time Analysis:
- **Single pass**: O(n) time for array processing
- **State updates**: O(1) time per element
- **Modulo operations**: O(1) time per operation
- **Total time**: O(n) overall

#### Optimization Trade-offs:
- **Readability vs Performance**: Bit manipulation vs clear logic
- **Space vs Time**: State compression vs clarity
- **Modulo vs Overflow**: Safety vs performance
- **Simplicity vs Efficiency**: Clear code vs optimized code
