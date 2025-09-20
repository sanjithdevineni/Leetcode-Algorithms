# Integer to Roman

### Difficulty: Medium

Seven different symbols represent Roman numerals with the following values:

Symbol	Value
I	1
V	5
X	10
L	50
C	100
D	500
M	1000

Roman numerals are formed by appending the conversions of decimal place values from highest to lowest. Converting a decimal place value into a Roman numeral has the following rules:

1. If the value does not start with 4 or 9, select the symbol of the maximal value that can be subtracted from the input, append that symbol to the result, subtract its value, and convert the remainder to a Roman numeral.

2. If the value starts with 4 or 9 use the subtractive form representing one symbol subtracted from the following symbol, for example, 4 is 1 (I) less than 5 (V): IV and 9 is 1 (I) less than 10 (X): IX. Only the following subtractive forms are used: 4 (IV), 9 (IX), 40 (XL), 90 (XC), 400 (CD) and 900 (CM).

3. Only powers of 10 (I, X, C, M) can be appended consecutively at most 3 times to represent multiples of 10. You cannot append 5 (V), 50 (L), or 500 (D) multiple times. If you need to append a symbol 4 times use the subtractive form.

Given an integer, convert it to a Roman numeral.

### Example 1:
Input: num = 3749
Output: "MMMDCCXLIX"
Explanation:
- 3000 = MMM as 1000 (M) + 1000 (M) + 1000 (M)
- 700 = DCC as 500 (D) + 100 (C) + 100 (C)
- 40 = XL as 10 (X) less of 50 (L)
- 9 = IX as 1 (I) less of 10 (X)

### Example 2:
Input: num = 58
Output: "LVIII"
Explanation:
- 50 = L
- 8 = VIII

### Example 3:
Input: num = 1994
Output: "MCMXCIV"
Explanation:
- 1000 = M
- 900 = CM
- 90 = XC
- 4 = IV

### Constraints:
- 1 <= num <= 3999

## Solution:

### Most Efficient Approach: Greedy with Predefined Mappings

The key insight is to use predefined value-symbol pairs in descending order and greedily select the largest possible symbol.

#### Algorithm Steps:
1. **Create mapping**: Predefined value-symbol pairs in descending order
2. **Greedy selection**: For each value-symbol pair, use as many as possible
3. **Subtract value**: Remove used value from remaining number
4. **Concatenate**: Join all symbols to form final result

#### Pseudo-code:
```python
def int_to_roman(num):
    value_symbols = [
        (1000, 'M'), (900, 'CM'), (500, 'D'), (400, 'CD'),
        (100, 'C'), (90, 'XC'), (50, 'L'), (40, 'XL'), (10, 'X'),
        (9, 'IX'), (5, 'V'), (4, 'IV'), (1, 'I')
    ]
    
    res = []
    
    for value, symbol in value_symbols:
        if num == 0:
            break
        count = num // value  # How many times this symbol can be used
        res.append(symbol * count)  # Add symbol 'count' times
        num -= count * value  # Subtract used value
    
    return ''.join(res)
```

#### Visual Examples:

**Example 1: num = 3749**
```
Step 1: 3749 // 1000 = 3, add "MMM", num = 749
Step 2: 749 // 900 = 0, skip CM
Step 3: 749 // 500 = 1, add "D", num = 249
Step 4: 249 // 400 = 0, skip CD
Step 5: 249 // 100 = 2, add "CC", num = 49
Step 6: 49 // 90 = 0, skip XC
Step 7: 49 // 50 = 0, skip L
Step 8: 49 // 40 = 1, add "XL", num = 9
Step 9: 9 // 10 = 0, skip X
Step 10: 9 // 9 = 1, add "IX", num = 0
Result: "MMMDCCXLIX"
```

**Example 2: num = 58**
```
Step 1: 58 // 1000 = 0, skip M
Step 2: 58 // 900 = 0, skip CM
Step 3: 58 // 500 = 0, skip D
Step 4: 58 // 400 = 0, skip CD
Step 5: 58 // 100 = 0, skip C
Step 6: 58 // 90 = 0, skip XC
Step 7: 58 // 50 = 1, add "L", num = 8
Step 8: 8 // 40 = 0, skip XL
Step 9: 8 // 10 = 0, skip X
Step 10: 8 // 9 = 0, skip IX
Step 11: 8 // 5 = 1, add "V", num = 3
Step 12: 3 // 4 = 0, skip IV
Step 13: 3 // 1 = 3, add "III", num = 0
Result: "LVIII"
```

**Example 3: num = 1994**
```
Step 1: 1994 // 1000 = 1, add "M", num = 994
Step 2: 994 // 900 = 1, add "CM", num = 94
Step 3: 94 // 500 = 0, skip D
Step 4: 94 // 400 = 0, skip CD
Step 5: 94 // 100 = 0, skip C
Step 6: 94 // 90 = 1, add "XC", num = 4
Step 7: 4 // 50 = 0, skip L
Step 8: 4 // 40 = 0, skip XL
Step 9: 4 // 10 = 0, skip X
Step 10: 4 // 9 = 0, skip IX
Step 11: 4 // 5 = 0, skip V
Step 12: 4 // 4 = 1, add "IV", num = 0
Result: "MCMXCIV"
```

#### Complexity Analysis:
- **Time Complexity**: O(1) - Fixed number of predefined values (13 pairs)
- **Space Complexity**: O(1) - Fixed size result string (max 15 characters)

#### Why This Works:
1. **Predefined mapping**: Includes all subtractive forms (CM, CD, XC, XL, IX, IV)
2. **Greedy approach**: Always use largest possible symbol first
3. **Complete coverage**: All possible values covered by the mapping
4. **Efficient**: Constant time and space complexity

#### Key Insights:
- **Predefined pairs**: Include subtractive forms in descending order
- **Greedy selection**: Always choose largest possible symbol
- **Integer division**: Calculate how many times symbol can be used
- **Subtraction**: Remove used value from remaining number

#### Alternative Approach (Less Efficient):
```python
def int_to_roman_alternative(num):
    # Handle each place value separately
    thousands = ["", "M", "MM", "MMM"]
    hundreds = ["", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"]
    tens = ["", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"]
    ones = ["", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"]
    
    return (thousands[num // 1000] + 
            hundreds[num % 1000 // 100] + 
            tens[num % 100 // 10] + 
            ones[num % 10])
```

#### Edge Cases Handled:
- Minimum value: 1 → "I"
- Maximum value: 3999 → "MMMCMXCIX"
- Powers of 10: 10 → "X", 100 → "C", 1000 → "M"
- Subtractive forms: 4 → "IV", 9 → "IX", 40 → "XL", 90 → "XC", 400 → "CD", 900 → "CM"

#### Pattern Recognition:
This problem demonstrates the **Greedy with Predefined Mappings** pattern:
- **Predefined values**: Use precomputed value-symbol pairs
- **Greedy selection**: Always choose largest possible option
- **Complete coverage**: All cases handled by predefined mapping
- **Efficient lookup**: Constant time access to symbols

#### Mathematical Insight:
- **Roman numeral system**: Historical numbering system with specific rules
- **Subtractive forms**: Special cases for 4, 9, 40, 90, 400, 900
- **Greedy optimality**: Greedy approach gives correct result
- **Range limitation**: Maximum value of 3999

#### Real-World Applications:
- **Date formatting**: Convert years to Roman numerals
- **Numbering systems**: Display numbers in Roman format
- **Historical documents**: Generate Roman numeral representations
- **Educational tools**: Teaching Roman numeral conversion

#### Code Variations:
```python
# Using dictionary instead of list of tuples
def int_to_roman_dict(num):
    symbols = {1000: 'M', 900: 'CM', 500: 'D', 400: 'CD',
               100: 'C', 90: 'XC', 50: 'L', 40: 'XL', 10: 'X',
               9: 'IX', 5: 'V', 4: 'IV', 1: 'I'}
    
    result = []
    for value in sorted(symbols.keys(), reverse=True):
        count = num // value
        result.append(symbols[value] * count)
        num -= count * value
    
    return ''.join(result)

# Using while loop instead of for loop
def int_to_roman_while(num):
    values = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
    symbols = ['M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I']
    
    result = []
    i = 0
    while num > 0:
        if num >= values[i]:
            count = num // values[i]
            result.append(symbols[i] * count)
            num -= count * values[i]
        i += 1
    
    return ''.join(result)
```

#### Performance Characteristics:
- **Time complexity**: O(1) - Fixed number of iterations (13)
- **Space complexity**: O(1) - Fixed size result string
- **Cache efficiency**: Excellent due to sequential access
- **Memory usage**: Minimal with predefined mappings

#### Optimization Notes:
- **Predefined mapping**: Include all subtractive forms
- **Descending order**: Process largest values first
- **Greedy selection**: Always use maximum possible symbols
- **Early termination**: Stop when num reaches 0

#### Debugging Tips:
- **Test edge cases**: 1, 4, 9, 40, 90, 400, 900, 3999
- **Verify subtractive forms**: Ensure correct mapping
- **Check maximum**: Test with 3999
- **Trace execution**: Print intermediate values

#### Constraint Analysis:
- **Input range**: 1 <= num <= 3999 (small range, O(1) is optimal)
- **Output length**: Maximum 15 characters (MMMCMXCIX)
- **Symbol count**: 13 predefined value-symbol pairs
- **No overflow**: Input guaranteed to be valid

#### Algorithm Correctness:
1. **Mapping completeness**: All values covered by predefined pairs
2. **Greedy optimality**: Largest symbol first gives correct result
3. **Subtractive forms**: Proper handling of special cases
4. **Edge cases**: Correct handling of boundary values

#### Mathematical Properties:
- **Roman numeral rules**: Standard conversion rules with subtractive forms
- **Greedy optimality**: Greedy approach gives optimal solution
- **Range limitation**: Maximum value of 3999 (MMMCMXCIX)
- **Unique representation**: Each number has unique Roman representation

#### Algorithm Elegance:
The solution demonstrates several clean techniques:
- **Predefined mapping**: Efficient value-symbol lookup
- **Greedy approach**: Simple selection strategy
- **Constant complexity**: Optimal time and space usage
- **Complete coverage**: All cases handled systematically

#### Testing Strategy:
- **Edge cases**: Test minimum (1) and maximum (3999) values
- **Subtractive forms**: Test all six subtractive cases
- **Powers of 10**: Test 10, 100, 1000
- **Mixed cases**: Test combinations like 1994, 3749
- **Boundary cases**: Test values just above/below subtractive forms

#### Memory Analysis:
- **Predefined mapping**: O(1) space for 13 value-symbol pairs
- **Result list**: O(1) space for maximum 15 characters
- **Variables**: O(1) space for counters and indices
- **Total space**: O(1) overall

#### Time Analysis:
- **Fixed iterations**: O(1) time for 13 predefined pairs
- **Integer operations**: O(1) time for division and subtraction
- **String operations**: O(1) time for concatenation
- **Total time**: O(1) overall

#### Optimization Trade-offs:
- **Time vs Space**: Predefined mapping vs computed mapping
- **Simplicity vs Efficiency**: Clear logic vs optimized operations
- **Readability vs Performance**: Understandable code vs fast code
- **Memory vs Speed**: Extra space vs computation time

#### Advanced Optimizations:
- **Lookup tables**: Could use lookup tables for place values
- **Bit manipulation**: Could use bit operations for certain cases
- **String optimization**: Could optimize string concatenation
- **Memory pooling**: Could reuse string buffers

#### Mathematical Insight:
- **Roman numeral conversion**: Classic problem in computer science
- **Greedy optimality**: Greedy approach gives correct result
- **Subtractive forms**: Key insight is including special cases
- **Historical context**: Understanding ancient numbering systems

#### Algorithm Correctness Proof:
1. **Base case**: Single digit correctly converted
2. **Subtractive case**: Special forms (4, 9, 40, 90, 400, 900) handled
3. **Greedy case**: Largest symbol first maintains correctness
4. **Induction**: Each step maintains correctness of result

#### Pattern Applications:
This **Greedy with Predefined Mappings** pattern is useful for:
- **Symbol conversion**: Converting between different symbol systems
- **Number formatting**: Displaying numbers in specific formats
- **Text generation**: Creating formatted text output
- **Configuration mapping**: Mapping values to predefined options

#### Real-World Examples:
- **Date formatting**: Converting years to Roman numerals
- **Numbering systems**: Displaying numbers in Roman format
- **Historical documents**: Generating Roman numeral representations
- **Educational software**: Teaching Roman numeral conversion

#### Code Quality:
The solution demonstrates excellent code quality:
- **Readability**: Clear variable names and logic
- **Efficiency**: Optimal time and space complexity
- **Correctness**: Handles all edge cases properly
- **Maintainability**: Easy to understand and modify

#### Performance Metrics:
- **Time complexity**: O(1) - optimal for this problem
- **Space complexity**: O(1) - minimal extra space
- **Scalability**: Handles maximum input size efficiently
- **Reliability**: Handles all valid input cases

#### Educational Value:
This problem teaches important concepts:
- **Greedy algorithms**: Making locally optimal choices
- **Predefined mappings**: Using precomputed value pairs
- **String processing**: Building strings efficiently
- **Historical context**: Understanding ancient numbering systems
