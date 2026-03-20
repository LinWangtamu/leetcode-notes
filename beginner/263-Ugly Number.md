
# 263. Ugly Number

## Problem

An **ugly number** is a positive integer whose only prime factors are `2`, `3`, and `5`.

```
1  → True   (1 is always ugly by definition)
6  → True   (6 = 2 × 3)
14 → False  (14 = 2 × 7, contains factor 7)
0  → False  (not a positive integer)
-6 → False  (not a positive integer)
```

---

## Core Idea

Keep dividing `n` by `2`, `3`, and `5` until none of them divide evenly.

- If `n` reduces down to `1` → it had no other prime factors → **ugly**
- If `n` gets stuck at something > 1 → it still has a prime factor other than 2/3/5 → **not ugly**

```
12 → ÷2 → 6 → ÷2 → 3 → ÷3 → 1  ✅
14 → ÷2 → 7 → stuck (7 % 2 ≠ 0, 7 % 3 ≠ 0, 7 % 5 ≠ 0) → 7 ≠ 1  ❌
```

---

## Version 1 — First Attempt (with bugs)

```python
class Solution:
    def isUgly(self, n: int) -> bool:
        if n == 1:
            return True
        if n == 0 or n == -1:
            return False
        for i in range(100):
            if n % 2 == 0:
                n = n / 2        # ⚠️ Bug 1: should be //= (integer division)
            elif n % 3 == 0:
                n = n / 3        # ⚠️ Bug 1: same issue
            elif n % 5 == 0:
                n = n / 5        # ⚠️ Bug 1: same issue
            else:
                return False
            if n == 1:           # ⚠️ Bug 2: indentation inside else block
                return True      #    only reaches here after returning False
```

### Bug 1 — `/` vs `//`

Using `/` (true division) turns `n` into a **float**:

```python
6 / 2     # 3.0  ← float
6 // 2    # 3    ← int  ✅
```

This causes `n % 2 == 0` to work unexpectedly on floats, and `n == 1` may never match
because you're comparing `1.0 == 1` — which happens to be `True` in Python, but it's
fragile. Always use `//=` to keep `n` an integer throughout.

### Bug 2 — Indentation

```python
# ❌ Wrong: the if n == 1 check is inside the else block
else:
    return False
    if n == 1:       # unreachable — return False already exited
        return True

# ✅ Correct: the if n == 1 check should be at the loop level
else:
    return False
if n == 1:           # checked after each loop iteration
    return True
```

### Bug 3 — `for i in range(100)` is fragile

Using a fixed loop count of 100 assumes the number will always be reduced in under 100 steps.
For very large inputs this could silently fail. A `while` loop is the correct tool here —
keep dividing until the factor no longer divides, with no artificial ceiling.

### Bug 4 — Incomplete edge case check

```python
if n == 0 or n == -1:
    return False
```

This only catches `0` and `-1`, but any negative number should return `False`. Use `n <= 0`.

---

## Version 2 — Clean Solution ✅

```python
class Solution:
    def isUgly(self, n: int) -> bool:
        if n <= 0:
            return False
        while n % 2 == 0:
            n //= 2
        while n % 3 == 0:
            n //= 3
        while n % 5 == 0:
            n //= 5
        return n == 1
```

### Why three separate `while` loops instead of `if / elif`?

With `if / elif`, each iteration only divides by **one** factor — you need to cycle through
the loop multiple times to fully remove a factor.

```
# if/elif on n=12:
# iteration 1: 12 % 2 == 0 → n = 6   (only divided once)
# iteration 2: 6  % 2 == 0 → n = 3
# iteration 3: 3  % 3 == 0 → n = 1   ✅ (works, but needs exactly the right loop count)
```

With three separate `while` loops, each factor is **completely eliminated** before moving on:

```
n = 12
while n % 2 == 0: n //= 2   → 12 → 6 → 3    (all 2s removed)
while n % 3 == 0: n //= 3   → 3 → 1          (all 3s removed)
while n % 5 == 0: n //= 5   → 1              (no 5s to remove)
return 1 == 1  → True ✅
```

### Compact alternative using a loop over factors

```python
class Solution:
    def isUgly(self, n: int) -> bool:
        if n <= 0:
            return False
        for factor in [2, 3, 5]:
            while n % factor == 0:
                n //= factor
        return n == 1
```

Both solutions are identical in logic. The `for factor in [2, 3, 5]` version is
slightly more concise and easy to extend if the factor list ever changes.

---

## Worked Examples

| Input | Steps | Output |
|---|---|---|
| `n = 1` | No divisions needed, `1 == 1` | `True` |
| `n = 6` | `6 ÷ 2 = 3`, `3 ÷ 3 = 1` | `True` |
| `n = 8` | `8 ÷ 2 = 4 ÷ 2 = 2 ÷ 2 = 1` | `True` |
| `n = 14` | `14 ÷ 2 = 7`, stuck at `7` | `False` |
| `n = 0` | Early return (`n <= 0`) | `False` |
| `n = -6` | Early return (`n <= 0`) | `False` |

---

## Complexity

| | Value |
|---|---|
| Time | O(log n) — each division reduces n by at least half |
| Space | O(1) — no extra data structures |

---

## Key Takeaways

| Point | Detail |
|---|---|
| Edge case | `n <= 0` always returns `False` — don't just check `0` and `-1` |
| Integer division | Always use `//=`, never `/=` when you need `n` to stay an integer |
| `while` vs `if/elif` | `while` fully eliminates a factor; `if/elif` only removes it once per iteration |
| Loop ceiling | Never use `for i in range(N)` as a substitute for a proper `while` condition |
| Final check | After removing all 2s, 3s, 5s — if `n == 1` it's ugly, otherwise it's not |
