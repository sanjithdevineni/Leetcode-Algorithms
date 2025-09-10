# Best Time to Buy and Sell Stock

### Difficulty: Easy

You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

### Example 1:

Input: prices = [7,1,5,3,6,4]

Output: 5

Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.

Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

### Example 2:

Input: prices = [7,6,4,3,1]

Output: 0

Explanation: In this case, no transactions are done and the max profit = 0.

### Constraints:

- 1 <= prices.length <= 10^5
- 0 <= prices[i] <= 10^4

## Solution:

### Most Efficient Approach: Single Pass with Minimum Tracking

The key insight is to track the minimum price seen so far and calculate the maximum profit at each day.

#### Algorithm Steps:
1. **Initialize**: Set minimum price to infinity and maximum profit to 0
2. **Process each day**: For each price, check if it's a new minimum
3. **Calculate profit**: If not a new minimum, calculate potential profit
4. **Update maximum**: Keep track of the maximum profit found

#### Pseudo-code:
```python
def max_profit(prices):
    min_price = float('inf')  # Track minimum price seen
    max_profit = 0            # Track maximum profit found
    
    for price in prices:
        if price < min_price:
            min_price = price  # Update minimum price
        elif price - min_price > max_profit:
            max_profit = price - min_price  # Update maximum profit
    
    return max_profit
```

#### Visual Example:
```
Prices: [7,1,5,3,6,4]
Day 1: price=7, min_price=7, max_profit=0
Day 2: price=1, min_price=1, max_profit=0
Day 3: price=5, min_price=1, max_profit=4 (5-1)
Day 4: price=3, min_price=1, max_profit=4 (3-1=2, but 4 is better)
Day 5: price=6, min_price=1, max_profit=5 (6-1=5)
Day 6: price=4, min_price=1, max_profit=5 (4-1=3, but 5 is better)
Result: 5
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(1) - Only using two variables
- **In-place**: No modification of original array

#### Why This Works:
The key insight is that:
1. **Minimum tracking**: We only need to track the minimum price seen so far
2. **Greedy approach**: For each day, we check if selling today gives better profit
3. **Single transaction**: We can only buy once and sell once
4. **Future constraint**: We must buy before we sell

#### Edge Cases Handled:
- No profit possible: Returns 0
- Single day: Returns 0 (can't buy and sell on same day)
- Decreasing prices: Returns 0
- Increasing prices: Returns maximum possible profit

#### Related Problems:
- Best Time to Buy and Sell Stock II (multiple transactions)
- Best Time to Buy and Sell Stock III (at most 2 transactions)
- Best Time to Buy and Sell Stock with Cooldown
- Best Time to Buy and Sell Stock with Transaction Fee
