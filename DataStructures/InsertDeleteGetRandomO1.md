# Insert Delete GetRandom O(1)

### Difficulty: Medium

Implement the `RandomizedSet` class:

- `RandomizedSet()` Initializes the RandomizedSet object.
- `bool insert(int val)` Inserts an item val into the set if not present. Returns true if the item was not present, false otherwise.
- `bool remove(int val)` Removes an item val from the set if present. Returns true if the item was present, false otherwise.
- `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the same probability of being returned.

You must implement the functions of the class such that each function works in average O(1) time complexity.

### Example 1:

Input: ["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
       [[], [1], [2], [2], [], [1], [2], []]

Output: [null, true, false, true, 2, true, false, 2]

Explanation:
- RandomizedSet randomizedSet = new RandomizedSet();
- randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
- randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
- randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
- randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
- randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
- randomizedSet.insert(2); // 2 was already in the set, so return false.
- randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.

### Constraints:

- -2^31 <= val <= 2^31 - 1
- At most 2 * 10^5 calls will be made to insert, remove, and getRandom.
- There will be at least one element in the data structure when getRandom is called.

## Solution:

### Most Efficient Approach: Hybrid Array + Hash Map

The key insight is to combine an array and a hash map to achieve O(1) operations for all three methods.

#### Data Structure Design:
- **Array (`lst`)**: Stores the actual values for O(1) random access
- **Hash Map (`idx_map`)**: Maps values to their indices in the array for O(1) lookups

#### Algorithm Steps:

##### Insert Operation:
1. Check if value already exists using hash map
2. If exists, return false
3. Add value to end of array
4. Update hash map with new index
5. Return true

##### Remove Operation:
1. Check if value exists using hash map
2. If not exists, return false
3. Get index of value to remove
4. Swap with last element in array
5. Update hash map for swapped element
6. Remove last element from array
7. Remove value from hash map
8. Return true

##### GetRandom Operation:
1. Generate random index in range [0, len(array))
2. Return element at that index

#### Pseudo-code:
```python
import random

class RandomizedSet:
    def __init__(self):
        self.lst = []           # Array for O(1) random access
        self.idx_map = {}       # Hash map for O(1) lookups
    
    def search(self, val):
        return val in self.idx_map
    
    def insert(self, val):
        if self.search(val):
            return False
        
        self.lst.append(val)                    # Add to end of array
        self.idx_map[val] = len(self.lst) - 1  # Update hash map
        return True
    
    def remove(self, val):
        if not self.search(val):
            return False
        
        idx = self.idx_map[val]           # Get index of value to remove
        self.lst[idx] = self.lst[-1]      # Swap with last element
        self.idx_map[self.lst[-1]] = idx  # Update hash map for swapped element
        self.lst.pop()                    # Remove last element
        del self.idx_map[val]             # Remove from hash map
        return True
    
    def getRandom(self):
        return random.choice(self.lst)    # O(1) random selection
```

#### Visual Example:
```
Initial: lst = [], idx_map = {}

Insert 1: lst = [1], idx_map = {1: 0}
Insert 2: lst = [1, 2], idx_map = {1: 0, 2: 1}
Insert 3: lst = [1, 2, 3], idx_map = {1: 0, 2: 1, 3: 2}

Remove 2:
  - idx = 1 (index of 2)
  - lst[1] = lst[-1] → lst = [1, 3, 3]
  - idx_map[3] = 1 → idx_map = {1: 0, 3: 1}
  - lst.pop() → lst = [1, 3]
  - del idx_map[2] → idx_map = {1: 0, 3: 1}

Final: lst = [1, 3], idx_map = {1: 0, 3: 1}
```

#### Complexity Analysis:
- **Insert**: O(1) average - Hash map lookup + array append
- **Remove**: O(1) average - Hash map lookup + swap + pop
- **GetRandom**: O(1) - Random index generation + array access
- **Space**: O(n) - Array and hash map storage

#### Why This Works:
1. **Array for random access**: O(1) random selection by index
2. **Hash map for lookups**: O(1) existence checks and index retrieval
3. **Swap-and-pop technique**: Maintains O(1) deletion by avoiding array shifting
4. **Index synchronization**: Hash map always reflects current array indices

#### Key Insights:
- **Hybrid approach**: Combines benefits of array and hash map
- **Swap-and-pop**: Avoids O(n) array shifting during deletion
- **Index tracking**: Hash map must be updated when array changes
- **Random access**: Array enables O(1) random selection

#### Edge Cases Handled:
- Empty set: getRandom() guaranteed to have at least one element
- Duplicate insertions: Returns false without modifying set
- Non-existent removals: Returns false without modifying set
- Single element: All operations work correctly

#### Alternative Approaches:
1. **Hash Set + Array**: Similar but less efficient for removals
2. **Linked List + Hash Map**: More complex, same complexity
3. **Tree-based**: O(log n) operations, not optimal

#### Related Problems:
- Insert Delete GetRandom O(1) - Duplicates Allowed
- Design HashSet
- Design HashMap
- LRU Cache

#### Pattern Recognition:
This problem demonstrates the **Data Structure Design** pattern:
- **Hybrid structures**: Combine multiple data structures for optimal performance
- **Trade-offs**: Balance between different operations' complexities
- **Index synchronization**: Maintain consistency between data structures
- **O(1) operations**: Design for constant time complexity

#### Real-World Applications:
- **Caching systems**: Random eviction policies
- **Load balancing**: Random server selection
- **Game development**: Random item selection
- **Sampling algorithms**: Random data selection

#### Design Principles:
1. **Separation of concerns**: Array for storage, hash map for lookups
2. **Consistency**: Always keep data structures synchronized
3. **Efficiency**: Optimize for the most common operations
4. **Simplicity**: Keep the design as simple as possible while meeting requirements
