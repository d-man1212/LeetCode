---
date_created: 2026-08-20
tags:
  - dsa/problem
  - dsa/pattern/stack
  - dsa/pattern/monotonic_stack
  - status/solved
platform: NeetCode
difficulty: Medium
pattern: Stack / Monotonic Stack
sheet: NeetCode 150
times_solved: 1
last_reviewed: 2026-08-20
next_review:
---

# [[Car Fleet]]

- **Problem Link:** [NeetCode](https://neetcode.io/problems/car-fleet/question)
- **Difficulty:** `= this.difficulty`
- **Pattern:** `= this.pattern`

---

## 💡 Key Takeaway & Core Intuition

> Sort cars by position from closest to the target to farthest. Calculate each car's time to reach the target. If a car behind takes less than or equal to the time of the fleet ahead, it catches that fleet and becomes part of it. Otherwise, it forms a new fleet.

### Important Rule

- `curr_time > ahead_time` → **New fleet**
- `curr_time <= ahead_time` → **Joins fleet**
- Equal arrival times mean the car catches the fleet exactly at the target, so they still count as **one fleet**.

---

## ⚙️ Approaches

### Approach 1: Brute Force Simulation

- **Idea:** Simulate the movement of every car and repeatedly check whether a faster car catches the car ahead. Once cars meet, merge them into a fleet.
- **Time Complexity:** Inefficient / depends on simulation approach, potentially **O(n²)** or worse.
- **Space Complexity:** O(n)

> Not practical for large inputs and unnecessary because we can reason about the cars using their time to reach the target.

---

### Approach 2: Sort + Track Maximum Fleet Time

This is the approach I independently implemented.

- **Idea:** Sort `(position, speed)` pairs in descending order of position, so we process cars from closest to the target to farthest. Track the arrival time of the fleet ahead.
- **Time Complexity:** O(n log n)
- **Space Complexity:** O(n)

```python
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        sorted_items = sorted(zip(position, speed), reverse=True)

        fleet = 0
        aheadTime = 0

        for curr_position, curr_speed in sorted_items:
            curr = (target - curr_position) / curr_speed

            if curr > aheadTime:
                fleet += 1
                aheadTime = curr

        return fleet
````

### Why `aheadTime` Works

Suppose the fleet ahead reaches the target in `5` hours:

* Current car takes `7` hours → cannot catch up → **new fleet**
* Current car takes `3` hours → catches up before the target → **joins fleet**
* Current car takes `5` hours → catches up exactly at the target → **joins fleet**

When a car joins a fleet, the fleet's arrival time does not change, so we keep the same `aheadTime`.

---

### Approach 3: Monotonic Stack

Instead of keeping only the latest fleet time, explicitly maintain a stack of fleet arrival times.

* **Idea:** After sorting cars from closest to farthest, calculate each car's arrival time.
* Push the time onto the stack.
* If the current car catches the fleet ahead, remove its separate entry because both cars belong to the same fleet.

```python
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        cars = sorted(zip(position, speed), reverse=True)

        stack = []

        for pos, spd in cars:
            time = (target - pos) / spd

            if stack and time <= stack[-1]:
                continue

            stack.append(time)

        return len(stack)
```

### Stack Interpretation

The stack stores the arrival time of each distinct fleet.

For example, after processing cars:

```text
stack = [2.0, 5.0, 7.0]
```

This means there are currently three fleets.

If the next car has:

```text
time = 4.0
```

Then:

```text
4.0 <= 7.0
```

It catches the fleet ahead, so it does not create a new fleet.

The arrival times of distinct fleets are therefore strictly increasing as we move from the target toward the starting positions.

---

### Approach 4: Stack Using the Common Push-and-Pop Style

Another way to write the same monotonic stack idea is to first push every arrival time.

```python
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        cars = sorted(zip(position, speed))

        stack = []

        for pos, spd in cars:
            time = (target - pos) / spd
            stack.append(time)

            if len(stack) >= 2 and stack[-1] <= stack[-2]:
                stack.pop()

        return len(stack)
```

Here we process cars from **farthest to closest**.

After pushing a car's arrival time:

* If `stack[-1] <= stack[-2]`, the car behind reaches the target earlier or at the same time as the car ahead.
* Therefore, it catches the car/fleet ahead.
* The two merge, so the current car's separate fleet entry is removed.

This version makes the **monotonic stack behavior** more explicit.

---

## ⚠️ Pitfalls & Edge Cases

* [x] Cars cannot pass each other
* [x] A faster car does not always form a fleet with the car ahead; it must catch it before or exactly at the target
* [x] Same speed means the distance between cars never changes
* [x] Equal arrival times → same fleet
* [x] Use `>` when checking whether a new fleet is formed
* [x] Multiple cars can merge into the same fleet
* [x] After merging, the fleet travels at the speed of the slowest car
* [x] Sort by **position**, not by speed
* [x] Process from closest to the target when using the `aheadTime` approach

---

## 🔁 Spaced Repetition Tracker

| Review       | Date       | Solved Without Hints? (Y/N) | Notes / Reflections                                                                                                                                                                                                                                      |
| ------------ | ---------- | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1st Pass** | 2026-08-20 | Y                           | Initially needed clarification on what happens when a car catches another car and when two cars have the same speed. After understanding the rules, independently derived the optimal sort + fleet arrival time solution and refined the implementation. |
| **2nd Pass** |            |                             |                                                                                                                                                                                                                                                          |
| **3rd Pass** |            |                             |                                                                                                                                                                                                                                                          |

---

## 🔗 Related Questions & Pattern Links

* Parent Pattern Note: [[Pattern - Monotonic Stack]]
* Parent Pattern Note: [[Pattern - Stack]]
* Similar Problem: [[Daily Temperatures]]
* Similar Problem: [[Largest Rectangle in Histogram]]
* Similar Problem: [[Next Greater Element]]
