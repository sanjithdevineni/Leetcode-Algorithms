# [Problem Name]

### Difficulty: [Easy/Medium/Hard]

[Problem description from LeetCode]

### Example 1:

Input: [input example]

Output: [output example]

### Example 2:

Input: [input example]

Output: [output example]

### Constraints:

[Constraints from LeetCode]

### Follow up:

[Any follow-up questions]

## Solution:

### Most Efficient Approach: [DP Pattern Name]

[Brief description of the DP approach]

#### DP Pattern:
- **1D DP**: [If applicable]
- **2D DP**: [If applicable]
- **State Definition**: [What dp[i] or dp[i][j] represents]
- **Transition**: [How to compute current state from previous states]

#### Algorithm Steps:
1. **Define State**: [What each dp state represents]
2. **Base Cases**: [Initial values]
3. **Transition**: [How to fill dp table]
4. **Return**: [Final answer]

#### Pseudo-code:
```python
def solve_problem(input_data):
    n = len(input_data)
    
    # Initialize DP array/table
    dp = [base case values]
    
    # Fill DP table
    for i in range(1, n):
        dp[i] = transition(dp, i, input_data)
    
    return dp[n-1]  # or appropriate final state
```

#### State Transition:
```
dp[i] = [formula using previous states]
```

#### Visual Example:
```
[DP table visualization if helpful and simple]
```

#### Complexity Analysis:
- **Time Complexity**: O([complexity]) - [explanation]
- **Space Complexity**: O([complexity]) - [explanation]
- **Space Optimization**: [If applicable, mention space optimization]

#### Why This Works:
[Explanation of optimal substructure and overlapping subproblems]

#### Edge Cases Handled:
- [Edge case 1]
- [Edge case 2]
- [Edge case 3]

#### Related Problems:
- [Related problem 1]
- [Related problem 2]
