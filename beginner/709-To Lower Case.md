# Python String Built-in Methods

---

## What is a String?

**Purpose:** Used to describe text — names, addresses, preferences, etc.

**Definition:** A sequence of characters wrapped in `''`, `""`, `''''''`, or `""""""`.

```python
name = 'lin'           # same as str('lin')
s1 = str(1.1)           # "1.1"
s2 = str([1, 2, 3])     # "[1, 2, 3]"
```

**Special string prefixes:**

| Prefix | Meaning | Example |
|---|---|---|
| `u'...'` | Unicode string | `u'hello'` |
| `b'...'` | Bytes string | `b'101'` |
| `r'...'` | Raw string (no escape processing) | `r'\n'` → two chars, not newline |

---

## Part 1: Must Know ★★★★★

### 1. Index Access (read-only)

```python
msg = 'hello lin'
#      012345678

msg[6]   # 'l'
msg[-3]  # 'l'   (negative index counts from the right)
```

> Strings are **immutable** — you can read by index but cannot change a character in place.

---

### 2. Slicing `[start:end:step]`

> Rule: **includes start, excludes end**

```python
msg = 'hello lin'

msg[3:]       # 'lo lin'     (from index 3 to end)
msg[3:8]      # 'lo li'       (index 3 up to but not including 8)
msg[3:8:2]    # 'l i'         (every 2nd character)
msg[::-1]     # 'nil olleh'   (reverse the string)
msg[-5:-2]    # 'o l'          (negative slicing)
```

---

### 3. `len()` — Length

```python
msg = 'hello lin'
len(msg)   # 9
```

---

### 4. `in` / `not in` — Membership Test

```python
msg = 'my name is lin, lin handsome'

'lin' in msg        # True
'jason' not in msg   # True
not 'jason' in msg   # True   (same as above)
```

---

### 5. `strip()` — Remove Characters from Both Ends

```python
name = '&&&lin'
name.strip('&')     # 'lin'  (removes & from both ends)
name                # '&&&lin'  (original unchanged — strings are immutable)

'*-& lin+'.strip('*-& +')   # 'lin'
```

> **Default:** `strip()` with no argument removes **whitespace** (spaces, tabs, newlines).

**Common use case — clean user input:**
```python
pwd = input('password: ')
if pwd.strip() == '123':     # handles accidental spaces typed by user
    print('Login success')
```

---

### 6. `split()` — Split into List

```python
info = 'lin:male:19'

info.split(':')      # ['lin', 'male', '19']
info.split(':', 1)   # ['lin', 'male:19']  (split at most 1 time)
```

---

### 7. Loop Over a String

```python
for ch in 'hello lin':
    print(ch)
# h e l l o   l i n  (one character per line)
```

---

## Part 2: Should Know ★★★★

### 1. `lstrip()` / `rstrip()` — Remove from One Side Only

```python
name = '&&lin&&'
name.lstrip('&')   # 'lin&&'   (left side only)
name.rstrip('&')   # '&&lin'   (right side only)
```

---

### 2. `lower()` / `upper()` — Change Case

```python
name = 'Lin Chen'
name.lower()   # 'lin chen'
name.upper()   # 'LIN CHEN'
```

---

### 3. `startswith()` / `endswith()`

```python
name = 'Lin Chen'
name.startswith('Lin')   # True
name.endswith('chen')     # False  (case-sensitive!)
```

---

### 4. `rsplit()` — Split from the Right

```python
info = 'lin:male:19'
info.rsplit(':', 1)   # ['lin:male', '19']  (splits from right, max 1 time)
```

> Useful when you only want the **last** segment of a string.

---

### 5. `join()` — Join a List into a String

```python
lis = ['lin', 'male', '19']
':'.join(lis)   # 'lin:male:19'
```

> **Note:** All items in the list must be strings. Numbers will raise a `TypeError`.

```python
':'.join([1, 2, '19'])   # ❌ TypeError — can't join integers
```

---

### 6. `replace()` — Replace Substring

```python
name = 'lin shuai'
name.replace('shuai', 'handsome')   # 'lin handsome'
```

> The original string is **not modified** — a new string is returned.

---

### 7. `isdigit()` — Check if All Characters Are Digits

```python
'111'.isdigit()     # True
'111.1'.isdigit()   # False  (decimal point is not a digit)
```

**Common use case — validate user input:**
```python
age = input('age: ')
if age.isdigit():
    age = int(age)
    if age < 18:
        print('minor')
    else:
        print('adult')
else:
    print(f'Invalid input: {age}')
```

---

## Part 3: Good to Know ★★

### 1. `find()` / `rfind()` / `index()` / `rindex()` / `count()`

```python
msg = 'my name is tank, tank shi sb'

msg.find('tank')        # 11   (first occurrence, returns -1 if not found)
msg.find('tank', 0, 3)  # -1   (search only within range [0:3])
msg.rfind('tank')       # 17   (last occurrence, returns -1 if not found)
msg.index('tank')       # 11   (like find(), but raises ValueError if not found)
msg.rindex('tank')      # 17   (like rfind(), but raises ValueError if not found)
msg.count('tank')       # 2    (how many times it appears)
```

| Method | Not Found Behavior |
|---|---|
| `find()` / `rfind()` | Returns `-1` (safe) |
| `index()` / `rindex()` | Raises `ValueError` ⚠️ |

---

### 2. `center()` / `ljust()` / `rjust()` / `zfill()` — Padding

```python
'info lin'.center(50, '*')   # *********************info lin*********************
'info lin'.ljust(50, '*')    # info lin******************************************
'info lin'.rjust(50, '*')    # ******************************************info lin
'info lin'.zfill(50)         # 000000000000000000000000000000000000000000info lin
```

---

### 3. `expandtabs()` — Expand Tab Characters

```python
'a\tb\tc'.expandtabs(8)    # default: 8 spaces per tab
'a\tb\tc'.expandtabs(32)   # 32 spaces per tab
```

---

### 4. `capitalize()` / `swapcase()` / `title()`

```python
name = 'lin handsome sWAPCASE'

name.capitalize()   # 'Lin handsome swapcase'  (first letter upper, rest lower)
name.swapcase()     # 'LIN HANDSOME Swapcase'  (flip every letter's case)
name.title()        # 'Lin Handsome Swapcase'  (capitalize first letter of each word)
```

---

### 5. `is` Digit Series — Comparison Table

| Method | `'1'` | `b'1'` | Roman `'Ⅷ'` | Chinese `'四'` |
|---|---|---|---|---|
| `isdigit()` | ✅ | ✅ | ✅ | ❌ |
| `isdecimal()` | ✅ | ❌ Error | ❌ | ❌ |
| `isnumeric()` | ✅ | ❌ Error | ✅ | ✅ |

> **Rule of thumb:** Just use `isdigit()` for almost all practical cases.

---

### 6. Other `is` Checks

```python
'abc123'.isalnum()       # True  — all letters or digits
'abc'.isalpha()          # True  — all letters only
'abc'.islower()          # True  — all lowercase
'ABC'.isupper()          # True  — all uppercase
'   '.isspace()          # True  — only whitespace
'Hello World'.istitle()  # True  — each word starts with uppercase
```

---

## Part 4: String Properties

| Property | Value |
|---|---|
| Stores | One value (the whole string) |
| Ordered | ✅ Yes — has index |
| Mutable | ❌ No — immutable |

```python
name = 'lin'
id(name)               # e.g. 4377100160
name = 'lin handsome'
id(name)               # e.g. 4377841264  ← different object in memory
```

> Reassigning a string variable doesn't modify the original string — it creates a **new string object** in memory.

---

## Part 5: Comparing Strings with ASCII

### What is ASCII?

Every character is stored as a number internally. **ASCII** is the mapping between characters and those numbers.

```python
ord('A')   # 65  — get ASCII value of a character
ord('a')   # 97
ord('0')   # 48
chr(65)    # 'A' — get character from ASCII value
chr(97)    # 'a'
```

### ASCII Order (low → high)

```
Space(32) < Digits 0-9 (48–57) < Uppercase A-Z (65–90) < Lowercase a-z (97–122)
```

### How Python Compares Strings

Python compares strings **character by character from left to right**, using ASCII values at each position.

```python
'a' > 'A'       # True  — ord('a')=97 > ord('A')=65
'b' > 'a'       # True  — ord('b')=98 > ord('a')=97
'A' < 'a'       # True  — uppercase always < lowercase
'9' < 'A'       # True  — digits(57) < uppercase(65)

'abc' < 'abd'   # True  — 'c'(99) < 'd'(100) at index 2
'abc' < 'abcd'  # True  — equal prefix, shorter string is smaller
'apple' > 'Banana'  # True  — 'a'(97) > 'B'(66) at index 0
```

> **Key rule:** Comparison stops at the **first character that differs**. If one string is a prefix of another, the shorter one is smaller.

---

### ASCII Tricks for LeetCode

```python
# Check if character is lowercase letter
'a' <= ch <= 'z'

# Check if character is uppercase letter
'A' <= ch <= 'Z'

# Check if character is a digit character
'0' <= ch <= '9'

# Get 0-indexed position in alphabet
ord('c') - ord('a')   # 2   ('a'=0, 'b'=1, 'c'=2, ...)
ord('C') - ord('A')   # 2

# Get numeric value from digit character
ord('7') - ord('0')   # 7   (useful for parsing without int())

# Sort strings — uses ASCII order by default
sorted(['banana', 'Apple', 'cherry'])
# ['Apple', 'banana', 'cherry'] — uppercase 'A'(65) < lowercase 'b'(98)

# Case-insensitive sort
sorted(['banana', 'Apple', 'cherry'], key=str.lower)
# ['Apple', 'banana', 'cherry']
```

---

### Common ASCII Values to Remember

| Character | ASCII Range |
|---|---|
| `' '` space | 32 |
| `'0'` – `'9'` | 48 – 57 |
| `'A'` – `'Z'` | 65 – 90 |
| `'a'` – `'z'` | 97 – 122 |

> **Useful trick:** `ord('a') - ord('A') == 32`
> This means you can convert between upper and lowercase using `+32` / `-32`, which is why bitwise tricks like `ch | 32` (to lowercase) and `ch & ~32` (to uppercase) work at the bit level.

```python
# Convert lowercase to uppercase using ASCII math
chr(ord('a') - 32)   # 'A'
chr(ord('z') - 32)   # 'Z'

# LeetCode pattern: map each letter to a frequency index
freq = [0] * 26
for ch in 'hello':
    freq[ord(ch) - ord('a')] += 1
# freq[7] = 1  (h), freq[4] = 1  (e), freq[11] = 2  (l), freq[14] = 1  (o)
```
