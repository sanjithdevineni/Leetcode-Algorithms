# Minimum Operations to Transform String

### Difficulty: Medium

You are given a string `s` consisting only of lowercase English letters.

You can perform the following operation any number of times (including zero):

Choose any character `c` in the string and replace every occurrence of `c` with the next lowercase letter in the English alphabet.

Return the minimum number of operations required to transform `s` into a string consisting of only 'a' characters.

Note: Consider the alphabet as circular, thus 'a' comes after 'z'.

### Example 1:

Input: s = "yz"

Output: 2

Explanation: Change 'y' to 'z' to get "zz". Change 'z' to 'a' to get "aa". Thus, the answer is 2.

### Example 2:

Input: s = "a"

Output: 0

Explanation: The string "a" only consists of 'a' characters. Thus, the answer is 0.

### Constraints:

- 1 <= s.length <= 5 * 10^5
- s consists only of lowercase English letters.

## Solution:

### Most Efficient Approach: Maximum Distance Strategy

The key insight is that we need to find the character that requires the most operations to reach 'a', since operations must be performed sequentially.

#### Algorithm Steps:
1. **Calculate distance**: For each character, compute its distance to 'a' in the circular alphabet
2. **Find maximum**: The minimum operations needed is the maximum distance among all characters
3. **Return result**: This maximum distance represents the minimum operations required

#### Pseudo-code:
```python
def min_operations(s):
    ans = 0
    for c in s:
        dist = (26 - (ord(c) - ord('a'))) % 26  # Distance to 'a'
        ans = max(ans, dist)  # Take maximum distance
    return ans
```

#### Visual Example:
```
String: "yz"
Character 'y': distance = (26 - (ord('y') - ord('a'))) % 26 = (26 - 24) % 26 = 2
Character 'z': distance = (26 - (ord('z') - ord('a'))) % 26 = (26 - 25) % 26 = 1
Maximum distance: max(2, 1) = 2
Answer: 2 operations

String: "a"
Character 'a': distance = (26 - (ord('a') - ord('a'))) % 26 = (26 - 0) % 26 = 0
Maximum distance: 0
Answer: 0 operations
```

#### Complexity Analysis:
- **Time Complexity**: O(n) - Single pass through the string
- **Space Complexity**: O(1) - Only using constant extra space
- **In-place**: Yes, no modification of original string

#### Why This Works:
The key insight is that:
1. **Sequential operations**: We must perform operations one at a time, not in parallel
2. **Replace all occurrences**: When we choose a character, ALL instances get replaced
3. **Circular alphabet**: 'a' comes after 'z', so 'z' → 'a' in one operation
4. **Maximum constraint**: The character requiring the most operations determines the minimum total

#### Edge Cases Handled:
- String with all 'a' characters: Returns 0
- Single character string: Returns distance to 'a'
- String with duplicate characters: Only unique characters matter for distance calculation
- String with 'z': Distance is 1 (z → a in one operation)

#### Related Problems:
- Minimum Operations to Make Array Equal
- Minimum Number of Operations to Make String Sorted
- Minimum Operations to Convert String
