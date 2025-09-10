# Best Time to Buy and Sell Stock II

### Difficulty: Medium

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `i`th day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.

### Example 1:

Input: prices = [7,1,5,3,6,4]

Output: 7

Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.

### Example 2:

Input: prices = [1,2,3,4,5]

Output: 4

Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.

### Example 3:

Input: prices = [7,6,4,3,1]

Output: 0

Explanation: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.

### Constraints:

- 1 <= prices.length <= 3 * 10^4
- 0 <= prices[i] <= 10^4

## Solution:

### Most Efficient Approach: Greedy with Immediate Selling

The key insight is to capture every profitable price increase by buying low and selling high whenever possible.

#### Algorithm Steps:
1. **Initialize**: Set profit to 0 and buy price to first day's price
2. **Process each day**: For each price, compare with current buy price
3. **Sell when profitable**: If price > buy price, sell immediately and add profit
4. **Update buy price**: Always update to current price after selling
5. **Buy when cheaper**: If price < buy price, update to lower buy price

#### Pseudo-code:
```python
def max_profit(prices):
    profit = 0
    buy_price = prices[0]
    
    for price in prices:
        if price == buy_price:
            continue  # No change, no action needed
        elif price < buy_price:
            buy_price = price  # Update to lower buy price
        else:
            profit += price - buy_price  # Sell immediately, add profit
            buy_price = price  # Update buy price to current price
    
    return profit
```

#### Visual Example:
```
Prices: [7,1,5,3,6,4]
Day 1: price=7, buy_price=7, profit=0 (no action)
Day 2: price=1, buy_price=1, profit=0 (update to lower price)
Day 3: price=5, buy_price=5, profit=4 (sell: 5-1=4)
Day 4: price=3, buy_price=3, profit=4 (update to lower price)
Day 5: price=6, buy_price=6, profit=7 (sell: 6-3=3, total=4+3=7)
Day 6: price=4, buy_price=4, profit=7 (update to lower price)
Result: 7
```

#### Alternative Approach (Sum of Positive Differences):
```python
def max_profit_alternative(prices):
    profit = 0
    for i in range(1, len(prices)):
        if prices[i] > prices[i-1]:
            profit += prices[i] - prices[i-1]
    return profit
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(1) - Only using two variables
- **In-place**: No modification of original array

#### Why This Works:
1. **Greedy strategy**: Always capture profitable price increases
2. **No holding penalty**: Can buy and sell on same day
3. **Optimal transactions**: Each profitable increase is captured exactly once
4. **Immediate selling**: No need to hold for better future prices

#### Key Differences from Stock I:
- **Stock I**: Single transaction (buy once, sell once)
- **Stock II**: Multiple transactions (buy/sell as many times as profitable)
- **Strategy**: Capture every price increase vs. finding single best opportunity

#### Edge Cases Handled:
- No profit possible: Returns 0
- Single day: Returns 0 (can't buy and sell on same day for profit)
- Decreasing prices: Returns 0
- Increasing prices: Returns maximum possible profit
- Flat prices: Returns 0

#### Related Problems:
- Best Time to Buy and Sell Stock (single transaction)
- Best Time to Buy and Sell Stock III (at most 2 transactions)
- Best Time to Buy and Sell Stock with Cooldown
- Best Time to Buy and Sell Stock with Transaction Fee
