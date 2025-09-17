# Candy

### Difficulty: Hard

There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.

You are giving candies to these children subjected to the following requirements:
- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return the minimum number of candies you need to have to distribute the candies to the children.

### Example 1:
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

### Example 2:
Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.

### Constraints:
- n == ratings.length
- 1 <= n <= 2 * 10^4
- 0 <= ratings[i] <= 2 * 10^4

## Solution:

### Most Efficient Approach: Two-Pass Greedy

The key insight is to use two separate passes to handle left and right neighbor constraints independently.

#### Algorithm Steps:
1. **Initialize**: Give each child 1 candy
2. **Left-to-right pass**: If rating[i] > rating[i-1], give more candy than left neighbor
3. **Right-to-left pass**: If rating[i] > rating[i+1], ensure more candy than right neighbor
4. **Return**: Sum of all candies

#### Pseudo-code:
```python
def candy(ratings):
    n = len(ratings)
    candies = [1] * n
    
    if n == 1:
        return 1
    
    # Left-to-right pass
    for i in range(1, n):
        if ratings[i] > ratings[i-1]:
            candies[i] = candies[i-1] + 1
    
    # Right-to-left pass
    for i in range(n-2, -1, -1):
        if ratings[i] > ratings[i+1]:
            if candies[i] <= candies[i+1]:
                candies[i] = candies[i+1] + 1
    
    return sum(candies)
```

#### Visual Example:
```
ratings = [1,0,2]

Step 1: Initialize candies = [1,1,1]

Step 2: Left-to-right pass
- i=1: ratings[1]=0 < ratings[0]=1, no change: [1,1,1]
- i=2: ratings[2]=2 > ratings[1]=0, candies[2] = candies[1] + 1 = 2: [1,1,2]

Step 3: Right-to-left pass  
- i=1: ratings[1]=0 < ratings[2]=2, no change: [1,1,2]
- i=0: ratings[0]=1 > ratings[1]=0, candies[0] = candies[1] + 1 = 2: [2,1,2]

Result: sum([2,1,2]) = 5
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Two passes through the array
- **Space Complexity**: O(n) - Array to store candy counts

#### Why This Works:
1. **Two-pass approach**: Separates left and right neighbor constraints
2. **Greedy strategy**: Give minimum necessary candies at each step
3. **Constraint satisfaction**: Ensures all rating requirements are met
4. **Optimal solution**: Produces minimum total candies

#### Key Insights:
- **Separation of concerns**: Handle left and right neighbors independently
- **Greedy optimality**: Local optimal choices lead to global optimum
- **Two-pass necessity**: Single pass cannot handle both directions optimally
- **Minimum guarantee**: Always give at least 1 candy per child

#### Edge Cases Handled:
- Single child: Returns 1
- All same ratings: Each gets 1 candy
- Strictly increasing: 1,2,3,...,n candies
- Strictly decreasing: Handled by right-to-left pass

#### Pattern Recognition:
This problem demonstrates the **Two-Pass Greedy** pattern:
- **Independent passes**: Handle different constraints separately
- **Greedy decisions**: Make locally optimal choices
- **Constraint satisfaction**: Ensure all requirements are met
- **Global optimum**: Combine local solutions for global solution
