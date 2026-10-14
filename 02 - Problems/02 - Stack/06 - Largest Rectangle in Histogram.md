---
date_created: 2026-08-23
tags:
  - dsa/problem
  - dsa/pattern/stack
  - dsa/pattern/monotonic_stack
  - status/solved
platform: NeetCode
difficulty: Hard
pattern: Monotonic Stack
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-23
next_review:
---

# [[Largest Rectangle in Histogram]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/largest-rectangle-in-histogram/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Every rectangle in the histogram has some bar as its limiting minimum height. Instead of checking every possible range, find how far each bar can extend left and right before encountering a smaller bar.
>
> A monotonic increasing stack helps us find these boundaries efficiently. A bar stays in the stack while we haven't found a smaller bar to its right. When a smaller bar appears, we pop taller bars because their maximum possible right boundary has now been found.

---

# ⚙️ Approaches

## Approach 1: Brute Force — Try Every Range

### Idea

Consider every possible contiguous range:

```text
[left ... right]
````

The largest rectangle covering that range has:

```text
height = minimum height in the range
width  = right - left + 1
area   = height × width
```

Instead of finding the minimum from scratch for every range, maintain a running minimum while expanding `right`.

### Example

```text
heights = [2, 1, 5, 6, 2, 3]

Starting with left = 2:

[5]          minHeight = 5, width = 1 → area = 5
[5, 6]       minHeight = 5, width = 2 → area = 10
[5, 6, 2]    minHeight = 2, width = 3 → area = 6
```

### Code

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        maxArea = 0

        for left in range(n):
            minHeight = heights[left]

            for right in range(left, n):
                if heights[right] < minHeight:
                    minHeight = heights[right]

                area = minHeight * (right - left + 1)
                maxArea = max(maxArea, area)

        return maxArea
```

### Complexity

- **Time:** $O(n^2)$
    
- **Space:** $O(1)$
    

---

## Approach 2: Each Bar as the Minimum Height

### Idea

Instead of asking:

> What is the minimum height for every possible range?

Flip the problem:

> For each bar, how far can this bar extend left and right while remaining the minimum height?

For each bar:

1. Expand left while bars are greater than or equal to the current height.
    
2. Expand right while bars are greater than or equal to the current height.
    
3. Calculate:
    

```text
area = current height × total width
```

### Example

```text
[2, 1, 5, 6, 2, 3]
       ↑
       5
```

For height `5`:

```text
left  → blocked by 1
right → can include 6, blocked by 2
```

So:

```text
[5, 6]

height = 5
width = 2
area = 10
```

### Code

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        maxArea = 0

        for i in range(n):
            height = heights[i]

            j = i + 1
            k = i - 1

            while j < n and heights[j] >= height:
                j += 1

            while k >= 0 and heights[k] >= height:
                k -= 1

            width = j - k - 1
            maxArea = max(maxArea, height * width)

        return maxArea
```

### Complexity

- **Time:** $O(n^2)$
    
- **Space:** $O(1)$
    

### Problem With This Approach

We repeatedly search for smaller bars.

For example, multiple bars may repeatedly scan through the same region:

```text
[2, 1, 5, 6, 2, 3]
```

The optimized solution avoids repeating those searches.

---

# Approach 3: Monotonic Increasing Stack — Optimal

## The Main Question

For every bar, we want to know:

```text
Where is the first smaller bar on my left?
Where is the first smaller bar on my right?
```

Once we know both, we know the maximum width where that bar can be the limiting height.

The stack helps us discover these boundaries efficiently.

---

## 🧠 What Does the Stack Store?

The stack stores **indices** of bars.

The heights corresponding to those indices are in **increasing order**.

For example:

```text
heights = [2, 1, 5, 6]

stack indices: [1, 2, 3]
stack heights: [1, 5, 6]
```

The stack is increasing:

```text
1 < 5 < 6
```

### Why keep bars in the stack?

A bar in the stack is basically saying:

> "I have not found a smaller bar to my right yet."

So it is waiting.

---

# 🚨 The Important Moment: A Smaller Bar Appears

Consider:

```text
[2, 1, 5, 6, 2, 3]
          ↑  ↑
          6  current = 2
```

Before processing `2`, the stack contains:

```text
[1, 5, 6]
```

Now:

```text
6 > 2
```

Therefore, `6` must be popped.

Why?

Because `2` is the **first smaller bar to the right of 6**.

So now we finally know that `6` cannot extend beyond this point.

---

## Calculating the Area When Popping

Suppose we pop `6`.

```python
popped = stack.pop()
```

### Right boundary

The current index `i` contains a smaller bar.

Therefore:

```text
right smaller boundary = i
```

The actual rectangle ends at:

```text
i - 1
```

---

### Left boundary

After popping `6`, the new top of the stack is the nearest smaller bar on the left.

For example:

```text
stack before: [1, 5, 6]

pop 6

stack after:  [1, 5]
                   ↑
             smaller than 6
```

So:

```text
left smaller boundary = stack[-1]
```

The actual rectangle starts at:

```text
stack[-1] + 1
```

---

## Width Formula

Visually:

```text
left smaller        valid rectangle        right smaller
     |                    |                     |
     v                    v                     v

    index L          L+1 ...... i-1            index i
```

Therefore:

```text
width = (i - 1) - (L + 1) + 1
```

Simplifying:

```python
width = i - L - 1
```

So:

```python
popped = stack.pop()

if stack:
    width = i - stack[-1] - 1
else:
    width = i
```

Then:

```python
area = heights[popped] * width
```

---

# Example: Why `5` Gets Area 10

Consider:

```text
index:    0  1  2  3  4
height:   2  1  5  6  2
                ↑        ↑
                5      current
```

When we reach the `2` at index `4`:

First, `6` is popped.

Then `5` is also popped because:

```text
5 > 2
```

After popping `5`:

```text
index 1 → height 1 → smaller on the left
index 4 → height 2 → smaller on the right
```

So `5` can cover:

```text
index:     2   3
height:    5   6
```

Width:

```text
4 - 1 - 1 = 2
```

Area:

```text
5 × 2 = 10
```

---

# Why Do We Calculate Area During Popping?

This was the main confusing part.

When we **append** a bar, we don't know how far it can extend to the right.

For example:

```text
[1, 5, 6]
```

When we see `5`, we don't know what comes next.

Maybe:

```text
[1, 5, 6, 7, 8, 9]
```

Then `5` can extend very far.

But maybe:

```text
[1, 5, 6, 2]
```

Now `5` must stop before `2`.

So when a bar is pushed:

> We don't know its final maximum rectangle yet.

When a smaller bar appears and causes it to be popped:

> Now we know exactly where its rectangle ends.

### Mental Model

```text
PUSH
↓
"This bar is waiting for its first smaller bar on the right."


POP
↓
"A smaller bar arrived. I now know this bar's right boundary."
```

---

# What Happens to Bars Still in the Stack?

Consider an increasing histogram:

```text
[1, 2, 3, 4]
```

Nothing is smaller than the previous bar.

So nothing gets popped:

```text
stack = [0, 1, 2, 3]
```

But we still need to calculate their areas.

### Solution: Add a Sentinel `0`

```python
heights.append(0)
```

Now:

```text
[1, 2, 3, 4, 0]
```

The `0` is smaller than every bar:

```text
4 > 0 → pop
3 > 0 → pop
2 > 0 → pop
1 > 0 → pop
```

This forces all remaining bars to calculate their final area.

The extra `0` is not part of any rectangle.

Its only purpose is:

> Force the stack to empty at the end.

---

## Final Optimal Code

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        heights.append(0)

        maxArea = 0
        stack = []

        for i, curr in enumerate(heights):

            # A smaller bar means we found the right boundary
            # for all taller bars in the stack.
            while stack and heights[stack[-1]] > curr:

                popped = stack.pop()

                # If stack is not empty, stack[-1] is the
                # first smaller bar on the left.
                if stack:
                    width = i - stack[-1] - 1
                else:
                    # No smaller bar on the left.
                    # Rectangle extends back to index 0.
                    width = i

                area = heights[popped] * width
                maxArea = max(maxArea, area)

            stack.append(i)

        return maxArea
```

---

## Step-by-Step Stack Flow

For:

```text
[2, 1, 5, 6, 2, 3, 0]
```

### `2`

```text
stack = [2]
```

Waiting for a smaller bar.

---

### `1`

```text
2 > 1
```

Pop `2`.

```text
area = 2 × 1 = 2
```

Push `1`:

```text
stack = [1]
```

---

### `5`

```text
1 < 5
```

Push:

```text
stack = [1, 5]
```

---

### `6`

```text
5 < 6
```

Push:

```text
stack = [1, 5, 6]
```

---

### `2`

Now:

```text
6 > 2
```

Pop `6`.

```text
area = 6 × 1 = 6
```

Still:

```text
5 > 2
```

Pop `5`.

```text
area = 5 × 2 = 10
```

Now:

```text
1 < 2
```

Stop popping and push `2`.

```text
stack = [1, 2]
```

---

### `3`

```text
2 < 3
```

Push:

```text
stack = [1, 2, 3]
```

---

### Sentinel `0`

Eventually `0` causes every remaining bar to pop and calculate its final possible rectangle.

---

## ⚠️ Pitfalls & Edge Cases

-  A bar is popped when a **strictly smaller** height appears.
    
-  The current index `i` is the first smaller bar on the **right**.
    
-  After popping, `stack[-1]` is the first smaller bar on the **left**.
    
-  The width is:
    

```text
i - stack[-1] - 1
```

-  If the stack is empty after popping:
    

```text
width = i
```

-  An increasing array needs the sentinel `0` to force remaining bars to pop.
    
-  The stack stores **indices**, not heights, because indices are needed to calculate width.
    
-  `heights.append(0)` modifies the input. Use `heights = heights + [0]` if you want to avoid that.
    

---

## 🔁 Spaced Repetition Tracker

|Review|Date|Solved Without Hints? (Y/N)|Notes / Reflections|
|---|---|---|---|
|**1st Pass**|2026-08-23|N|Independently derived the O(n²) brute-force and boundary-expansion approaches. Needed guided hints to understand the monotonic stack, specifically why areas are calculated during popping, how the width formula works, and why a sentinel `0` is needed.|
|**2nd Pass**||||
|**3rd Pass**||||

---

## 🔗 Related Questions & Pattern Links

- Parent Pattern Note: [[Pattern - Monotonic Stack]]
    
- Parent Pattern Note: [[Pattern - Stack]]
    
- Similar Problem: [[Daily Temperatures]]
    
- Similar Problem: [[Car Fleet]]
    
- Similar Problem: [[Next Greater Element]]
    
- Similar Problem: [[Trapping Rain Water]]