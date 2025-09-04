# Majority Element

### Difficulty: Easy

Given an array `nums` of size `n`, return the majority element.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

### Example 1:

Input: nums = [3,2,3]

Output: 3

### Example 2:

Input: nums = [2,2,1,1,1,2,2]

Output: 2

### Constraints:

- n == nums.length
- 1 <= n <= 5 * 10^4
- -10^9 <= nums[i] <= 10^9

### Follow up:

Could you solve the problem in linear time and in O(1) space?

## Solution:

### Most Efficient Approach: Boyer-Moore Voting Algorithm

The Boyer-Moore Voting Algorithm finds the majority element in O(n) time and O(1) space by using a voting mechanism.

#### Algorithm Steps:
1. **Initialize**: Set candidate to first element, count to 0
2. **Vote**: For each element, if count is 0, set new candidate
3. **Count**: If element matches candidate, increment count; otherwise decrement
4. **Return**: The candidate is guaranteed to be the majority element

#### Pseudo-code:
```python
def majority_element(nums):
    candidate = nums[0]
    count = 0
    
    for num in nums:
        if count == 0:
            candidate = num
        count += 1 if num == candidate else -1
    
    return candidate
```

#### Visual Example:
```
Array: [2,2,1,1,1,2,2]
Step 1: candidate=2, count=1   (first element)
Step 2: candidate=2, count=2   (2 matches candidate)
Step 3: candidate=2, count=1   (1 doesn't match, decrement)
Step 4: candidate=2, count=0   (1 doesn't match, decrement)
Step 5: candidate=1, count=1   (count=0, new candidate=1)
Step 6: candidate=1, count=0   (2 doesn't match, decrement)
Step 7: candidate=2, count=1   (count=0, new candidate=2)
Result: 2 (majority element)
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(1) - Only using two variables
- **In-place**: Yes, no extra space needed

#### Why This Works:
The key insight is that the majority element will always "survive" the voting process because:
1. It appears more than n/2 times
2. Even if it gets "cancelled out" by other elements, it will always have a positive count at the end
3. The algorithm guarantees that if a majority element exists, it will be found

#### Edge Cases Handled:
- Single element array: Returns that element
- Array with all same elements: Returns that element
- Array with majority element at the end: Still works correctly
- Array with majority element in the middle: Voting mechanism handles it

#### Related Problems:
- Majority Element II (find elements appearing more than n/3 times)
- Find the Duplicate Number
- Find All Duplicates in an Array
