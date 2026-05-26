# 🏠 House Robber

---

## 📌 Problem

You are a robber planning to rob houses.

Each house contains some money.

Constraint:

```text
You cannot rob two adjacent houses.
```

Find the maximum amount of money you can rob.

---

## ✅ Example

Input:

```text
nums = [2,7,9,3,1]
```

Output:

```text id="fhx4ll"
12
```

Explanation:

```text id="w80ts7"
Rob house 1 → 2
Rob house 3 → 9
Rob house 5 → 1

Total = 12
```

---

# ✅ Code (UNCHANGED)

```java id="2tw0t8"
class Solution {
    public int rob(int[] nums) {
        if(nums.length < 2){
            return nums[0];
        }

        int[] dp = new int[nums.length];

        dp[0] = nums[0];
        dp[1] = Math.max(nums[0],nums[1]);

        for(int i = 2;i<nums.length;i++){
            dp[i] = Math.max(dp[i-2] + nums[i],dp[i-1]);
        }

        return dp[nums.length - 1];
    }
}
```

---

# 🧠 Core Idea

At every house, we have 2 choices:

---

## 1️⃣ Rob Current House

If we rob current house `i`:

```text id="gshz1g"
We cannot rob house i-1
```

So total becomes:

```text id="xv9x6m"
dp[i-2] + nums[i]
```

---

## 2️⃣ Skip Current House

Then profit remains:

```text id="6m6r9w"
dp[i-1]
```

---

# 🎯 Transition Formula

```java id="q0qqwz"
dp[i] = Math.max(dp[i-2] + nums[i], dp[i-1]);
```

---

# 🧩 Meaning of `dp[i]`

```text id="y4w7k3"
Maximum money we can rob from houses 0 to i
```

---

# 🔍 Base Cases

---

## `dp[0]`

Only one house:

```java id="tq5r2m"
dp[0] = nums[0];
```

---

## `dp[1]`

Choose maximum of first two houses:

```java id="v7m4g0"
dp[1] = Math.max(nums[0], nums[1]);
```

---

# 📊 Dry Run

Input:

```text id="7w1g8y"
nums = [2,7,9,3,1]
```

---

# Step 1

```text id="8xhf7j"
dp[0] = 2
```

---

# Step 2

```text id="4z1d1v"
dp[1] = max(2,7)
      = 7
```

---

# Step 3 (`i = 2`)

House value:

```text id="f8ptwk"
9
```

Options:

---

## Rob current

```text id="m0c3m9"
dp[0] + 9
= 2 + 9
= 11
```

---

## Skip current

```text id="ubczhb"
dp[1] = 7
```

---

Choose maximum:

```text id="ehb3ne"
dp[2] = 11
```

---

# Step 4 (`i = 3`)

Value:

```text id="3sv2jm"
3
```

---

## Rob current

```text id="z88jce"
dp[1] + 3
= 7 + 3
= 10
```

---

## Skip current

```text id="g0vzyx"
dp[2] = 11
```

---

Choose maximum:

```text id="cm4lv2"
dp[3] = 11
```

---

# Step 5 (`i = 4`)

Value:

```text id="8mbpxh"
1
```

---

## Rob current

```text id="g95v6n"
dp[2] + 1
= 11 + 1
= 12
```

---

## Skip current

```text id="6mlrdr"
dp[3] = 11
```

---

Choose maximum:

```text id="6fjlwm"
dp[4] = 12
```

---

# ✅ Final DP Array

```text id="a5n7t9"
[2, 7, 11, 11, 12]
```

Answer:

```text id="f2pq8y"
12
```

---

# 🧠 Why This Works

At each house:

```text id="10d4m2"
Either take it
or skip it
```

Dynamic Programming stores the best answer up to that point.

---

# ⏱ Complexity

### Time

```text id="wmbgdn"
O(n)
```

Single traversal.

---

### Space

```text id="y6rj6q"
O(n)
```

DP array.

---

# 🚀 Space Optimized Idea

We only need:

```text id="x4cvuk"
dp[i-1]
dp[i-2]
```

So we can reduce space to:

```text id="x78nq3"
O(1)
```

using two variables.

---

# 🏆 Pattern Recognition

If problem says:

```text id="8yscv8"
Choose or skip
Adjacent restriction
Maximum profit/count
```

Think:

```text id="7g8fc6"
Dynamic Programming
```

---

# 🔥 Golden Mental Model

```text id="1of0jv"
For every house:
either rob it and skip previous,
or skip it entirely.
Take the better option.
```

---

# 🚀 Final Summary

The algorithm:

1. Defines `dp[i]` as max money till house `i`.
2. For each house:
   - rob current
   - or skip current
3. Stores the better choice.

---

```text id="l40z2j"
House Robber = Pick/Skip Dynamic Programming.
```

---
