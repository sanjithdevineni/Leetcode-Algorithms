# Minimum Discards to Balance Inventory

### Difficulty: Medium

You are given two integers w and m, and an integer array arrivals, where arrivals[i] is the type of item arriving on day i (days are 1-indexed).

Items are managed according to the following rules:

1. Each arrival may be kept or discarded; an item may only be discarded on its arrival day.
2. For each day i, consider the window of days [max(1, i - w + 1), i] (the w most recent days up to day i):
3. For any such window, each item type may appear at most m times among kept arrivals whose arrival day lies in that window.
4. If keeping the arrival on day i would cause its type to appear more than m times in the window, that arrival must be discarded.

Return the minimum number of arrivals to be discarded so that every w-day window contains at most m occurrences of each type.

### Example 1:

Input: arrivals = [1,2,1,3,1], w = 4, m = 2

Output: 0

Explanation:
- On day 1, Item 1 arrives; the window contains no more than m occurrences of this type, so we keep it.
- On day 2, Item 2 arrives; the window of days 1 - 2 is fine.
- On day 3, Item 1 arrives, window [1, 2, 1] has item 1 twice, within limit.
- On day 4, Item 3 arrives, window [1, 2, 1, 3] has item 1 twice, allowed.
- On day 5, Item 1 arrives, window [2, 1, 3, 1] has item 1 twice, still valid.
- There are no discarded items, so return 0.

### Example 2:

Input: arrivals = [1,2,3,3,3,4], w = 3, m = 2

Output: 1

Explanation:
- On day 1, Item 1 arrives. We keep it.
- On day 2, Item 2 arrives, window [1, 2] is fine.
- On day 3, Item 3 arrives, window [1, 2, 3] has item 3 once.
- On day 4, Item 3 arrives, window [2, 3, 3] has item 3 twice, allowed.
- On day 5, Item 3 arrives, window [3, 3, 3] has item 3 three times, exceeds limit, so the arrival must be discarded.
- On day 6, Item 4 arrives, window [3, 4] is fine.
- Item 3 on day 5 is discarded, and this is the minimum number of arrivals to discard, so return 1.

### Constraints:

- 1 <= arrivals.length <= 10^5
- 1 <= arrivals[i] <= 10^5
- 1 <= w <= arrivals.length
- 1 <= m <= w

## Solution:

### Most Efficient Approach: Deque-Based Sliding Window

The key insight is to use deque to efficiently track kept arrival days for each item type and maintain sliding window constraints.

#### Algorithm Steps:
1. **Initialize tracking**: Use defaultdict(deque) to track kept days for each item type
2. **Process arrivals**: For each arrival, check if keeping would violate constraints
3. **Update window**: Remove days outside current w-day window
4. **Make decision**: Keep if within limit, discard otherwise
5. **Return result**: Total number of discards

#### Pseudo-code:
```python
def min_arrivals_to_discard(arrivals, w, m):
    kept = defaultdict(deque)  # Track kept days for each item type
    discards = 0
    
    for day, item_type in enumerate(arrivals, 1):
        q = kept[item_type]  # Get deque for this item type
        left = day - w + 1   # Left boundary of current window
        
        # Remove days outside current window
        while q and q[0] < left:
            q.popleft()
        
        # Check if keeping would exceed limit
        if len(q) >= m:
            discards += 1  # Must discard
        else:
            q.append(day)  # Keep this arrival
    
    return discards
```

#### Visual Example 1:
```
arrivals = [1,2,1,3,1], w = 4, m = 2

Day 1: Item 1 arrives
- kept[1] = [1], discards = 0
- Window [1]: Item 1 appears 1 time ≤ 2 ✓

Day 2: Item 2 arrives  
- kept[2] = [2], discards = 0
- Window [1,2]: Item 1 appears 1 time ≤ 2 ✓

Day 3: Item 1 arrives
- kept[1] = [1,3], discards = 0
- Window [1,2,3]: Item 1 appears 2 times ≤ 2 ✓

Day 4: Item 3 arrives
- kept[3] = [4], discards = 0
- Window [1,2,3,4]: Item 1 appears 2 times ≤ 2 ✓

Day 5: Item 1 arrives
- kept[1] = [3,5], discards = 0 (removed day 1)
- Window [2,3,4,5]: Item 1 appears 2 times ≤ 2 ✓

Result: 0 discards
```

#### Visual Example 2:
```
arrivals = [1,2,3,3,3,4], w = 3, m = 2

Day 1: Item 1 arrives
- kept[1] = [1], discards = 0

Day 2: Item 2 arrives
- kept[2] = [2], discards = 0

Day 3: Item 3 arrives
- kept[3] = [3], discards = 0
- Window [1,2,3]: Item 3 appears 1 time ≤ 2 ✓

Day 4: Item 3 arrives
- kept[3] = [3,4], discards = 0
- Window [2,3,4]: Item 3 appears 2 times ≤ 2 ✓

Day 5: Item 3 arrives
- kept[3] = [3,4], discards = 1 (day 5 discarded)
- Window [3,4,5]: Item 3 would appear 3 times > 2 ✗

Day 6: Item 4 arrives
- kept[4] = [6], discards = 1
- Window [4,5,6]: Item 3 appears 2 times ≤ 2 ✓

Result: 1 discard
```

#### Alternative Approach (Naive):
```python
def min_arrivals_naive(arrivals, w, m):
    discards = 0
    kept = []
    
    for day, item_type in enumerate(arrivals, 1):
        # Count occurrences in current window
        window_start = max(1, day - w + 1)
        count = sum(1 for d, t in kept if d >= window_start and t == item_type)
        
        if count >= m:
            discards += 1  # Must discard
        else:
            kept.append((day, item_type))  # Keep
    
    return discards
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass with amortized O(1) deque operations
- **Space Complexity**: O(n) - Store at most n kept days
- **Deque efficiency**: O(1) amortized for popleft and append operations

#### Why This Works:
1. **Deque optimization**: Efficiently maintain sliding window for each item type
2. **Greedy approach**: Always keep when possible, discard only when necessary
3. **Window maintenance**: Remove outdated days automatically
4. **Constraint checking**: Simple length check against limit m

#### Key Insights:
- **Deque usage**: Efficiently track kept days for each item type
- **Sliding window**: Maintain w-day window constraints
- **Greedy decision**: Keep when possible, discard when necessary
- **Amortized complexity**: Each day is added and removed at most once

#### Edge Cases Handled:
- Single item type: Handles uniform arrays correctly
- w equals array length: Handles full array window
- m equals 1: Handles strict uniqueness constraint
- Large arrays: Efficient O(n) solution

#### Mathematical Insight:
- **Sliding window**: Classic problem with deque optimization
- **Constraint satisfaction**: Ensure no window violates limit
- **Greedy optimality**: Greedy approach gives optimal solution
- **Amortized analysis**: Each element processed once

#### Related Problems:
- Sliding Window Maximum
- Longest Substring Without Repeating Characters
- Fruit Into Baskets
- Subarray Product Less Than K

#### Pattern Recognition:
This problem demonstrates the **Sliding Window with Deque** pattern:
- **Window maintenance**: Keep track of relevant elements
- **Deque optimization**: Efficient add/remove operations
- **Constraint checking**: Validate window constraints
- **Greedy decision**: Make optimal choices locally

#### Real-World Applications:
- **Inventory management**: Balance item quantities over time
- **Resource allocation**: Limit resource usage in time windows
- **Rate limiting**: Control request rates in time windows
- **Data streaming**: Process data with sliding window constraints

#### Optimization Notes:
- **Deque usage**: Use deque for O(1) operations
- **Window maintenance**: Remove outdated elements efficiently
- **Memory efficiency**: Only store necessary information
- **Single pass**: Process array in one iteration

#### Algorithm Correctness:
1. **Window correctness**: Maintains proper w-day window
2. **Constraint satisfaction**: Never exceeds m occurrences
3. **Greedy optimality**: Discards minimum necessary items
4. **Efficiency**: O(n) time with optimal space usage

#### Constraint Analysis:
- **Array length**: 1 <= n <= 10^5 (large array, O(n) solution needed)
- **Item types**: 1 <= arrivals[i] <= 10^5 (moderate range)
- **Window size**: 1 <= w <= n (can be full array)
- **Limit**: 1 <= m <= w (reasonable constraint)

#### Code Variations:
```python
# Using Counter for counting (less efficient)
from collections import Counter

def min_arrivals_counter(arrivals, w, m):
    discards = 0
    kept = []
    
    for day, item_type in enumerate(arrivals, 1):
        window_start = max(1, day - w + 1)
        # Remove outdated items
        kept = [(d, t) for d, t in kept if d >= window_start]
        
        # Count current type in window
        count = sum(1 for _, t in kept if t == item_type)
        
        if count >= m:
            discards += 1
        else:
            kept.append((day, item_type))
    
    return discards

# Using list with manual cleanup (less efficient)
def min_arrivals_list(arrivals, w, m):
    discards = 0
    kept = []
    
    for day, item_type in enumerate(arrivals, 1):
        window_start = max(1, day - w + 1)
        
        # Remove outdated items
        kept = [(d, t) for d, t in kept if d >= window_start]
        
        # Count current type
        count = sum(1 for _, t in kept if t == item_type)
        
        if count >= m:
            discards += 1
        else:
            kept.append((day, item_type))
    
    return discards
```

#### Performance Characteristics:
- **Time complexity**: O(n) - Single pass with amortized O(1) operations
- **Space complexity**: O(n) - Store kept days
- **Cache efficiency**: Good due to sequential access
- **Memory usage**: Efficient with deque

#### Advanced Optimizations:
- **Deque usage**: Use deque for O(1) operations
- **Window maintenance**: Efficient cleanup of outdated elements
- **Memory management**: Only store necessary information
- **Single pass**: Process in one iteration

#### Mathematical Properties:
- **Sliding window**: Classic problem with efficient solution
- **Constraint satisfaction**: Ensure window constraints
- **Greedy optimality**: Local optimal choices lead to global optimum
- **Amortized analysis**: Each element processed once

#### Algorithm Elegance:
The solution demonstrates several advanced techniques:
- **Deque optimization**: Efficient sliding window maintenance
- **Greedy approach**: Simple decision making
- **Constraint handling**: Clean validation logic
- **Single pass**: Optimal time complexity

#### Debugging Tips:
- **Window visualization**: Print window contents at each step
- **Step-by-step**: Trace through small examples
- **Edge cases**: Test with w=1, m=1
- **Deque state**: Print deque contents for each item type

#### Testing Strategy:
- **Small cases**: Test with n=1,2,3
- **Edge cases**: Test with w=1, m=1
- **Large arrays**: Test with maximum constraints
- **Uniform arrays**: Test with same item type

#### Memory Analysis:
- **Deque storage**: O(n) for kept days
- **Temporary variables**: O(1) additional space
- **Input array**: O(n) space (not counted)
- **Total space**: O(n) overall

#### Time Analysis:
- **Single pass**: O(n) time for array processing
- **Deque operations**: O(1) amortized per operation
- **Window maintenance**: O(1) amortized per day
- **Total time**: O(n) overall

#### Optimization Trade-offs:
- **Space vs Time**: Deque vs list with manual cleanup
- **Simplicity vs Efficiency**: Clear code vs optimized code
- **Memory vs Speed**: Store more vs compute more
- **Readability vs Performance**: Clear logic vs optimized operations
