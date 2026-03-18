# Bitwise vs Logical Operators in Python

> **Common Mistake:** Never use `&` / `|` for condition checks in `if` statements — use `and` / `or` instead.

---

## Part 1: Bitwise Operators (for numbers)

Bitwise operators work at the **binary (bit-level)**. They compare numbers bit by bit.

### 1. `&` — Bitwise AND

Each bit is `1` only if **both** bits are `1`.

```
5 & 3
5 = 101
3 = 011
    ---
    001 = 1
```

### 2. `|` — Bitwise OR

Each bit is `1` if **either** bit is `1`.

```
5 | 3
5 = 101
3 = 011
    ---
    111 = 7
```

### 3. `^` — Bitwise XOR

Each bit is `1` if the bits are **different**.

```
5 ^ 3
5 = 101
3 = 011
    ---
    110 = 6
```

### 4. `~` — Bitwise NOT

Flips all bits. Formula: `~n = -(n + 1)`

```
~5 = -(5 + 1) = -6
~0 = -1
~-1 = 0
```

### 5. `<<` — Left Shift

Shifts bits left, equivalent to multiplying by `2^n`.

```
5 << 1
5 = 101 → 1010 = 10   (5 × 2 = 10)
```

### 6. `>>` — Right Shift

Shifts bits right, equivalent to floor dividing by `2^n`.

```
5 >> 1
5 = 101 → 010 = 2   (5 // 2 = 2)
```

---

## Part 2: Logical Operators (for conditions)

Logical operators work on **boolean logic**. Use these in `if` statements.

### 1. `and`

Returns the **first falsy value**, or the **last value** if all are truthy.

```python
True and True    # True
True and False   # False
5 and 10         # 10   (both truthy → returns last)
0 and 10         # 0    (0 is falsy → returns first falsy)
```

### 2. `or`

Returns the **first truthy value**, or the **last value** if all are falsy.

```python
True or False    # True
False or False   # False
5 or 10          # 5    (5 is truthy → returns first truthy)
0 or 10          # 10   (0 is falsy → skips to next)
0 or ""          # ""   (both falsy → returns last)
```

### 3. `not`

Returns the **boolean inverse**.

```python
not True     # False
not False    # True
not 0        # True
not 5        # False
```

---

## Part 3: The Classic Mistake ⚠️

### ❌ Wrong — using `&` in a condition

```python
if a > b & c > d:
```

This is interpreted as:

```python
if a > (b & c) > d:   # Python applies bitwise & first (higher precedence)!
```

### ✅ Correct — use `and`

```python
if a > b and c > d:
```

---

## Part 4: Operator Precedence (high → low)

| Priority | Operator | Type |
|---|---|---|
| 1 (highest) | `~` | Bitwise NOT |
| 2 | `*`, `/`, `//`, `%` | Arithmetic |
| 3 | `+`, `-` | Arithmetic |
| 4 | `<<`, `>>` | Bit Shift |
| 5 | `&` | Bitwise AND |
| 6 | `^` | Bitwise XOR |
| 7 | `\|` | Bitwise OR |
| 8 | `<`, `>`, `<=`, `>=`, `==`, `!=` | Comparison |
| 9 | `not` | Logical NOT |
| 10 | `and` | Logical AND |
| 11 (lowest) | `or` | Logical OR |

> **Rule of thumb:** Bitwise operators always bind tighter than comparisons. Always use `and` / `or` in `if` conditions to avoid surprises.

---

## Part 5: When to Use `&` / `|`

| Use case | Example | Explanation |
|---|---|---|
| Check if a number is odd | `if x & 1:` | Last bit is `1` for odd numbers |
| Check if a number is even | `if not x & 1:` | Last bit is `0` for even numbers |
| Check if power of 2 | `if x & (x - 1) == 0:` | Powers of 2 have exactly one `1` bit |
| Toggle a bit | `x ^ (1 << n)` | Flip the nth bit |
| Set a bit | `x \| (1 << n)` | Force the nth bit to `1` |
| Clear a bit | `x & ~(1 << n)` | Force the nth bit to `0` |
| Bitmask filtering | `x & 0xFF` | Keep only the lower 8 bits |

---

## Part 6: Quick Reference

```python
# ✅ Use 'and' / 'or' for condition checks
if a > b and c > d:
if x == 0 or y == 0:

# ✅ Use & / | for bit manipulation
if x & 1:           # check if x is odd
if x & (x - 1) == 0 and x != 0:   # check if x is a power of 2
x = x | (1 << 3)   # set bit 3
x = x & ~(1 << 3)  # clear bit 3
x = x ^ (1 << 3)   # toggle bit 3
```
