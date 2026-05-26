# 🪜 Climbing Stairs

---

## 📌 Problem

You are climbing a staircase.

Each time you can climb:

- `1` step
- `2` steps

Find how many distinct ways you can reach the top.

---

## ✅ Example

### Input

```text
n = 3
```

### Output

```text
3
```

### Explanation

```text
1 + 1 + 1
1 + 2
2 + 1
```

Total ways:

```text
3
```

---

# ✅ Code (UNCHANGED)

```java
class Solution {
    public int climbStairs(int n) {

        if(n == 1){
            return 1;
        }

        int[] dp = new int[n+1];
        dp[1] = 1;
        dp[2] = 2;

        for(int i = 3;i <=n;i++){
            dp[i] = dp[i-1] + dp[i-2];
        }

        return dp[n];

    }
}
```

---

# 🧠 Core Idea

To reach stair `i`:

You can come from:

- stair `i-1` (take 1 step)
- stair `i-2` (take 2 steps)

So:

```text
dp[i] = dp[i-1] + dp[i-2]
```

---

# 🎯 Meaning of `dp[i]`

```text
dp[i] = number of ways to reach stair i
```

---

# 🔍 Base Cases

## `dp[1] = 1`

Only one way:

```text
1
```

---

## `dp[2] = 2`

Ways:

```text
1 + 1
2
```

Total:

```text
2
```

---

# 🔄 Transition Formula

```java
dp[i] = dp[i-1] + dp[i-2];
```

---

# 🧩 Why This Formula Works

Suppose:

```text
i = 5
```

To reach stair `5`:

---

## Case 1: Last move was 1 step

Then before that you were on stair:

```text
4
```

Ways:

```text
dp[4]
```

---

## Case 2: Last move was 2 steps

Then before that you were on stair:

```text
3
```

Ways:

```text
dp[3]
```

---

## Total Ways

```text
dp[5] = dp[4] + dp[3]
```

---

# 📊 Dry Run

Input:

```text
n = 5
```

---

## Initial

```text
dp[1] = 1
dp[2] = 2
```

---

## i = 3

```text
dp[3] = dp[2] + dp[1]
      = 2 + 1
      = 3
```

---

## i = 4

```text
dp[4] = dp[3] + dp[2]
      = 3 + 2
      = 5
```

---

## i = 5

```text
dp[5] = dp[4] + dp[3]
      = 5 + 3
      = 8
```

---

# ✅ Final DP Array

```text
Index: 0 1 2 3 4 5
Value: 0 1 2 3 5 8
```

Answer:

```text
8
```

---

# 🧠 Fibonacci Relation

Notice:

```text
1 2 3 5 8 ...
```

This is Fibonacci pattern.

Actually:

```text
Climbing Stairs = Fibonacci
```

---

# ⏱ Complexity

### Time

```text
O(n)
```

Single loop.

---

### Space

```text
O(n)
```

DP array.

---

# 🚀 Space Optimized Version Idea

Since only previous two values are needed:

```text
prev1
prev2
```

We can reduce space to:

```text
O(1)
```

---

# 🏆 Pattern Recognition

If problem says:

```text
Count ways
Reach target
1-step / 2-step choices
```

Think:

```text
Dynamic Programming
Fibonacci Pattern
```

---

# 🔥 Golden Mental Model

```text
Ways to reach current stair
=
Ways from previous stair
+
Ways from two stairs before
```

---

# 🚀 Final Summary

The algorithm:

1. Defines `dp[i]` as ways to reach stair `i`.
2. Uses previous results to build current result.
3. Applies:

```text
dp[i] = dp[i-1] + dp[i-2]
```

4. Returns `dp[n]`.

---

```text
Climbing Stairs = Fibonacci-style Dynamic Programming.
```

---
