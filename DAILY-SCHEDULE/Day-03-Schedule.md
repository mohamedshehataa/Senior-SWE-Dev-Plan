# Day 3 Schedule - Wednesday, November 18, 2025

**Theme**: HashSet Pattern + Two Pointers Mastery
**Goals**: Contains Duplicate problem, Two Pointers technique, Concurrency Ch 3

---

## 🌅 Morning Block (12:00 PM - 2:30 PM) - 2.5 hours

### 12:00 PM - 12:15 PM: Wake Up & Breakfast (15 min)
- Light breakfast, hydrate
- Quick review: What did I learn yesterday?
  - Best Time to Buy Stock (single-pass pattern)
  - Java Concurrency Ch 1-2 (threads, race conditions)
  - Load Balancing (L4 vs L7)

### 12:15 PM - 1:30 PM: LeetCode Problem (75 min)
**Problem**: Contains Duplicate

**Approach:**
1. **Read & Understand** (10 min)
   - Read problem statement carefully
   - Identify: "Detect if any value appears at least twice"
   - Constraints: Up to 10^5 elements
   - Expected: O(n) time solution

2. **Think & Plan** (15 min)
   - Brainstorm approaches:
     - Brute force: O(n²) nested loops
     - Sorting: O(n log n)
     - HashSet: O(n) time, O(n) space ✓
   - Choose HashSet approach
   - Edge cases: single element, all same, all different

3. **Code** (30 min)
   - Implement brute force first (5 min)
   - Implement sorting approach (10 min)
   - Implement optimal HashSet solution (15 min)
   ```java
   Set<Integer> seen = new HashSet<>();
   for (int num : nums) {
       if (!seen.add(num)) {  // add() returns false if exists
           return true;
       }
   }
   return false;
   ```

4. **Test** (10 min)
   - Test case 1: [1,2,3,1] → true
   - Test case 2: [1,2,3,4] → false
   - Test case 3: [1] → false
   - Edge cases: negative numbers, large array

5. **Reflect** (10 min)
   - Why HashSet over HashMap? (Only need existence, not value)
   - Pattern: Hash-based duplicate detection
   - Connection to Day 1 (Two Sum used HashMap)
   - Update LeetCode tracker

### 1:30 PM - 2:30 PM: Concept Learning - Two Pointers (60 min)

**Goal**: Master the two-pointer technique

1. **Read & Understand** (25 min)
   - Three patterns:
     - **Left/Right**: Palindromes, pairs in sorted arrays
     - **Slow/Fast**: In-place modifications, remove duplicates
     - **Tortoise/Hare**: Cycle detection (Floyd's algorithm)
   - When to use two pointers vs HashMap
   - Real-world applications

2. **Code Examples** (25 min)
   - **Example 1**: Valid Palindrome (left/right)
   - **Example 2**: Remove Duplicates (slow/fast)
   - **Example 3**: Two Sum II (sorted array)
   - Type out each solution, understand pointer movement

3. **Quick Exercise** (10 min)
   - Reverse String in-place
   ```java
   int left = 0, right = s.length - 1;
   while (left < right) {
       swap(s[left++], s[right--]);
   }
   ```
   - Move Zeroes to end

---

## ⚙️ Work Block (2:30 PM - 5:00/6:00 PM) - 2.5-3.5 hours

### 2:30 PM - 5:00 PM: PMC Work
- Meetings: 3:30 PM - 5:00 PM
- Development work as needed
- **If finish early:** Start Java Concurrency Ch 3

---

## 😴 Nap Time (6:30 PM - 7:30 PM) - 1 hour
- Power nap to recharge
- Set alarm for 7:30 PM

---

## 📚 Evening Study Block (8:00 PM - 10:00 PM) - 2 hours

### 8:00 PM - 9:30 PM: Java Concurrency Ch 3 - Memory Model (90 min)

**Reading**: "Java Concurrency in Practice" Chapter 3 - Visibility

**Focus Areas:**
1. **Visibility** (30 min)
   - What is visibility in concurrent programs?
   - Why updates by one thread might not be visible to others
   - Example: Thread 1 writes, Thread 2 reads stale value
   - Real-world bug scenarios

2. **Volatile Keyword** (30 min)
   - What does `volatile` do?
   - When to use volatile vs synchronized
   - Performance implications
   ```java
   private volatile boolean ready;  // Ensures visibility
   private int number;

   // Writer thread
   number = 42;
   ready = true;  // volatile write

   // Reader thread
   while (!ready) {  // volatile read
       Thread.yield();
   }
   System.out.println(number);  // Guaranteed to see 42
   ```

3. **Publication and Escape** (30 min)
   - Safe publication patterns
   - How objects can "escape" before fully constructed
   - Immutability as thread safety guarantee
   - Connection to your PMC code (safe object initialization)

**Active Learning:**
- Highlight key points in book
- Take notes on visibility issues
- Think: "Have I seen this bug in PMC code?"

### 9:30 PM - 10:00 PM: Code BankAccount Example (30 min)

**From Day 2 Concept File:**

Implement thread-safe BankAccount with:
1. Synchronized methods
2. Test with multiple threads
3. Verify final balance is correct

```java
public class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    public synchronized void deposit(double amount) {
        balance += amount;
        System.out.println("Deposited: " + amount + ", Balance: " + balance);
    }

    public synchronized void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            System.out.println("Withdrew: " + amount + ", Balance: " + balance);
        } else {
            System.out.println("Insufficient funds for withdrawal: " + amount);
        }
    }

    public synchronized double getBalance() {
        return balance;
    }
}

// Test: 10 threads deposit $100, 10 threads withdraw $50
// Expected final balance: initial + 1000 - 500 = initial + 500
```

**Test it:**
- Create 10 deposit threads
- Create 10 withdraw threads
- Start all threads, join all
- Verify balance = initial + 500

---

## 🏋️ Gym Time (Wednesday - 8:00 PM - 10:30 PM)

**CONFLICT RESOLUTION:**
Today is Wednesday = Gym day (8:00 - 10:30 PM)

**Adjusted Schedule:**
- 8:00 PM - 10:30 PM: Gym 🏋️
- 10:30 PM - 12:30 AM: Java Concurrency Ch 3 + BankAccount coding (shifted)

---

## 🌙 Night Study Block (10:30 PM - 12:30 AM) - 2 hours

### 10:30 PM - 12:00 AM: Java Concurrency Ch 3 (90 min)
[Same content as 8:00 PM slot above - shifted due to gym]

### 12:00 AM - 12:30 AM: Code BankAccount Example (30 min)
[Same content as 9:30 PM slot above - shifted due to gym]

---

## 📝 Reflection & Update (12:30 AM - 1:00 AM) - 30 min

### Update Progress Tracker
```markdown
**Week 1, Day 3 Completed:**
- ✅ LeetCode: Contains Duplicate (HashSet pattern)
- ✅ Concept: Two Pointers technique (3 patterns learned)
- ✅ Java Concurrency Ch 3 (Visibility, volatile, publication)
- ✅ Coded BankAccount thread-safe example
- ✅ Week 1: 3/7 LeetCode, 14/19 hours
```

### Daily Reflection Questions:
1. **What pattern did I learn today?**
   - HashSet for duplicate detection
   - Two Pointers (left/right, slow/fast, fast/slow)

2. **How does this connect to previous days?**
   - Day 1: HashMap (Two Sum)
   - Day 2: Greedy (Best Time to Buy Stock)
   - Day 3: HashSet (Contains Duplicate)
   - **Pattern**: Hash-based optimizations!

3. **What was challenging?**
   - Understanding when to use two pointers vs hash structures
   - Visibility issues in concurrency (new concept)

4. **What can I improve tomorrow?**
   - Code faster (spent too much time on testing?)
   - Read book more actively (take notes while reading)

5. **Interview-ready soundbite:**
   - "I can use two pointers to optimize from O(n²) to O(n) for sorted arrays"
   - "Two pointers has three patterns: opposite direction, same direction, and different speeds"

---

## 🎯 Late Night (1:00 AM - 4:00 AM) - Free Time

**Options:**
- Review LeetCode patterns from Week 1
- Watch YouTube video on Two Pointers (NeetCode)
- Read ahead: Effective Java Items 78-81
- Relax, entertainment, personal time
- Get 7-8 hours sleep before tomorrow

---

## 📊 Day 3 Summary

| Activity | Time Allocated | Status |
|----------|---------------|--------|
| LeetCode (Contains Duplicate) | 75 min | ⬜ |
| Two Pointers Concept | 60 min | ⬜ |
| Java Concurrency Ch 3 | 90 min | ⬜ |
| BankAccount Coding | 30 min | ⬜ |
| PMC Work | 2.5-3.5 hrs | ⬜ |
| Gym | 2.5 hrs | ⬜ |
| Reflection & Updates | 30 min | ⬜ |
| **Total Study Time** | ~4.5 hours | ⬜ |

---

## 🔥 Streak Tracker

- Day 1: ✅ (Two Sum, Scalability)
- Day 2: ✅ (Best Time to Buy Stock, Concurrency Ch 1-2, Load Balancing)
- Day 3: ⬜ (Contains Duplicate, Two Pointers, Concurrency Ch 3)

**Keep the streak alive!** 🔥🔥🔥

---

## 💡 Tomorrow Preview (Day 4 - Thursday)

**LeetCode**: Valid Palindrome (Two Pointers practice!)
**Concept**: Caching Strategies (Redis, Memcached, cache invalidation)
**Evening**: Spring Boot basics review (your PMC stack!)

---

## 🎯 Week 1 Progress

```
LeetCode: [✅][✅][  ][  ][  ][  ][  ] 2/7 (29%)
Concepts: [✅][✅][  ][  ][  ][  ][  ] 2/7 (29%)
Hours:    [✅][✅][  ][  ][  ][  ][  ] 9/19 hours
```

**On track!** If you complete Day 3, you'll be at 43% weekly completion (3/7 days).

---

## ✅ Pre-Sleep Checklist

Before going to sleep at 4 AM:
- [ ] Updated PROGRESS.md with Day 3 completion
- [ ] Committed code to GitHub (BankAccount example)
- [ ] Reviewed what I learned today
- [ ] Planned tomorrow's wake-up (12 PM)
- [ ] Set alarm for 12 PM
- [ ] Feeling confident about Two Pointers pattern? ✓

**Sleep well! Day 4 awaits.** 🌙
