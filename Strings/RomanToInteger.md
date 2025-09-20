# Roman to Integer

### Difficulty: Easy

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000

For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

- I can be placed before V (5) and X (10) to make 4 and 9. 
- X can be placed before L (50) and C (100) to make 40 and 90. 
- C can be placed before D (500) and M (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.

### Example 1:
Input: s = "III"
Output: 3
Explanation: III = 3.

### Example 2:
Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.

### Example 3:
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

### Constraints:
- 1 <= s.length <= 15
- s contains only the characters ('I', 'V', 'X', 'L', 'C', 'D', 'M').
- It is guaranteed that s is a valid roman numeral in the range [1, 3999].

## Solution:

### Most Efficient Approach: Lookup Table with Conditional Logic

The key insight is to use a lookup table and check if current symbol should be subtracted based on the next symbol.

#### Algorithm Steps:
1. **Create lookup table**: Map Roman symbols to integer values
2. **Process each symbol**: Check if current < next symbol
3. **Apply logic**: Subtract if current < next, add otherwise
4. **Return result**: Sum of all processed values

#### Pseudo-code:
```python
def roman_to_int(s):
    m = {
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000
    }
    
    ans = 0
    
    for i in range(len(s)):
        if i < len(s) - 1 and m[s[i]] < m[s[i+1]]:
            ans -= m[s[i]]  # Subtraction case
        else:
            ans += m[s[i]]  # Addition case
    
    return ans
```

#### Visual Examples:

**Example 1: s = "III"**
```
i=0: 'I' vs 'I' → 1 >= 1 → add 1 → ans = 1
i=1: 'I' vs 'I' → 1 >= 1 → add 1 → ans = 2  
i=2: 'I' vs None → add 1 → ans = 3
Result: 3
```

**Example 2: s = "LVIII"**
```
i=0: 'L' vs 'V' → 50 >= 5 → add 50 → ans = 50
i=1: 'V' vs 'I' → 5 >= 1 → add 5 → ans = 55
i=2: 'I' vs 'I' → 1 >= 1 → add 1 → ans = 56
i=3: 'I' vs 'I' → 1 >= 1 → add 1 → ans = 57
i=4: 'I' vs None → add 1 → ans = 58
Result: 58
```

**Example 3: s = "MCMXCIV"**
```
i=0: 'M' vs 'C' → 1000 >= 100 → add 1000 → ans = 1000
i=1: 'C' vs 'M' → 100 < 1000 → subtract 100 → ans = 900
i=2: 'M' vs 'X' → 1000 >= 10 → add 1000 → ans = 1900
i=3: 'X' vs 'C' → 10 < 100 → subtract 10 → ans = 1890
i=4: 'C' vs 'I' → 100 >= 1 → add 100 → ans = 1990
i=5: 'I' vs 'V' → 1 < 5 → subtract 1 → ans = 1989
i=6: 'V' vs None → add 5 → ans = 1994
Result: 1994
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the string
- **Space Complexity**: O(1) - Fixed size lookup table

#### Why This Works:
1. **Lookup efficiency**: O(1) access to Roman symbol values
2. **Subtraction detection**: Compare current symbol with next symbol
3. **Single pass**: Process string in one iteration
4. **Correct handling**: Properly handles all subtraction cases (IV, IX, XL, XC, CD, CM)

#### Key Insights:
- **Subtraction rule**: Only when current symbol is less than next symbol
- **Lookup table**: Efficient mapping of symbols to values
- **Single pass**: Process each character exactly once
- **Edge cases**: Handle last character (no next character to compare)

#### Alternative Approach (Less Efficient):
```python
def roman_to_int_alternative(s):
    # Handle subtraction cases explicitly
    s = s.replace("IV", "IIII").replace("IX", "VIIII")
    s = s.replace("XL", "XXXX").replace("XC", "LXXXX") 
    s = s.replace("CD", "CCCC").replace("CM", "DCCCC")
    
    m = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
    return sum(m[char] for char in s)
```

#### Edge Cases Handled:
- Single character: Returns its direct value
- All addition: No subtraction cases (e.g., "III", "LVIII")
- All subtraction: Multiple subtraction cases (e.g., "IV", "IX", "CM")
- Mixed cases: Combination of addition and subtraction

#### Pattern Recognition:
This problem demonstrates the **Lookup Table with Conditional Logic** pattern:
- **Efficient lookup**: O(1) symbol-to-value mapping
- **Conditional processing**: Different logic based on context
- **Single pass**: Process input in one iteration
- **Simple logic**: Clear rules for addition vs subtraction

#### Mathematical Insight:
- **Roman numeral rules**: Standard conversion rules with subtraction cases
- **Lookahead logic**: Need to check next symbol for subtraction
- **Optimal approach**: Single pass with O(1) lookups
- **Constraint satisfaction**: Handle all valid Roman numeral patterns

#### Real-World Applications:
- **Date parsing**: Convert Roman numeral dates
- **Number systems**: Understanding different numeral representations
- **Text processing**: Parse historical documents
- **Educational tools**: Roman numeral learning systems

#### Code Variations:
```python
# Using dictionary with get method
def roman_to_int_variant(s):
    values = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
    total = 0
    prev = 0
    
    for char in reversed(s):
        curr = values[char]
        if curr < prev:
            total -= curr
        else:
            total += curr
        prev = curr
    
    return total

# Using enumerate for cleaner indexing
def roman_to_int_enumerate(s):
    m = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
    return sum(-m[s[i]] if i < len(s)-1 and m[s[i]] < m[s[i+1]] else m[s[i]] 
               for i in range(len(s)))
```

#### Performance Characteristics:
- **Time complexity**: O(n) - Single pass through string
- **Space complexity**: O(1) - Fixed size lookup table
- **Cache efficiency**: Good due to sequential string access
- **Memory usage**: Minimal with lookup table

#### Optimization Notes:
- **Lookup table**: Use dictionary for O(1) symbol access
- **Single pass**: Avoid multiple iterations over string
- **Conditional logic**: Simple comparison for subtraction detection
- **No extra space**: Process in place without additional arrays

#### Debugging Tips:
- **Test subtraction cases**: IV, IX, XL, XC, CD, CM
- **Test addition cases**: III, LVIII, MMXXI
- **Test mixed cases**: MCMXCIV, MCMLXXXVIII
- **Test edge cases**: Single characters, maximum values

#### Constraint Analysis:
- **String length**: 1 <= n <= 15 (small string, O(n) is optimal)
- **Valid characters**: Only Roman numeral symbols
- **Range guarantee**: 1 <= result <= 3999 (no overflow concerns)
- **Valid input**: Guaranteed valid Roman numeral

#### Algorithm Correctness:
1. **Symbol mapping**: Correct lookup table values
2. **Subtraction logic**: Proper detection of subtraction cases
3. **Addition logic**: Correct handling of normal cases
4. **Edge cases**: Proper handling of last character

#### Mathematical Properties:
- **Roman numeral system**: Standard historical numbering system
- **Subtraction rules**: Six specific cases where subtraction applies
- **Range limitation**: Maximum value of 3999 (MMMCMXCIX)
- **Unique representation**: Each number has unique Roman representation

#### Algorithm Elegance:
The solution demonstrates several clean techniques:
- **Lookup table**: Efficient symbol-to-value mapping
- **Conditional logic**: Simple subtraction detection
- **Single pass**: Optimal time complexity
- **Minimal space**: Constant space usage

#### Testing Strategy:
- **Subtraction cases**: Test all six subtraction patterns
- **Addition cases**: Test normal addition scenarios
- **Mixed cases**: Test combinations of addition and subtraction
- **Edge cases**: Test single characters and maximum values
- **Boundary cases**: Test minimum (1) and maximum (3999) values

#### Memory Analysis:
- **Lookup table**: O(1) space for 7 symbols
- **Variables**: O(1) space for result and loop variables
- **Input string**: O(n) space (not counted as extra)
- **Total space**: O(1) extra space

#### Time Analysis:
- **Single pass**: O(n) time for string processing
- **Lookup operations**: O(1) time per symbol
- **Comparison operations**: O(1) time per symbol
- **Total time**: O(n) overall

#### Optimization Trade-offs:
- **Time vs Space**: Single pass vs multiple passes
- **Simplicity vs Efficiency**: Clear logic vs optimized operations
- **Readability vs Performance**: Understandable code vs fast code
- **Memory vs Speed**: Extra space vs computation time

#### Advanced Optimizations:
- **Bit manipulation**: Could use bit operations for certain cases
- **Lookup optimization**: Cache frequently used values
- **String preprocessing**: Could preprocess subtraction cases
- **Parallel processing**: Could parallelize for very long strings (though not applicable here)

#### Mathematical Insight:
- **Roman numeral conversion**: Classic problem in computer science
- **Subtraction detection**: Key insight is comparing with next symbol
- **Optimal approach**: Single pass with constant space
- **Historical context**: Understanding ancient numbering systems

#### Algorithm Correctness Proof:
1. **Base case**: Single character correctly returns its value
2. **Subtraction case**: When current < next, subtraction is correct
3. **Addition case**: When current >= next, addition is correct
4. **Induction**: Each step maintains correctness of running sum

#### Pattern Applications:
This **Lookup Table with Conditional Logic** pattern is useful for:
- **Symbol conversion**: Converting between different symbol systems
- **Text processing**: Parsing structured text with rules
- **Data validation**: Checking format compliance
- **Language processing**: Handling different writing systems

#### Real-World Examples:
- **Date parsing**: Converting Roman numeral dates to integers
- **Historical documents**: Processing ancient texts with Roman numerals
- **Educational software**: Teaching Roman numeral conversion
- **Number systems**: Understanding different numeral representations

#### Code Quality:
The solution demonstrates excellent code quality:
- **Readability**: Clear variable names and logic
- **Efficiency**: Optimal time and space complexity
- **Correctness**: Handles all edge cases properly
- **Maintainability**: Easy to understand and modify

#### Performance Metrics:
- **Time complexity**: O(n) - optimal for this problem
- **Space complexity**: O(1) - minimal extra space
- **Scalability**: Handles maximum input size efficiently
- **Reliability**: Handles all valid input cases

#### Educational Value:
This problem teaches important concepts:
- **Lookup tables**: Efficient data structure usage
- **Conditional logic**: Making decisions based on context
- **String processing**: Iterating through character sequences
- **Historical context**: Understanding ancient numbering systems
