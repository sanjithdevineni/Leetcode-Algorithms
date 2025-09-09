# Count Binary Palindromic Numbers

### Difficulty: Hard

You are given a non-negative integer `n`.

A non-negative integer is called binary-palindromic if its binary representation (written without leading zeros) reads the same forward and backward.

Return the number of integers `k` such that `0 <= k <= n` and the binary representation of `k` is a palindrome.

Note: The number 0 is considered binary-palindromic, and its representation is "0".

### Example 1:

Input: n = 9

Output: 6

Explanation: The integers k in the range [0, 9] whose binary representations are palindromes are:

0 → "0"

1 → "1"

3 → "11"

5 → "101"

7 → "111"

9 → "1001"

All other values in [0, 9] have non-palindromic binary forms. Therefore, the count is 6.

### Example 2:

Input: n = 0

Output: 1

Explanation: Since "0" is a palindrome, the count is 1.

### Constraints:

- 0 <= n <= 10^15

## Solution:

### Most Efficient Approach: Mathematical Pattern Recognition

The key insight is to count binary palindromes by length and use bit manipulation to efficiently count palindromes ≤ n.

#### Algorithm Steps:
1. **Handle edge case**: If n = 0, return 1
2. **Count shorter palindromes**: For each length k < n's binary length, add 2^((k+1)//2 - 1)
3. **Count same-length palindromes**: For palindromes of same length as n, count those ≤ n
4. **Check n itself**: Determine if n is a palindrome

#### Pseudo-code:
```python
def count_binary_palindromes(n):
    if n == 0: 
        return 1  # Edge case
    
    res = 1  # Count 0
    A = [int(i) for i in bin(n)[2:]]  # Binary representation as array
    n_len = len(A)  # Length of binary representation
    
    # Count palindromes of length < n's length
    for k in range(1, n_len):
        res += 1 << ((k + 1) // 2 - 1)
    
    # Count palindromes of same length as n
    half = (n_len + 1) // 2
    for i in range(1, half):
        if A[i] == 1:
            res += 1 << (half - i - 1)
    
    # Check if n itself is a palindrome
    res += A[:half][::-1] <= A[-half:]
    return res
```

#### Visual Example:
```
n = 9, binary = "1001"
Length = 4

Shorter palindromes:
- Length 1: 2^0 = 1 (just "1", "0" already counted)
- Length 2: 2^0 = 1 (just "11")
- Length 3: 2^1 = 2 ("101", "111")

Same-length palindromes (length 4):
- First half: "10", second half: "01"
- Compare "01" <= "10": True
- So n itself is a palindrome

Total: 1 + 1 + 1 + 2 + 1 = 6
```

#### Complexity Analysis:
- **Time Complexity**: O(log n) - We iterate through the binary representation length
- **Space Complexity**: O(log n) - We store the binary representation as an array
- **In-place**: No modification of original input

#### Why This Works:
The key insight is that:
1. **Binary palindromes have structure**: For length k, we can choose the first (k+1)//2 bits freely
2. **Length-based counting**: We count palindromes by their binary length
3. **Range constraint**: We only count palindromes ≤ n by comparing bit patterns
4. **Mathematical pattern**: For length k, there are 2^((k+1)//2 - 1) palindromes

#### Edge Cases Handled:
- n = 0: Returns 1 (only "0")
- n = 1: Returns 2 (0 and 1)
- Large n: Handles up to 10^15 efficiently
- Single bit: Handles n = 1 correctly

#### Related Problems:
- Palindrome Number (decimal palindromes)
- Valid Palindrome (string palindromes)
- Count Different Palindromic Subsequences
- Binary Watch (bit manipulation)
