# Maximize Sum of At Most K Distinct Elements

### Difficulty: Easy

You are given a positive integer array `nums` and an integer `k`.

Choose at most `k` elements from `nums` so that their sum is maximized. However, the chosen numbers must be distinct.

Return an array containing the chosen numbers in strictly descending order.

### Example 1:

Input: nums = [84,93,100,77,90], k = 3

Output: [100,93,90]

Explanation:
The maximum sum is 283, which is attained by choosing 93, 100 and 90. We rearrange them in strictly descending order as [100, 93, 90].

### Example 2:

Input: nums = [84,93,100,77,93], k = 3

Output: [100,93,84]

Explanation:
The maximum sum is 277, which is attained by choosing 84, 93 and 100. We rearrange them in strictly descending order as [100, 93, 84]. We cannot choose 93, 100 and 93 because the chosen numbers must be distinct.

### Example 3:

Input: nums = [1,1,1,2,2,2], k = 6

Output: [2,1]

Explanation:
The maximum sum is 3, which is attained by choosing 1 and 2. We rearrange them in strictly descending order as [2, 1].

### Constraints:

- 1 <= nums.length <= 100
- 1 <= nums[i] <= 10^9
- 1 <= k <= nums.length

## Solution:

### Most Efficient Approach: Greedy with Sorting

The key insight is to sort the array in descending order and select the first k distinct elements to maximize the sum.

#### Algorithm Steps:
1. **Sort**: Sort the array in descending order
2. **Select distinct**: Iterate through sorted array and select distinct elements
3. **Stop condition**: Stop when we have k elements or no more distinct elements
4. **Return**: Return selected elements in descending order

#### Pseudo-code:
```python
def max_k_distinct(nums, k):
    result = []
    nums.sort(reverse=True)  # Sort in descending order
    
    for num in nums:
        if num in result:
            continue  # Skip if already selected
        else:
            result.append(num)  # Add distinct element
            if len(result) == k:
                return result  # Stop when we have k elements
    
    return result  # Return all distinct elements found
```

#### Visual Example 1:
```
nums = [84,93,100,77,90], k = 3
After sorting: [100,93,90,84,77]

Iteration 1: num=100, not in result → result=[100]
Iteration 2: num=93, not in result → result=[100,93]
Iteration 3: num=90, not in result → result=[100,93,90]
len(result) == k → return [100,93,90]

Result: [100,93,90]
```

#### Visual Example 2:
```
nums = [84,93,100,77,93], k = 3
After sorting: [100,93,93,84,77]

Iteration 1: num=100, not in result → result=[100]
Iteration 2: num=93, not in result → result=[100,93]
Iteration 3: num=93, already in result → skip
Iteration 4: num=84, not in result → result=[100,93,84]
len(result) == k → return [100,93,84]

Result: [100,93,84]
```

#### Visual Example 3:
```
nums = [1,1,1,2,2,2], k = 6
After sorting: [2,2,2,1,1,1]

Iteration 1: num=2, not in result → result=[2]
Iteration 2: num=2, already in result → skip
Iteration 3: num=2, already in result → skip
Iteration 4: num=1, not in result → result=[2,1]
Iteration 5: num=1, already in result → skip
Iteration 6: num=1, already in result → skip

Result: [2,1]
```

#### Alternative Approach (Using Set for O(1) Lookup):
```python
def max_k_distinct_optimized(nums, k):
    result = []
    seen = set()  # O(1) lookup for distinct check
    nums.sort(reverse=True)
    
    for num in nums:
        if num not in seen:
            result.append(num)
            seen.add(num)
            if len(result) == k:
                return result
    
    return result
```

#### Complexity Analysis:
- **Time Complexity**: O(n log n) - Due to sorting
- **Space Complexity**: O(k) - For storing result array
- **In-place**: Modifies the input array (sorting)

#### Why This Works:
1. **Greedy choice**: Always choose the largest available distinct element
2. **Sorting insight**: Sorting in descending order ensures we consider largest elements first
3. **Distinct constraint**: Skip elements that are already selected
4. **Optimal substructure**: Choosing largest distinct elements maximizes sum

#### Key Insights:
- **Sorting requirement**: Must sort to consider largest elements first
- **Distinct constraint**: Use set or list to track selected elements
- **Greedy strategy**: Always choose the largest available distinct element
- **Early termination**: Stop when we have k elements

#### Edge Cases Handled:
- All elements are the same: Returns only one distinct element
- k equals array length: Returns all distinct elements
- k is 1: Returns the largest element
- Duplicate elements: Correctly handles distinct constraint

#### Mathematical Insight:
- **Sum maximization**: To maximize sum, choose largest possible elements
- **Distinct constraint**: Each element can be chosen at most once
- **Greedy optimality**: Greedy approach gives optimal solution
- **Sorting necessity**: Must sort to consider elements in descending order

#### Related Problems:
- Two Sum
- 3Sum
- Kth Largest Element in an Array
- Top K Frequent Elements

#### Pattern Recognition:
This problem demonstrates the **Greedy Algorithm** pattern with **Sorting**:
- **Sorting first**: Arrange elements in descending order
- **Greedy selection**: Choose largest available distinct elements
- **Constraint handling**: Ensure distinct elements only
- **Early termination**: Stop when k elements are selected

#### Real-World Applications:
- **Resource allocation**: Selecting most valuable distinct resources
- **Portfolio optimization**: Choosing best performing distinct stocks
- **Feature selection**: Selecting most important distinct features
- **Gaming**: Choosing most valuable distinct items

#### Optimization Notes:
- **Set lookup**: Use set for O(1) distinct check instead of O(k) list search
- **Early termination**: Stop as soon as k elements are selected
- **Memory efficient**: Only store selected elements, not all elements

#### Algorithm Correctness:
1. **Greedy choice**: Always choosing largest available distinct element
2. **Optimal substructure**: Optimal solution contains optimal solutions to subproblems
3. **Distinct constraint**: Correctly handles duplicate elements
4. **Sorting correctness**: Descending order ensures largest elements are considered first

#### Constraint Analysis:
- **Array length**: 1 <= n <= 100 (small enough for O(n log n) operations)
- **Element range**: 1 <= nums[i] <= 10^9 (large range, but sorting handles it)
- **K constraint**: 1 <= k <= n (k cannot exceed array length)
- **Result size**: At most k elements, at least 1 element

#### Code Variations:
```python
# Using list comprehension with set
def max_k_distinct_list_comp(nums, k):
    nums.sort(reverse=True)
    seen = set()
    return [num for num in nums if num not in seen and not seen.add(num) and len(seen) <= k]

# Using heapq for large datasets
import heapq
def max_k_distinct_heap(nums, k):
    unique_nums = list(set(nums))
    unique_nums.sort(reverse=True)
    return unique_nums[:k]

# Using Counter for frequency-based approach
from collections import Counter
def max_k_distinct_counter(nums, k):
    unique_nums = list(Counter(nums).keys())
    unique_nums.sort(reverse=True)
    return unique_nums[:k]
```

#### Performance Characteristics:
- **Time complexity**: O(n log n) - dominated by sorting
- **Space complexity**: O(k) - for result storage
- **Cache efficiency**: Good due to sequential access after sorting
- **Branch prediction**: Excellent due to simple conditional logic
