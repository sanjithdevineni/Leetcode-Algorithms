# Gas Station

### Difficulty: Medium

There are `n` gas stations along a circular route, where the amount of gas at the `i`th station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `i`th station to its next `(i + 1)`th station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique.

### Example 1:

Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]

Output: 3

Explanation:
- Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
- Travel to station 4. Your tank = 4 - 1 + 5 = 8
- Travel to station 0. Your tank = 8 - 2 + 1 = 7
- Travel to station 1. Your tank = 7 - 3 + 2 = 6
- Travel to station 2. Your tank = 6 - 4 + 3 = 5
- Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
- Therefore, return 3 as the starting index.

### Example 2:

Input: gas = [2,3,4], cost = [3,4,3]

Output: -1

Explanation:
- You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
- Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
- Travel to station 0. Your tank = 4 - 3 + 2 = 3
- Travel to station 1. Your tank = 3 - 3 + 3 = 3
- You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
- Therefore, you can't travel around the circuit once no matter where you start.

### Constraints:

- n == gas.length == cost.length
- 1 <= n <= 10^5
- 0 <= gas[i], cost[i] <= 10^4
- The input is generated such that the answer is unique.

## Solution:

### Most Efficient Approach: Greedy with Gas Tank Simulation

The key insight is to use a greedy approach that simulates the gas tank and finds the starting point where gas never goes negative.

#### Algorithm Steps:
1. **Calculate differences**: For each station, calculate gas[i] - cost[i]
2. **Feasibility check**: If total gas < total cost, return -1 (impossible)
3. **Find starting point**: Simulate gas tank and find where it never goes negative
4. **Reset strategy**: When gas becomes negative, reset starting point to next station

#### Pseudo-code:
```python
def can_complete_circuit(gas, cost):
    n = len(gas)
    diff = [gas[i] - cost[i] for i in range(n)]
    
    # Feasibility check: total gas must >= total cost
    if sum(diff) < 0:
        return -1
    
    start_idx = 0
    current_gas = 0
    
    for i in range(n):
        current_gas += diff[i]
        if current_gas < 0:
            # Reset starting point to next station
            start_idx = (i + 1) % n
            current_gas = 0
    
    return start_idx
```

#### Visual Example 1 (Success):
```
gas = [1,2,3,4,5], cost = [3,4,5,1,2]
diff = [-2,-2,-2,3,3]

Feasibility check: sum(diff) = 0 >= 0 ✓

Simulation:
i=0: current_gas = -2, current_gas < 0 → start_idx = 1, current_gas = 0
i=1: current_gas = -2, current_gas < 0 → start_idx = 2, current_gas = 0
i=2: current_gas = -2, current_gas < 0 → start_idx = 3, current_gas = 0
i=3: current_gas = 3, current_gas >= 0 ✓
i=4: current_gas = 6, current_gas >= 0 ✓

Result: start_idx = 3
```

#### Visual Example 2 (Failure):
```
gas = [2,3,4], cost = [3,4,3]
diff = [-1,-1,1]

Feasibility check: sum(diff) = -1 < 0 ✗

Result: -1 (impossible)
```

#### Alternative Approach (Brute Force):
```python
def can_complete_circuit_brute_force(gas, cost):
    n = len(gas)
    
    for start in range(n):
        tank = 0
        for i in range(n):
            station = (start + i) % n
            tank += gas[station] - cost[station]
            if tank < 0:
                break
        else:
            return start
    
    return -1
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(n) - For storing differences array
- **In-place**: Can be optimized to O(1) space

#### Why This Works:
1. **Feasibility check**: If total gas < total cost, no solution exists
2. **Greedy choice**: If we can't reach station i from current start, we can't reach it from any previous start
3. **Reset strategy**: When gas becomes negative, reset to next station
4. **Uniqueness**: Problem guarantees unique solution if it exists

#### Key Insights:
- **Total gas >= total cost**: Necessary condition for solution existence
- **Greedy reset**: When gas becomes negative, previous starting points won't work
- **Single pass**: Only need to check each station once
- **Circular route**: Use modulo arithmetic for circular traversal

#### Edge Cases Handled:
- No solution: Returns -1 when total gas < total cost
- Single station: Works correctly with modulo arithmetic
- All stations have enough gas: Returns 0
- All stations have insufficient gas: Returns -1

#### Mathematical Insight:
- **Net gas**: gas[i] - cost[i] represents net gas gained at station i
- **Total feasibility**: sum(gas) >= sum(cost) is necessary and sufficient
- **Starting point**: First station where cumulative gas never goes negative

#### Related Problems:
- Jump Game (similar greedy approach)
- Best Time to Buy and Sell Stock (greedy optimization)
- Container With Most Water (two-pointer approach)
- Trapping Rain Water (prefix/suffix approach)

#### Pattern Recognition:
This problem demonstrates the **Greedy Algorithm** pattern with **Simulation**:
- **Greedy choice**: Reset starting point when gas becomes negative
- **Simulation**: Track gas tank state during traversal
- **Feasibility check**: Verify solution existence before simulation
- **Single pass**: Optimize to O(n) time complexity

#### Real-World Applications:
- **Route planning**: Finding optimal starting points for circular routes
- **Resource allocation**: Balancing supply and demand in circular systems
- **Game theory**: Finding winning strategies in circular games
- **Economics**: Market equilibrium in circular trade systems

#### Optimization Notes:
- **Space optimization**: Can avoid storing diff array by calculating on-the-fly
- **Early termination**: Can stop as soon as feasibility check fails
- **Modulo arithmetic**: Essential for circular route simulation

#### Algorithm Correctness:
1. **Feasibility**: If sum(gas) < sum(cost), no solution exists
2. **Completeness**: If solution exists, algorithm will find it
3. **Optimality**: Returns the unique starting point (guaranteed by problem)
4. **Efficiency**: O(n) time complexity with single pass
