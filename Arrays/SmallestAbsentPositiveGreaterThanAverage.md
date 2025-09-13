# Smallest Absent Positive Greater Than Average

### Difficulty: Easy

You are given an integer array `nums`.

Return the smallest absent positive integer in `nums` such that it is strictly greater than the average of all elements in `nums`.

The average of an array is defined as the sum of all its elements divided by the number of elements.

### Example 1:

Input: nums = [3,5]

Output: 6

Explanation:
The average of nums is (3 + 5) / 2 = 8 / 2 = 4.
The smallest absent positive integer greater than 4 is 6.

### Example 2:

Input: nums = [-1,1,2]

Output: 3

Explanation:
The average of nums is (-1 + 1 + 2) / 3 = 2 / 3 = 0.667.
The smallest absent positive integer greater than 0.667 is 3.

### Example 3:

Input: nums = [4,-1]

Output: 2

Explanation:
The average of nums is (4 + (-1)) / 2 = 3 / 2 = 1.50.
The smallest absent positive integer greater than 1.50 is 2.

### Constraints:

- 1 <= nums.length <= 100
- -100 <= nums[i] <= 100

## Solution:

### Most Efficient Approach: Incremental Search

The key insight is to calculate the average and then find the smallest positive integer greater than the average that is not present in the array.

#### Algorithm Steps:
1. **Calculate average**: Sum all elements and divide by length
2. **Determine starting point**: If average < 0, start from 1; otherwise start from ceil(average)
3. **Incremental search**: Check each positive integer until we find one not in the array
4. **Return result**: The first absent positive integer greater than average

#### Pseudo-code:
```python
def smallest_absent(nums):
    avg = sum(nums) / len(nums)  # Calculate average
    
    if avg < 0:
        ans = 1  # Start from 1 if average is negative
    else:
        ans = int(avg) + 1  # Start from smallest integer > average
    
    while True:
        if ans in nums:
            ans += 1  # Increment if found in array
        else:
            return ans  # Return first absent positive integer
```

#### Visual Example 1:
```
nums = [3,5]
avg = (3 + 5) / 2 = 4.0
ans = int(4.0) + 1 = 5

Check ans = 5: 5 in [3,5]? Yes → ans = 6
Check ans = 6: 6 in [3,5]? No → return 6

Result: 6
```

#### Visual Example 2:
```
nums = [-1,1,2]
avg = (-1 + 1 + 2) / 3 = 0.667
ans = int(0.667) + 1 = 1

Check ans = 1: 1 in [-1,1,2]? Yes → ans = 2
Check ans = 2: 2 in [-1,1,2]? Yes → ans = 3
Check ans = 3: 3 in [-1,1,2]? No → return 3

Result: 3
```

#### Visual Example 3:
```
nums = [4,-1]
avg = (4 + (-1)) / 2 = 1.5
ans = int(1.5) + 1 = 2

Check ans = 2: 2 in [4,-1]? No → return 2

Result: 2
```

#### Alternative Approach (Using Set for O(1) Lookup):
```python
def smallest_absent_optimized(nums):
    avg = sum(nums) / len(nums)
    nums_set = set(nums)  # O(1) lookup
    
    if avg < 0:
        ans = 1
    else:
        ans = int(avg) + 1
    
    while ans in nums_set:
        ans += 1
    
    return ans
```

#### Complexity Analysis:
- **Time Complexity**: O(n + k) where k is the number of positive integers to check
- **Space Complexity**: O(1) - only using constant extra space
- **In-place**: No modification of original array

#### Why This Works:
1. **Average calculation**: Correctly computes the mathematical average
2. **Starting point**: Chooses appropriate starting point based on average
3. **Incremental search**: Systematically checks each positive integer
4. **Absence check**: Verifies that the number is not in the array

#### Key Insights:
- **Average handling**: Handle both positive and negative averages correctly
- **Starting point**: Use ceil(average) + 1 for positive averages, 1 for negative
- **Incremental search**: Check consecutive positive integers until absent
- **Positive constraint**: Only consider positive integers as valid answers

#### Edge Cases Handled:
- Negative average: Start from 1 (smallest positive integer)
- Zero average: Start from 1
- All negative numbers: Start from 1
- All positive numbers: Start from appropriate positive integer
- Single element: Works correctly with any value

#### Mathematical Insight:
- **Average calculation**: sum(nums) / len(nums)
- **Ceiling function**: int(avg) + 1 gives smallest integer > avg
- **Positive constraint**: Only consider positive integers
- **Absence check**: Verify number is not in the original array

#### Related Problems:
- First Missing Positive
- Missing Number
- Find All Numbers Disappeared in an Array
- Kth Missing Positive Number

#### Pattern Recognition:
This problem demonstrates the **Mathematical Search** pattern:
- **Mathematical calculation**: Compute average of array elements
- **Incremental search**: Check consecutive positive integers
- **Absence verification**: Ensure number is not in original array
- **Constraint handling**: Only consider positive integers

#### Real-World Applications:
- **Data analysis**: Finding gaps in sequential data
- **Database systems**: Finding missing IDs in sequences
- **Statistics**: Identifying missing values in datasets
- **Game development**: Finding available player slots

#### Optimization Notes:
- **Set lookup**: Can use set for O(1) lookup instead of O(n) list search
- **Early termination**: Will always find a solution (guaranteed by problem constraints)
- **Memory efficient**: Only uses constant extra space

#### Algorithm Correctness:
1. **Average calculation**: Correctly computes mathematical average
2. **Starting point**: Chooses appropriate starting point based on average
3. **Search completeness**: Will find the smallest absent positive integer
4. **Positive constraint**: Only returns positive integers
5. **Absence guarantee**: Ensures returned number is not in original array

#### Constraint Analysis:
- **Array length**: 1 <= n <= 100 (small enough for O(n) operations)
- **Element range**: -100 <= nums[i] <= 100 (bounded range)
- **Guaranteed solution**: Problem guarantees a solution exists
