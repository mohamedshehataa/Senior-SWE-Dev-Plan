# Day 3 - Two Pointers Technique

**Date**: November 18, 2025
**Category**: Algorithm Pattern
**Difficulty**: Beginner to Intermediate

---

## 📚 What is Two Pointers?

**Two Pointers** is an algorithm pattern where you use two pointers (indices) to traverse a data structure, typically an array or string. The pointers move toward each other, away from each other, or at different speeds to solve problems efficiently.

**Simple analogy**: Imagine two people searching a bookshelf from opposite ends. They can cover the entire shelf faster than one person, and they can find matching books by comparing what they each find.

---

## 🎯 Why is it Important?

- **Real-world use case 1**: **String palindrome checking** - Check from both ends simultaneously (used in DNA sequence analysis)
- **Real-world use case 2**: **Array partitioning** - QuickSort uses two pointers to partition around pivot
- **Real-world use case 3**: **Container problems** - Finding optimal container sizes (logistics, shipping)

- **Interview relevance**: Two Pointers appears in **20-30% of array/string interview questions**. It's a fundamental optimization technique that:
  - Reduces O(n²) brute force to O(n)
  - Eliminates need for extra space in many cases
  - Shows understanding of efficient algorithm design

**FAANG companies LOVE this pattern!**

---

## 🔍 Deep Dive

### Three Main Patterns

#### 1. Opposite Direction (Left & Right)
**When to use:** Palindromes, finding pairs in sorted arrays, containers

```
Start:  [1, 2, 3, 4, 5, 6]
         ↑              ↑
        left          right

Move:   [1, 2, 3, 4, 5, 6]
            ↑        ↑
           left    right

End:    [1, 2, 3, 4, 5, 6]
               ↑  ↑
             left right
```

**Characteristics:**
- Start from both ends
- Move toward center
- Usually O(n) time, O(1) space
- Often terminates when pointers meet

---

#### 2. Same Direction (Slow & Fast)
**When to use:** Removing duplicates, partitioning, in-place modifications

```
Start:  [1, 1, 2, 2, 3, 3]
         ↑  ↑
       slow fast

Move:   [1, 2, 2, 2, 3, 3]
            ↑     ↑
          slow   fast

End:    [1, 2, 3, ?, ?, ?]
                  ↑
                slow (new length)
```

**Characteristics:**
- Both start from same end (usually left)
- Fast pointer explores, slow pointer writes/tracks
- In-place array modifications
- Classic example: Remove duplicates from sorted array

---

#### 3. Fast & Slow (Floyd's Cycle Detection)
**When to use:** Linked list cycle detection, finding middle element

```
[1] → [2] → [3] → [4] → [5]
 ↑           ↑
slow        fast

Move:
[1] → [2] → [3] → [4] → [5]
      ↑                 ↑
     slow              fast
```

**Characteristics:**
- Fast moves 2x speed of slow
- Used for cycle detection (if they meet, cycle exists)
- Finding middle of linked list
- Also called "Tortoise and Hare"

---

## 💻 Code Implementation

### Example 1: Valid Palindrome (Opposite Direction)

```java
// Problem: Check if string is a palindrome (ignore non-alphanumeric)
// Input: "A man, a plan, a canal: Panama"
// Output: true

class Solution {
    public boolean isPalindrome(String s) {
        // Edge case
        if (s == null || s.length() <= 1) {
            return true;
        }

        // Two pointers from opposite ends
        int left = 0;
        int right = s.length() - 1;

        while (left < right) {
            // Skip non-alphanumeric from left
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }

            // Skip non-alphanumeric from right
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }

            // Compare characters (case-insensitive)
            if (Character.toLowerCase(s.charAt(left)) !=
                Character.toLowerCase(s.charAt(right))) {
                return false;
            }

            // Move both pointers
            left++;
            right--;
        }

        return true;
    }
}
```

**Why this works:**
- Process from both ends simultaneously
- Skip invalid characters
- Compare characters that matter
- O(n) time, O(1) space

---

### Example 2: Remove Duplicates from Sorted Array (Same Direction)

```java
// Problem: Remove duplicates in-place, return new length
// Input: nums = [1,1,2,2,3]
// Output: 3, nums = [1,2,3,_,_]

class Solution {
    public int removeDuplicates(int[] nums) {
        // Edge case
        if (nums == null || nums.length == 0) {
            return 0;
        }

        // Slow pointer: position to write next unique element
        int slow = 0;

        // Fast pointer: explore array
        for (int fast = 1; fast < nums.length; fast++) {
            // Found new unique element
            if (nums[fast] != nums[slow]) {
                slow++;  // Move slow to next position
                nums[slow] = nums[fast];  // Write unique element
            }
        }

        // Length of array with unique elements
        return slow + 1;
    }
}
```

**Why this works:**
- Slow tracks position of last unique element
- Fast explores and finds next unique element
- Copy unique elements to front of array
- O(n) time, O(1) space

**Walkthrough:**
```
nums = [1, 1, 2, 2, 3]

Initial: slow=0, fast=1
[1, 1, 2, 2, 3]
 ↑  ↑
slow fast
nums[fast]=1 == nums[slow]=1 → skip

fast=2:
[1, 1, 2, 2, 3]
 ↑     ↑
slow  fast
nums[fast]=2 != nums[slow]=1 → found unique!
slow++, nums[1]=2

[1, 2, 2, 2, 3]
    ↑     ↑
  slow   fast

fast=3:
nums[fast]=2 == nums[slow]=2 → skip

fast=4:
[1, 2, 2, 2, 3]
    ↑        ↑
  slow      fast
nums[fast]=3 != nums[slow]=2 → found unique!
slow++, nums[2]=3

[1, 2, 3, 2, 3]
       ↑     ↑
     slow   fast

Return slow + 1 = 3
```

---

### Example 3: Two Sum II - Sorted Array (Opposite Direction)

```java
// Problem: Find two numbers that add up to target (sorted array)
// Input: numbers = [2,7,11,15], target = 9
// Output: [1,2] (1-indexed)

class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;

        while (left < right) {
            int sum = numbers[left] + numbers[right];

            if (sum == target) {
                // Found! Return 1-indexed positions
                return new int[] {left + 1, right + 1};
            } else if (sum < target) {
                // Sum too small, need larger numbers
                left++;  // Move left pointer right (increase sum)
            } else {
                // Sum too large, need smaller numbers
                right--;  // Move right pointer left (decrease sum)
            }
        }

        return new int[] {-1, -1};  // No solution
    }
}
```

**Why this works:**
- Array is sorted → we can use two pointers
- If sum < target → need bigger numbers → move left right
- If sum > target → need smaller numbers → move right left
- O(n) time, O(1) space
- **Compare to Day 1 Two Sum:** Used HashMap O(n) space, this uses O(1) space because array is sorted!

---

### Example 4: Container With Most Water (Opposite Direction)

```java
// Problem: Find two lines that form container with max water
// Input: height = [1,8,6,2,5,4,8,3,7]
// Output: 49 (lines at index 1 and 8)

class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxArea = 0;

        while (left < right) {
            // Calculate area with current pointers
            int width = right - left;
            int minHeight = Math.min(height[left], height[right]);
            int area = width * minHeight;

            maxArea = Math.max(maxArea, area);

            // Move pointer with smaller height
            // (moving larger height won't help)
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
}
```

**Why we move the smaller height:**
- Area = width × min(height[left], height[right])
- Width decreases as we move pointers inward
- To potentially increase area, we need to increase min height
- Moving the smaller height gives us a chance to find taller line

**This is a greedy two-pointer approach!**

---

### Example 5: Linked List Cycle (Fast & Slow)

```java
// Problem: Detect if linked list has a cycle
// Input: 1 → 2 → 3 → 4 → 2 (cycle back to node 2)
// Output: true

class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;        // Move 1 step
            fast = fast.next.next;   // Move 2 steps

            if (slow == fast) {
                return true;  // Cycle detected!
            }
        }

        return false;  // Fast reached end, no cycle
    }
}
```

**Why this works (Floyd's Cycle Detection):**
- If there's a cycle, fast will eventually catch up to slow
- Think of a circular race track: faster runner laps slower runner
- If no cycle, fast reaches null
- O(n) time, O(1) space

---

## 🏢 Real-World Examples

### Company 1: Google - Finding Optimal Ad Placement

**Challenge**: Find best pair of ad positions that maximize engagement (sum of click rates = target)

**How Two Pointers helps:**
```java
// Sorted array of ad positions by click rate
int[] adClickRates = [100, 200, 300, 400, 500];
int targetEngagement = 600;

// Use two pointers to find pair that sums to target
// O(n) time instead of O(n²) brute force
```

---

### Company 2: Amazon - Package Optimization

**Challenge**: Find two packages that fit exactly in delivery truck capacity

**How Two Pointers helps:**
- Sort packages by weight
- Use left/right pointers to find pair that sums to capacity
- O(n log n) for sorting + O(n) for two pointers
- Better than O(n²) checking all pairs

---

### Company 3: Netflix - Video Compression

**Challenge**: Find segments of video with specific characteristics (in-place processing)

**How Two Pointers helps:**
- Use slow/fast pointers to process video frames
- Remove redundant frames in-place
- Save memory by not creating new arrays

---

## 🎤 Common Interview Questions

### Question 1:
**Q**: When should you use Two Pointers instead of HashMap?

**A**:
Use **Two Pointers** when:
- Array/string is **sorted** (or can be sorted)
- Need **O(1) space** (in-place modification)
- Problem involves **pairs or subsequences**
- Looking for **palindromes or symmetry**

Use **HashMap** when:
- Array is **unsorted** and sorting would break the problem
- Need to **track indices or frequencies**
- O(n) space is acceptable
- Need **fast lookups by key**

**Example:**
- Two Sum (unsorted) → HashMap
- Two Sum II (sorted) → Two Pointers ✓

---

### Question 2:
**Q**: Explain the difference between slow/fast and left/right two pointer patterns.

**A**:

**Left/Right (Opposite Direction):**
- Start from both ends, move toward center
- Used for palindromes, sorted array pairs
- Terminate when pointers meet
- Example: Valid Palindrome

**Slow/Fast (Same Direction):**
- Both start from same end (usually left)
- Fast explores, slow writes/tracks
- Used for in-place modifications
- Example: Remove Duplicates

**Fast/Slow (Different Speeds):**
- Slow moves 1 step, fast moves 2 steps
- Used for cycle detection, finding middle
- Example: Linked List Cycle

---

### Question 3:
**Q**: How would you use Two Pointers to find three numbers that sum to target (3Sum)?

**A**:
```java
// 1. Sort array: O(n log n)
// 2. Fix one number (i)
// 3. Use two pointers (left, right) on remaining array
// 4. Find pair that sums to (target - nums[i])

class Solution {
    public List<List<Integer>> threeSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();

        for (int i = 0; i < nums.length - 2; i++) {
            // Skip duplicates
            if (i > 0 && nums[i] == nums[i-1]) continue;

            int left = i + 1;
            int right = nums.length - 1;
            int targetSum = target - nums[i];

            while (left < right) {
                int sum = nums[left] + nums[right];
                if (sum == targetSum) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    left++;
                    right--;
                    // Skip duplicates
                    while (left < right && nums[left] == nums[left-1]) left++;
                    while (left < right && nums[right] == nums[right+1]) right--;
                } else if (sum < targetSum) {
                    left++;
                } else {
                    right--;
                }
            }
        }
        return result;
    }
}
```

**Time:** O(n²) - O(n) for outer loop × O(n) for two pointers
**Space:** O(1) excluding result

---

## 🔗 Related Concepts

- **Binary Search** - Also uses two pointers (left/right) but for divide-and-conquer
- **Sliding Window** - Extension of two pointers for subarray problems
- **Fast & Slow Pointers** - For linked list problems (cycle, middle)
- **Quicksort Partitioning** - Uses two pointers to partition around pivot
- **Dutch National Flag** - Three-way partitioning with two pointers

---

## 📝 Hands-On Exercise

### Exercise 1: Reverse String In-Place

**Problem**: Write a function to reverse a string in-place.

```java
// Input: s = ['h','e','l','l','o']
// Output: ['o','l','l','e','h']
```

**Your solution:**
```java
class Solution {
    public void reverseString(char[] s) {
        // TODO: Implement using two pointers
    }
}
```

**Solution:**
```java
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length - 1;

        while (left < right) {
            // Swap
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;

            // Move pointers
            left++;
            right--;
        }
    }
}
```

**Time:** O(n), **Space:** O(1)

---

### Exercise 2: Move Zeroes (Slow/Fast Pattern)

**Problem**: Move all zeros to end of array, maintain relative order of non-zeros.

```java
// Input: nums = [0,1,0,3,12]
// Output: [1,3,12,0,0]
```

**Your solution:**
```java
class Solution {
    public void moveZeroes(int[] nums) {
        // TODO: Implement using slow/fast pointers
    }
}
```

**Solution:**
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int slow = 0;  // Position to write next non-zero

        // Fast pointer explores array
        for (int fast = 0; fast < nums.length; fast++) {
            if (nums[fast] != 0) {
                // Swap non-zero to slow position
                int temp = nums[slow];
                nums[slow] = nums[fast];
                nums[fast] = temp;
                slow++;
            }
        }
    }
}
```

**Time:** O(n), **Space:** O(1)

---

## 📖 Resources & References

- **LeetCode Problems**: Two Pointers tag - https://leetcode.com/tag/two-pointers/
- **Pattern Recognition**: "Two Pointers" in "Grokking the Coding Interview"
- **Video**: NeetCode Two Pointers Playlist
- **Book**: "Cracking the Coding Interview" - Chapter on Arrays & Strings

---

## ✅ Concept Mastery Checklist

- [ ] Understand all 3 two-pointer patterns
- [ ] Can identify when to use two pointers vs other techniques
- [ ] Completed all hands-on exercises
- [ ] Can explain pattern to someone else
- [ ] Solved at least 5 two-pointer LeetCode problems
- [ ] Can answer interview questions confidently

---

## 🧠 Key Takeaways (In Your Own Words)

1. **Two Pointers = Optimization Tool** - Reduces O(n²) to O(n) for many problems
2. **Three Main Patterns:**
   - **Left/Right**: Palindromes, pairs (opposite direction)
   - **Slow/Fast**: In-place modifications (same direction)
   - **Tortoise/Hare**: Cycle detection (different speeds)
3. **Sorted Arrays Love Two Pointers** - Take advantage of order
4. **Space Efficient** - Usually O(1) space, great for in-place operations
5. **Pointer Movement Logic is Key** - Understand WHEN to move which pointer

---

## 🔥 Two Pointers Decision Tree

```
Problem involves array/string?
  ↓
Is it sorted (or can you sort)?
  ↓ YES
Need to find pairs/triplets?
  ↓ YES
  → Use LEFT/RIGHT two pointers

  ↓ NO
Need in-place modification?
  ↓ YES
  → Use SLOW/FAST two pointers

Problem involves linked list?
  ↓
Need cycle detection or middle?
  ↓ YES
  → Use FAST/SLOW (Floyd's)
```

---

## 📅 Review Schedule

- [ ] Review after 1 day (Nov 19)
- [ ] Review after 1 week (Nov 25)
- [ ] Solve 2-3 two-pointer problems
- [ ] Review after 1 month (Dec 18)

---

## 💭 Personal Notes

**Connection to previous concepts:**
- Day 1 (Two Sum): Used HashMap for O(n) time
- **Today**: If array is sorted, Two Pointers achieves O(n) time with O(1) space!
- This shows importance of recognizing problem constraints (sorted vs unsorted)

**For interviews:**
- Always ask: "Is the array sorted?" → Opens door for two pointers
- Mention space optimization: "We can do this without extra space using two pointers"
- Draw the pointer movements on whiteboard

**Connection to PMC work:**
- Your BackupThreadPoolService processes tasks in order
- Could use two-pointer-like logic for priority queue merging
- In-place array modifications = memory efficient (critical for large datasets)

**Next concepts:**
- **Day 4**: Sliding Window (extension of two pointers for subarrays)
- **Day 5**: Stack/Queue patterns
- Both build on two-pointer foundation!

---

## 🎯 Practice Plan

**Today's LeetCode (Day 3):**
- Contains Duplicate ✓ (warmup, uses HashSet)

**Next 2 days (reinforce Two Pointers):**
- Day 4: Valid Palindrome (left/right pattern)
- Day 5: Remove Duplicates from Sorted Array (slow/fast pattern)
- Day 6: Move Zeroes (slow/fast pattern)

**This week's goal:** Master the three two-pointer patterns through repetition!
