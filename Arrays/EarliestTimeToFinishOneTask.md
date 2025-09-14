# Earliest Time to Finish One Task

### Difficulty: Easy

You are given a 2D integer array `tasks` where `tasks[i] = [si, ti]`.

Each `[si, ti]` in `tasks` represents a task with start time `si` that takes `ti` units of time to finish.

Return the earliest time at which at least one task is finished.

### Example 1:

Input: tasks = [[1,6],[2,3]]

Output: 5

Explanation:
The first task starts at time t = 1 and finishes at time 1 + 6 = 7. The second task finishes at time 2 + 3 = 5. You can finish one task at time 5.

### Example 2:

Input: tasks = [[100,100],[100,100],[100,100]]

Output: 200

Explanation:
All three tasks finish at time 100 + 100 = 200.

### Constraints:

- 1 <= tasks.length <= 100
- tasks[i] = [si, ti]
- 1 <= si, ti <= 100

## Solution:

### Most Efficient Approach: Direct Mathematical Calculation

The key insight is to calculate the finish time for each task and find the minimum.

#### Algorithm Steps:
1. **Calculate finish times**: For each task [si, ti], finish time = si + ti
2. **Find minimum**: Return the smallest finish time across all tasks
3. **One-liner**: Use min() with list comprehension for optimal solution

#### Pseudo-code:
```python
def earliest_time(tasks):
    return min(start + duration for start, duration in tasks)
```

#### Visual Example 1:
```
tasks = [[1,6],[2,3]]

Task 1: start=1, duration=6 → finish_time = 1 + 6 = 7
Task 2: start=2, duration=3 → finish_time = 2 + 3 = 5

Minimum finish time = min(7, 5) = 5

Result: 5
```

#### Visual Example 2:
```
tasks = [[100,100],[100,100],[100,100]]

Task 1: start=100, duration=100 → finish_time = 100 + 100 = 200
Task 2: start=100, duration=100 → finish_time = 100 + 100 = 200
Task 3: start=100, duration=100 → finish_time = 100 + 100 = 200

Minimum finish time = min(200, 200, 200) = 200

Result: 200
```

#### Alternative Approach (Explicit Loop):
```python
def earliest_time_explicit(tasks):
    min_finish_time = float('inf')
    
    for start, duration in tasks:
        finish_time = start + duration
        min_finish_time = min(min_finish_time, finish_time)
    
    return min_finish_time
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through all tasks
- **Space Complexity**: O(1) - Only using constant extra space
- **In-place**: No modification of original array

#### Why This Works:
1. **Mathematical insight**: Finish time = start time + duration
2. **Minimum property**: We want the earliest (smallest) finish time
3. **Independent tasks**: Each task's finish time is independent of others
4. **Optimal solution**: min() function finds the smallest value efficiently

#### Key Insights:
- **Simple calculation**: Finish time = start + duration
- **Minimum finding**: Use min() to find smallest finish time
- **One-liner solution**: List comprehension with min() is optimal
- **No complex logic**: Straightforward mathematical operation

#### Edge Cases Handled:
- Single task: Returns that task's finish time
- Multiple tasks with same finish time: Returns the common finish time
- Tasks starting at different times: Correctly calculates individual finish times
- Large start times: Handles large values correctly

#### Mathematical Insight:
- **Finish time formula**: finish_time[i] = start_time[i] + duration[i]
- **Minimum operation**: min(finish_times) gives earliest completion
- **Independence**: Each task's finish time is independent
- **Optimality**: No need to consider task ordering or dependencies

#### Related Problems:
- Meeting Rooms
- Task Scheduler
- Course Schedule
- Minimum Time to Complete All Tasks

#### Pattern Recognition:
This problem demonstrates the **Mathematical Optimization** pattern:
- **Direct calculation**: Simple mathematical formula for each element
- **Minimum finding**: Use min() to find optimal value
- **One-liner solution**: Concise implementation using built-in functions
- **No complex logic**: Straightforward mathematical operations

#### Real-World Applications:
- **Project management**: Finding earliest task completion
- **Scheduling systems**: Determining when first task finishes
- **Resource allocation**: Planning task completion times
- **Time management**: Optimizing task scheduling

#### Optimization Notes:
- **Built-in functions**: Use min() for optimal performance
- **List comprehension**: More efficient than explicit loops
- **Single pass**: Only need to iterate through tasks once
- **Constant space**: No additional data structures needed

#### Algorithm Correctness:
1. **Formula correctness**: finish_time = start + duration is mathematically correct
2. **Minimum property**: min() correctly finds the smallest value
3. **Completeness**: Considers all tasks in the calculation
4. **Optimality**: Returns the earliest possible finish time

#### Constraint Analysis:
- **Array length**: 1 <= n <= 100 (small enough for O(n) operations)
- **Start time range**: 1 <= si <= 100 (bounded range)
- **Duration range**: 1 <= ti <= 100 (bounded range)
- **Result range**: 2 <= result <= 200 (sum of start + duration)

#### Code Variations:
```python
# Using map function
def earliest_time_map(tasks):
    return min(map(lambda x: x[0] + x[1], tasks))

# Using reduce function
from functools import reduce
def earliest_time_reduce(tasks):
    return reduce(min, (start + duration for start, duration in tasks))

# Using numpy (if available)
import numpy as np
def earliest_time_numpy(tasks):
    return np.min([start + duration for start, duration in tasks])
```

#### Performance Characteristics:
- **Time complexity**: O(n) - linear in number of tasks
- **Space complexity**: O(1) - constant extra space
- **Cache efficiency**: Good due to sequential access
- **Branch prediction**: Excellent due to simple operations
