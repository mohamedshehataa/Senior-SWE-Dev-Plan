# Day 3 - Contains Duplicate

**Date**: November 18, 2025
**Difficulty**: Easy
**Pattern**: Hash Set / Array
**LeetCode Link**: https://leetcode.com/problems/contains-duplicate/

---

## Problem Statement

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**
```
Input: nums = [1,2,3,1]
Output: true
Explanation: The element 1 occurs at index 0 and 3.
```

**Example 2:**
```
Input: nums = [1,2,3,4]
Output: false
Explanation: All elements are distinct.
```

**Example 3:**
```
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```

**Constraints:**
- 1 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9

---

## Initial Thoughts

This problem tests:
1. **Set operations** - Detecting duplicates efficiently
2. **Space-time trade-offs** - Multiple approaches with different complexities
3. **Hash-based data structures** - Using HashSet for O(1) lookups

**Key insight:** We need to detect if ANY element appears twice. As soon as we find a duplicate, we can return true immediately.

**Common mistake:** Sorting the array first changes the time complexity and isn't necessary for the optimal solution.

---

## Approach

### Approach 1: Brute Force (Nested Loops)
**Intuition:**
Compare every element with every other element to find duplicates.

**Algorithm:**
1. For each element i
2. Check all elements j > i
3. If nums[i] == nums[j], return true
4. If no duplicates found, return false

**Time Complexity**: O(n²) - nested loops
**Space Complexity**: O(1) - no extra space

```java
// Java Solution - Brute Force (NOT recommended)
class Solution {
    public boolean containsDuplicate(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] == nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

**Why this doesn't work well:**
- Too slow for large inputs (up to 10^5 elements)
- Time Limit Exceeded on LeetCode

---

### Approach 2: Sorting
**Intuition:**
If we sort the array, duplicates will be adjacent to each other.

**Algorithm:**
1. Sort the array
2. Check each element with its next neighbor
3. If nums[i] == nums[i+1], return true
4. If no adjacent duplicates, return false

**Time Complexity**: O(n log n) - sorting dominates
**Space Complexity**: O(1) or O(n) depending on sorting algorithm (Arrays.sort uses O(log n) for QuickSort)

```java
// Java Solution - Sorting
import java.util.Arrays;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        // Sort the array
        Arrays.sort(nums);

        // Check adjacent elements
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }

        return false;
    }
}
```

**Why this works but isn't optimal:**
- Sorting takes O(n log n) time
- We can do better with O(n) using a hash set

---

### Approach 3: Hash Set ✓ (Optimal)
**Intuition:**
Use a HashSet to track elements we've seen. If we encounter an element already in the set, we found a duplicate.

**Key insight:**
- HashSet add() returns false if element already exists
- We can check and add in one operation

**Algorithm:**
1. Create empty HashSet
2. For each number in array:
   - If number is already in set → duplicate found, return true
   - Otherwise, add number to set
3. If loop completes → no duplicates, return false

**Time Complexity**: O(n) - single pass through array
**Space Complexity**: O(n) - HashSet stores up to n elements

```java
// Java Solution - Hash Set (Optimal)
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> seen = new HashSet<>();

        for (int num : nums) {
            // If add returns false, element already exists
            if (!seen.add(num)) {
                return true;
            }
        }

        return false;
    }
}
```

**Alternative implementation (more explicit):**
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> seen = new HashSet<>();

        for (int num : nums) {
            // Check if we've seen this number before
            if (seen.contains(num)) {
                return true;
            }
            seen.add(num);
        }

        return false;
    }
}
```

**Why this is optimal:**
- O(n) time - process each element once
- O(1) average time for HashSet operations
- Early termination on first duplicate
- Clean, readable code

---

### Approach 4: One-Liner Using Stream API (Java 8+)
**Just for fun:**
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        return Arrays.stream(nums).distinct().count() != nums.length;
    }
}
```

**Explanation:**
- `distinct()` removes duplicates
- If distinct count ≠ original length → duplicates exist
- Time: O(n), Space: O(n)
- **Note:** Less efficient than HashSet approach due to stream overhead

---

## Complexity Analysis

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Brute Force | O(n²) | O(1) | TLE on large inputs |
| Sorting | O(n log n) | O(1) to O(n) | Modifies original array |
| Hash Set | O(n) | O(n) | **Optimal solution** |
| Stream API | O(n) | O(n) | Stream overhead |

---

## Edge Cases to Consider

- [x] Single element array - No duplicates possible, return false
- [x] Two element array - Same [1,1] → true, Different [1,2] → false
- [x] All elements same - [5,5,5,5] → true (duplicate found immediately)
- [x] All elements distinct - [1,2,3,4,5] → false
- [x] Duplicate at beginning - [1,1,2,3] → true (early termination)
- [x] Duplicate at end - [1,2,3,3] → still O(n) but worst case
- [x] Negative numbers - [-1,-2,-3,-1] → true
- [x] Large numbers - [1000000000, 1000000000] → true

---

## Test Cases

```java
// Test Case 1: Standard case with duplicate
Input: nums = [1,2,3,1]
Expected Output: true
Actual Output: true
Status: ✓

// Test Case 2: All distinct
Input: nums = [1,2,3,4]
Expected Output: false
Actual Output: false
Status: ✓

// Test Case 3: Multiple duplicates
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Expected Output: true
Actual Output: true
Status: ✓

// Test Case 4: Single element
Input: nums = [1]
Expected Output: false
Actual Output: false
Status: ✓

// Test Case 5: Two same elements
Input: nums = [1,1]
Expected Output: true
Actual Output: true
Status: ✓

// Test Case 6: Two different elements
Input: nums = [1,2]
Expected Output: false
Actual Output: false
Status: ✓

// Test Case 7: Negative numbers
Input: nums = [-1,-2,-3,-1]
Expected Output: true
Actual Output: true
Status: ✓

// Test Case 8: Large array, no duplicates
Input: nums = [1,2,3,4,5,6,7,8,9,10]
Expected Output: false
Actual Output: false
Status: ✓
```

---

## Walkthrough Example

Let's trace through `nums = [1,2,3,1]`:

```
HashSet approach:

Initial: seen = {}

i=0, num=1:
  seen.contains(1)? No
  seen.add(1)
  seen = {1}

i=1, num=2:
  seen.contains(2)? No
  seen.add(2)
  seen = {1, 2}

i=2, num=3:
  seen.contains(3)? No
  seen.add(3)
  seen = {1, 2, 3}

i=3, num=1:
  seen.contains(1)? Yes! ← Duplicate found
  return true ✓
```

---

## Why HashSet.add() Returns Boolean?

**Important Java Detail:**
```java
Set<Integer> set = new HashSet<>();
boolean added = set.add(5);  // returns true (element added)
boolean added2 = set.add(5); // returns false (element already exists)
```

This is **perfect** for duplicate detection:
```java
if (!seen.add(num)) {
    // Element already existed (duplicate!)
    return true;
}
```

**One line does two things:**
1. Checks if element exists
2. Adds element if it doesn't exist

**This is cleaner than:**
```java
if (seen.contains(num)) {
    return true;
}
seen.add(num);
```

---

## Connection to Previous Problems

**Pattern Evolution:**

**Day 1 (Two Sum):**
- Used HashMap to store `value → index`
- Checked for complement
- Pattern: "Search for specific value"

**Day 2 (Best Time to Buy Stock):**
- Tracked minimum value
- No extra data structure needed (greedy)
- Pattern: "Track extremum"

**Day 3 (Contains Duplicate):**
- Use HashSet to track seen values
- Check for existence
- Pattern: "Detect duplicates"

**Common thread:** Hash-based structures optimize search from O(n) to O(1)!

---

## Key Takeaways

1. **HashSet is perfect for duplicate detection** - O(1) add/contains operations
2. **Early termination** - Return true as soon as first duplicate found
3. **Space-time trade-off** - Use O(n) space to achieve O(n) time
4. **Set.add() returns boolean** - Leverage this for cleaner code
5. **Hash-based patterns** - Third problem using hash structures (Two Sum used HashMap, this uses HashSet)

---

## Similar Problems

- **LeetCode #219** - Contains Duplicate II (Duplicate within k distance)
- **LeetCode #220** - Contains Duplicate III (Duplicate within value range)
- **LeetCode #1** - Two Sum (HashMap pattern, Day 1)
- **LeetCode #136** - Single Number (Find the unique element)
- **LeetCode #287** - Find the Duplicate Number (Array as HashMap)
- **LeetCode #442** - Find All Duplicates in an Array

---

## Interview Tips

**If asked this in an interview:**

1. **Clarify constraints:**
   - "Are there negative numbers?" (Yes, per constraints)
   - "Can I modify the original array?" (Affects whether you can sort)
   - "What's more important: time or space?" (Guides approach)

2. **Start with brute force:**
   - "The naive approach is nested loops, O(n²)"
   - "But we can do better..."

3. **Optimize step-by-step:**
   - "We could sort first, then check adjacent - O(n log n)"
   - "Or use HashSet for O(n) time and space"

4. **Explain trade-offs:**
   - "HashSet uses O(n) extra space but gives us O(n) time"
   - "If space is limited, sorting might be better"

5. **Code the optimal solution:**
   - Write the HashSet approach
   - Mention the `add()` boolean trick

6. **Test edge cases:**
   - Single element, all same, all different

---

## Review Checklist

- [ ] Problem understood completely
- [ ] All approaches documented
- [ ] Code runs without errors
- [ ] All test cases pass
- [ ] Edge cases handled
- [ ] Complexity analysis done
- [ ] Added to pattern list

---

## Notes for Review

**When to use this pattern:**
- Detecting duplicates in array/list
- Need O(n) time solution
- Have O(n) space available
- Order doesn't matter

**Common mistakes to avoid:**
- Using HashMap instead of HashSet (we don't need values, just keys)
- Not using early termination (checking entire array when duplicate found)
- Forgetting that HashSet.add() returns boolean

**Interview talking points:**
- "This is a classic hash set problem"
- "We trade O(n) space for O(n) time"
- "HashSet gives us O(1) average lookup"
- "We can return early on first duplicate"

---

## Pattern Category: Hash Set

**Pattern Name**: Duplicate Detection with HashSet
**Key Idea**: Track seen elements, check before adding
**Time**: O(n)
**Space**: O(n)

This is **Pattern #3** in your arsenal! 🎯

**Your Hash-based Patterns So Far:**
1. **HashMap (Two Sum)**: Find complement → O(n) time
2. **Greedy (Best Time to Buy Stock)**: Track min → O(1) space
3. **HashSet (Contains Duplicate)**: Detect duplicates → O(n) time/space

**You're building a strong foundation in hash-based optimizations!**

---

## 🔥 Pro Tip: When to Use HashMap vs HashSet

| Data Structure | Use When | Example Problems |
|---------------|----------|------------------|
| **HashMap** | Need to store key-value pairs | Two Sum (num → index) |
| **HashSet** | Only need to track existence | Contains Duplicate |
| **TreeMap** | Need sorted order + lookups | Sorted data with queries |
| **TreeSet** | Need sorted unique elements | Unique sorted collection |

**For this problem:** We only care if we've **seen** a number before, not where or how many times → **HashSet** is perfect!
