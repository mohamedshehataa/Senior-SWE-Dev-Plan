# Day 1 - Two Sum

**Date**: November 16, 2025
**Difficulty**: Easy
**Pattern**: Hash Table / Array
**LeetCode Link**: https://leetcode.com/problems/two-sum/

---

## Problem Statement

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

You can return the answer in any order.

**Example 1:**
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**
```
Input: nums = [3,3], target = 6
Output: [0,1]
```

**Constraints:**
- 2 <= nums.length <= 10^4
- -10^9 <= nums[i] <= 10^9
- -10^9 <= target <= 10^9
- Only one valid answer exists.

---

## Initial Thoughts

This is a classic problem that tests understanding of:
1. Hash tables for O(1) lookups
2. The complement pattern: if we're looking for a + b = target, then b = target - a
3. Trade-off between time and space complexity

First instinct: nested loops (brute force), but we can optimize with a hash map.

---

## Approach

### Approach 1: Brute Force (Not Recommended)
**Intuition**:
Check every pair of numbers to see if they sum to the target.

**Algorithm**:
1. Use two nested loops
2. For each pair (i, j) where i < j
3. Check if nums[i] + nums[j] == target
4. Return [i, j] if found

**Time Complexity**: O(n²) - nested loops
**Space Complexity**: O(1) - no extra space

```java
// Java Solution - Brute Force
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[] {i, j};
                }
            }
        }
        return new int[] {}; // Should never reach here per constraints
    }
}
```

**Why this doesn't work well**:
Too slow for large inputs. With n = 10,000, we'd do ~50 million comparisons.

---

### Approach 2: Hash Map (Optimized) ✓
**Intuition**:
Instead of checking every pair, we can store numbers we've seen in a hash map. For each number, we check if its complement (target - current number) already exists in the map.

**Algorithm**:
1. Create a HashMap to store number → index mapping
2. For each number in the array:
   - Calculate complement = target - current number
   - Check if complement exists in the map
   - If yes, return [map.get(complement), current index]
   - If no, add current number and index to map
3. Continue until solution found

**Time Complexity**: O(n) - single pass through array
**Space Complexity**: O(n) - hash map storage

```java
// Java Solution - Optimized
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        // Map to store: number -> index
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];

            // Check if complement exists in map
            if (map.containsKey(complement)) {
                return new int[] {map.get(complement), i};
            }

            // Add current number to map
            map.put(nums[i], i);
        }

        // Should never reach here per problem constraints
        throw new IllegalArgumentException("No solution found");
    }
}
```

**Why this works**:
- We only traverse the array once (O(n))
- Hash map lookups are O(1) on average
- We find the answer as soon as we see the complement
- Space trade-off is worth it for the time savings

---

## Complexity Analysis

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Brute Force | O(n²) | O(1) | Too slow for large inputs |
| Hash Map | O(n) | O(n) | **Optimal solution** |

---

## Edge Cases to Consider

- [x] Array with exactly 2 elements (minimum per constraints)
- [x] Negative numbers in array
- [x] Target is negative
- [x] Duplicate numbers (e.g., [3,3], target=6)
- [x] Zero in array
- [x] Large numbers (near 10^9)

---

## Test Cases

```java
// Test Case 1: Example from problem
Input: nums = [2,7,11,15], target = 9
Expected Output: [0,1]
Actual Output: [0,1]
Status: ✓

// Test Case 2: Different order
Input: nums = [3,2,4], target = 6
Expected Output: [1,2]
Actual Output: [1,2]
Status: ✓

// Test Case 3: Duplicates
Input: nums = [3,3], target = 6
Expected Output: [0,1]
Actual Output: [0,1]
Status: ✓

// Edge Case 1: Negative numbers
Input: nums = [-1,-2,-3,-4,-5], target = -8
Expected Output: [2,4]
Actual Output: [2,4]
Status: ✓

// Edge Case 2: With zero
Input: nums = [0,4,3,0], target = 0
Expected Output: [0,3]
Actual Output: [0,3]
Status: ✓
```

---

## Walkthrough Example

Let's trace through `nums = [2,7,11,15], target = 9`:

```
Iteration 0:
  i = 0, nums[i] = 2
  complement = 9 - 2 = 7
  map = {}
  7 not in map
  Add: map = {2: 0}

Iteration 1:
  i = 1, nums[i] = 7
  complement = 9 - 7 = 2
  map = {2: 0}
  2 IS in map! ✓
  Return [map.get(2), 1] = [0, 1]
```

---

## Key Takeaways

1. **Pattern Recognition**: This is the "complement pattern" - look for (target - current)
2. **Hash Map Power**: Trading O(n) space for O(1) lookup time is often worth it
3. **Single Pass**: We don't need to see all elements before finding the solution
4. **Interview Tip**: Always start by stating the brute force, then optimize
5. **This pattern appears in many problems**: 3Sum, 4Sum, Two Sum II, etc.

---

## Similar Problems

- **LeetCode #167** - Two Sum II (Input Array Is Sorted) - Use two pointers instead
- **LeetCode #15** - 3Sum - Extension to three numbers
- **LeetCode #18** - 4Sum - Extension to four numbers
- **LeetCode #454** - 4Sum II - Hash map with multiple arrays
- **LeetCode #1** - Two Sum (this problem)

---

## Time Spent

- Understanding: 5 mins
- Coding brute force: 5 mins
- Coding optimized: 10 mins
- Testing: 5 mins
- **Total: 25 mins**

---

## Review Checklist

- [x] Problem understood completely
- [x] All approaches documented
- [x] Code runs without errors
- [x] All test cases pass
- [x] Edge cases handled
- [x] Complexity analysis done
- [x] Added to pattern list

---

## Notes for Review

**When to use this pattern:**
- Looking for pairs/combinations that sum to a target
- Need O(n) time complexity
- Can afford O(n) space

**Common mistakes to avoid:**
- Using the same element twice (index i != index j)
- Not handling duplicates properly
- Forgetting to add current element to map AFTER checking for complement

**Alternative approaches:**
- If array is sorted: Two pointers (O(n) time, O(1) space)
- If we can modify array: Sort first, then two pointers

---

## Pattern Category: Hash Table

**Pattern Name**: Complement Search
**Key Idea**: For each element, check if (target - element) exists
**Time**: O(n)
**Space**: O(n)

This is now **Pattern #1** in your arsenal! 🎯
