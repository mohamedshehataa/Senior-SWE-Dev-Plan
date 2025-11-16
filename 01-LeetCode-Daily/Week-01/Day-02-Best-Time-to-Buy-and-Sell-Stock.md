# Day 2 - Best Time to Buy and Sell Stock

**Date**: November 17, 2025
**Difficulty**: Easy
**Pattern**: Array / Single Pass
**LeetCode Link**: https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

---

## Problem Statement

You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return the **maximum profit** you can achieve from this transaction. If you cannot achieve any profit, return `0`.

**Example 1:**
```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

**Example 2:**
```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

**Constraints:**
- 1 <= prices.length <= 10^5
- 0 <= prices[i] <= 10^4

---

## Initial Thoughts

This problem tests:
1. **Array traversal** - Processing elements in order
2. **Tracking state** - Remembering the minimum price seen so far
3. **Optimization** - Finding maximum difference with constraint (buy before sell)

**Key insight:** You can't sell before you buy, so you need to track the minimum price encountered so far and calculate potential profit at each step.

**Common mistake:** Trying to find the max and min of the entire array - this doesn't work because min might come after max!

---

## Approach

### Approach 1: Brute Force (Not Recommended)
**Intuition**:
Check every possible buy-sell pair where buy day < sell day.

**Algorithm**:
1. For each day i (buy day)
2. For each day j > i (sell day)
3. Calculate profit = prices[j] - prices[i]
4. Track maximum profit

**Time Complexity**: O(n²) - nested loops
**Space Complexity**: O(1)

```java
// Java Solution - Brute Force
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;

        for (int i = 0; i < prices.length; i++) {
            for (int j = i + 1; j < prices.length; j++) {
                int profit = prices[j] - prices[i];
                maxProfit = Math.max(maxProfit, profit);
            }
        }

        return maxProfit;
    }
}
```

**Why this doesn't work well**:
- Too slow for large inputs (up to 10^5 elements)
- Time Limit Exceeded on LeetCode

---

### Approach 2: Single Pass - Track Minimum Price ✓
**Intuition**:
As we iterate through prices:
- Keep track of the **minimum price seen so far** (best buy opportunity)
- At each day, calculate profit if we sell today: `current price - min price so far`
- Update maximum profit if current profit is better

**Key insight:** We only need to know the minimum price before current day, not all previous prices!

**Algorithm**:
1. Initialize minPrice = infinity (or first price)
2. Initialize maxProfit = 0
3. For each price in array:
   - Update maxProfit = max(maxProfit, current price - minPrice)
   - Update minPrice = min(minPrice, current price)
4. Return maxProfit

**Time Complexity**: O(n) - single pass
**Space Complexity**: O(1) - only two variables

```java
// Java Solution - Optimized (Single Pass)
class Solution {
    public int maxProfit(int[] prices) {
        // Edge case: empty or single element
        if (prices == null || prices.length < 2) {
            return 0;
        }

        int minPrice = Integer.MAX_VALUE;  // Track minimum price seen so far
        int maxProfit = 0;                  // Track maximum profit

        for (int price : prices) {
            // If selling today, what would the profit be?
            // (current price - cheapest buy price seen so far)
            maxProfit = Math.max(maxProfit, price - minPrice);

            // Update minimum price if current price is lower
            minPrice = Math.min(minPrice, price);
        }

        return maxProfit;
    }
}
```

**Why this works**:
- We process each price exactly once (O(n))
- At each step, we know the best buy price before this point
- We calculate potential profit and keep the maximum
- minPrice is always updated AFTER profit calculation, ensuring buy happens before sell

---

## Complexity Analysis

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Brute Force | O(n²) | O(1) | TLE on large inputs |
| Single Pass | O(n) | O(1) | **Optimal solution** |

---

## Edge Cases to Consider

- [x] Single element array - No transaction possible, return 0
- [x] Empty array - Return 0
- [x] Prices always decreasing - No profit, return 0
- [x] Prices always increasing - Buy first, sell last
- [x] All prices same - No profit, return 0
- [x] Minimum price is last element - Can't sell, maxProfit from earlier transactions

---

## Test Cases

```java
// Test Case 1: Standard case
Input: prices = [7,1,5,3,6,4]
Expected Output: 5 (buy at 1, sell at 6)
Actual Output: 5
Status: ✓

// Test Case 2: No profit possible
Input: prices = [7,6,4,3,1]
Expected Output: 0
Actual Output: 0
Status: ✓

// Test Case 3: Single element
Input: prices = [5]
Expected Output: 0
Actual Output: 0
Status: ✓

// Test Case 4: Always increasing
Input: prices = [1,2,3,4,5]
Expected Output: 4 (buy at 1, sell at 5)
Actual Output: 4
Status: ✓

// Test Case 5: All same price
Input: prices = [3,3,3,3,3]
Expected Output: 0
Actual Output: 0
Status: ✓

// Test Case 6: Min at end
Input: prices = [5,4,3,2,1]
Expected Output: 0
Actual Output: 0
Status: ✓
```

---

## Walkthrough Example

Let's trace through `prices = [7,1,5,3,6,4]`:

```
Initial: minPrice = ∞, maxProfit = 0

Day 0: price = 7
  maxProfit = max(0, 7 - ∞) = 0  (can't sell yet)
  minPrice = min(∞, 7) = 7

Day 1: price = 1
  maxProfit = max(0, 1 - 7) = 0  (no profit)
  minPrice = min(7, 1) = 1       (found cheaper buy!)

Day 2: price = 5
  maxProfit = max(0, 5 - 1) = 4  (profit if buy at 1, sell at 5)
  minPrice = min(1, 5) = 1

Day 3: price = 3
  maxProfit = max(4, 3 - 1) = 4  (current profit = 2, keep 4)
  minPrice = min(1, 3) = 1

Day 4: price = 6
  maxProfit = max(4, 6 - 1) = 5  (better profit! buy at 1, sell at 6)
  minPrice = min(1, 6) = 1

Day 5: price = 4
  maxProfit = max(5, 4 - 1) = 5  (current profit = 3, keep 5)
  minPrice = min(1, 4) = 1

Return: 5 ✓
```

---

## Alternative Approach: Kadane's Algorithm Variant

This problem can also be solved using a modified version of Kadane's algorithm (maximum subarray):

```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        int maxEndingHere = 0;

        for (int i = 1; i < prices.length; i++) {
            // Calculate profit from previous day
            int profit = prices[i] - prices[i - 1];

            // Either continue existing profit or start fresh
            maxEndingHere = Math.max(0, maxEndingHere + profit);

            // Track maximum profit seen
            maxProfit = Math.max(maxProfit, maxEndingHere);
        }

        return maxProfit;
    }
}
```

**Explanation:**
- Convert prices to daily profit/loss: [7,1,5,3,6,4] → [-6,+4,-2,+3,-2]
- Find maximum subarray sum (continuous profit days)
- This gives same result but conceptually different

**Both approaches are O(n) time and O(1) space!**

---

## Key Takeaways

1. **Pattern Recognition**: "Maximum difference with order constraint" → Track min/max in single pass
2. **State Tracking**: Keep minimum value seen so far instead of checking all previous values
3. **Greedy Approach**: At each step, assume we sell today and calculate profit with best buy price
4. **Order Matters**: Update minPrice AFTER calculating profit to ensure buy before sell
5. **This pattern extends**: Stock II (multiple transactions), Stock III (k transactions)

---

## Similar Problems

- **LeetCode #122** - Best Time to Buy and Sell Stock II (Multiple transactions allowed)
- **LeetCode #123** - Best Time to Buy and Sell Stock III (At most 2 transactions)
- **LeetCode #188** - Best Time to Buy and Sell Stock IV (At most k transactions)
- **LeetCode #309** - Best Time to Buy and Sell Stock with Cooldown
- **LeetCode #714** - Best Time to Buy and Sell Stock with Transaction Fee
- **LeetCode #53** - Maximum Subarray (Kadane's algorithm - similar concept)

---

## Time Spent

- Understanding: 5 mins
- Coding brute force: 5 mins
- Coding optimized: 10 mins
- Testing: 5 mins
- Understanding Kadane variant: 5 mins
- **Total: 30 mins**

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
- Finding optimal buy/sell with ordering constraint
- Tracking best value seen so far in array
- Need O(n) time, O(1) space

**Common mistakes to avoid:**
- Finding min and max of entire array (doesn't handle order!)
- Updating minPrice before calculating profit (breaks buy-before-sell constraint)
- Forgetting edge cases (single element, decreasing prices)

**Interview tips:**
- Start by explaining the constraint: "buy before sell"
- Mention brute force first, then optimize
- Walk through example to show why greedy works
- Explain why we update minPrice after profit calculation

---

## Pattern Category: Array / Greedy

**Pattern Name**: Track Min/Max with Single Pass
**Key Idea**: Maintain running minimum, calculate optimal result at each step
**Time**: O(n)
**Space**: O(1)

This is **Pattern #2** in your arsenal! 🎯

**Connection to Pattern #1 (Two Sum):**
- Both are array problems
- Both optimize O(n²) brute force to O(n)
- Two Sum uses hash map (O(n) space), this uses greedy (O(1) space)
