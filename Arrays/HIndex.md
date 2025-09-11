# H-Index

### Difficulty: Medium

Given an array of integers `citations` where `citations[i]` is the number of citations a researcher received for their `i`th paper, return the researcher's h-index.

According to the definition of h-index on Wikipedia: The h-index is defined as the maximum value of h such that the given researcher has published at least h papers that have each been cited at least h times.

### Example 1:

Input: citations = [3,0,6,1,5]

Output: 3

Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively.
Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3.

### Example 2:

Input: citations = [1,3,1]

Output: 1

### Constraints:

- n == citations.length
- 1 <= n <= 5000
- 0 <= citations[i] <= 1000

## Solution:

### Most Efficient Approach: Sorting and Counting

The key insight is to sort citations in descending order and count papers that meet the h-index criteria.

#### Algorithm Steps:
1. **Sort**: Sort citations in descending order
2. **Count**: For each position i, check if citations[i] >= i+1
3. **Accumulate**: Count how many papers meet the criteria
4. **Return**: The count is the h-index

#### Pseudo-code:
```python
def h_index(citations):
    citations.sort(reverse=True)  # Sort in descending order
    max_h = 0
    
    for i in range(len(citations)):
        if citations[i] >= i + 1:  # Check if paper i+1 has at least i+1 citations
            max_h += 1
        else:
            break  # No more papers can meet the criteria
    
    return max_h
```

#### Visual Example 1:
```
citations = [3,0,6,1,5]
After sorting: [6,5,3,1,0]

Position 0: citations[0] = 6, i+1 = 1
           6 >= 1? Yes → max_h = 1

Position 1: citations[1] = 5, i+1 = 2
           5 >= 2? Yes → max_h = 2

Position 2: citations[2] = 3, i+1 = 3
           3 >= 3? Yes → max_h = 3

Position 3: citations[3] = 1, i+1 = 4
           1 >= 4? No → break

Result: 3
```

#### Visual Example 2:
```
citations = [1,3,1]
After sorting: [3,1,1]

Position 0: citations[0] = 3, i+1 = 1
           3 >= 1? Yes → max_h = 1

Position 1: citations[1] = 1, i+1 = 2
           1 >= 2? No → break

Result: 1
```

#### Alternative Approach (Bucket Sort):
```python
def h_index_bucket(citations):
    n = len(citations)
    buckets = [0] * (n + 1)
    
    # Count papers with each citation count
    for citation in citations:
        if citation >= n:
            buckets[n] += 1
        else:
            buckets[citation] += 1
    
    # Find h-index
    papers = 0
    for i in range(n, -1, -1):
        papers += buckets[i]
        if papers >= i:
            return i
    
    return 0
```

#### Complexity Analysis:
- **Time Complexity**: O(n log n) - Due to sorting
- **Space Complexity**: O(1) - Only using one variable (excluding input)
- **In-place**: Modifies the input array (sorting)

#### Why This Works:
1. **Sorting insight**: After sorting in descending order, we can check each position
2. **Position mapping**: Position i corresponds to the (i+1)th most cited paper
3. **Criteria check**: citations[i] >= i+1 means paper i+1 has at least i+1 citations
4. **Greedy approach**: We count papers in order of citation count

#### Key Insights:
- **H-index definition**: Maximum h such that at least h papers have at least h citations
- **Sorting requirement**: Must sort to check papers in order of citation count
- **Position mapping**: Position i represents the (i+1)th most cited paper
- **Early termination**: Can break when criteria is no longer met

#### Edge Cases Handled:
- Single paper: Returns 1 if citation >= 1, 0 otherwise
- All zeros: Returns 0 (no papers have citations)
- All same citations: Returns min(citation_count, paper_count)
- Increasing citations: Returns the maximum possible h-index

#### Related Problems:
- H-Index II (sorted citations array)
- Sort Colors (Dutch National Flag)
- Top K Frequent Elements
- Kth Largest Element in an Array

#### Pattern Recognition:
This problem demonstrates the **Sorting and Counting** pattern:
- **Sorting first**: Arrange elements in a specific order
- **Position-based logic**: Use position to determine criteria
- **Counting with conditions**: Count elements that meet specific criteria
- **Early termination**: Stop when criteria can no longer be met

#### H-Index Intuition:
The h-index is a measure of both the productivity and citation impact of a researcher:
- **Productivity**: Number of papers published
- **Impact**: Number of citations received
- **Balance**: H-index balances both factors
- **Example**: H-index of 3 means 3 papers with at least 3 citations each

#### Mathematical Insight:
For a sorted array in descending order:
- Position 0: 1st most cited paper
- Position 1: 2nd most cited paper
- Position i: (i+1)th most cited paper
- H-index = maximum i such that citations[i] >= i+1
