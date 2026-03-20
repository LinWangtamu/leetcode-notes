# 867. Transpose Matrix

## Problem

Given a 2D matrix, return its **transpose** — swap the row and column indices of each element.

```
Original:        Transposed:
1 2 3            1 4 7
4 5 6    →       2 5 8
7 8 9            3 6 9
```

Formally: `result[j][i] = matrix[i][j]`

---

## Thought Process

### Step 1 — Figure out the new matrix dimensions

The transpose swaps rows and columns:

| | Original | Transposed |
|---|---|---|
| Rows | `r` | `c` (old col count) |
| Cols | `c` | `r` (old row count) |

```python
old_rows = len(matrix)
old_cols = len(matrix[0])

new_rows = old_cols   # transposed
new_cols = old_rows   # transposed
```

> **Why this matters:** The original matrix may **not be square** (e.g. 3×2 → transposed is 2×3). Always derive dimensions from the input, never assume rows == cols.

---

### Step 2 — Build an empty result container

We need a 2D list of size `new_rows × new_cols`, pre-filled with `0`.

```python
result = [[0] * new_cols for _ in range(new_rows)]
```

> ⚠️ **Never use** `[[0] * new_cols] * new_rows` — this creates copies of the **same row reference**, so modifying one row changes all of them.

```python
# ❌ Wrong
result = [[0] * new_cols] * new_rows
result[0][0] = 9   # this changes result[1][0], result[2][0], etc. too!

# ✅ Correct
result = [[0] * new_cols for _ in range(new_rows)]
```

---

### Step 3 — Fill with two for loops

```python
for i in range(old_rows):
    for j in range(old_cols):
        result[j][i] = matrix[i][j]
```

> ⚠️ **Assignment direction matters:**
> - ✅ `result[j][i] = matrix[i][j]` — correct
> - ❌ `result[i][j] = matrix[j][i]` — index out of range if matrix is not square

**Why does the wrong direction cause an index error?**

Say the original matrix is `3 rows × 2 cols`:
- `i` goes up to 2 (old rows), `j` goes up to 1 (old cols)
- `result` is `2 rows × 3 cols`
- If you write `result[i][j]`, you'd access `result[2][...]` — but result only has rows 0 and 1 → **IndexError**

---

## Full Solution

```python
class Solution:
    def transpose(self, matrix: List[List[int]]) -> List[List[int]]:
        old_rows = len(matrix)
        old_cols = len(matrix[0])

        result = [[0] * old_rows for _ in range(old_cols)]

        for i in range(old_rows):
            for j in range(old_cols):
                result[j][i] = matrix[i][j]

        return result
```

---

## Worked Example

Input:
```
matrix = [[1, 2, 3],
           [4, 5, 6]]
# 2 rows × 3 cols
```

Step 1 — dimensions:
```
old_rows = 2, old_cols = 3
result shape = 3 rows × 2 cols
```

Step 2 — empty result:
```
result = [[0, 0],
          [0, 0],
          [0, 0]]
```

Step 3 — fill:
```
result[0][0] = matrix[0][0] = 1
result[1][0] = matrix[0][1] = 2
result[2][0] = matrix[0][2] = 3
result[0][1] = matrix[1][0] = 4
result[1][1] = matrix[1][1] = 5
result[2][1] = matrix[1][2] = 6
```

Output:
```
[[1, 4],
 [2, 5],
 [3, 6]]
```

---

## Alternative: One-liner with `zip`

```python
class Solution:
    def transpose(self, matrix: List[List[int]]) -> List[List[int]]:
        return [list(row) for row in zip(*matrix)]
```

**How it works:**
- `*matrix` unpacks the matrix into individual rows as separate arguments
- `zip(*matrix)` groups together all elements at the same column index → effectively transposes
- `list(row)` converts each zip tuple into a list

```python
matrix = [[1, 2, 3],
          [4, 5, 6]]

zip(*matrix)
# → (1, 4), (2, 5), (3, 6)

[list(row) for row in zip(*matrix)]
# → [[1, 4], [2, 5], [3, 6]]
```

---

## Complexity

| | Two-loop | `zip` one-liner |
|---|---|---|
| Time | O(m × n) | O(m × n) |
| Space | O(m × n) | O(m × n) |

Both approaches have the same complexity. The two-loop version is more beginner-friendly and explicit; the `zip` version is more Pythonic.

---

## Key Takeaways

| Point | Detail |
|---|---|
| New shape | `new_rows = old_cols`, `new_cols = old_rows` |
| Assignment direction | Always `result[j][i] = matrix[i][j]` |
| Reversed direction | Causes `IndexError` on non-square matrices |
| Container creation | Use list comprehension, never `* n` row copy |
| Edge case | Single row or single column — both work correctly with this approach |
