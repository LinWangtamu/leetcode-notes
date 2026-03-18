# 2236. Root Equals Sum of Children

## What is a Class?

A **class** is a blueprint/template for creating objects. An **object** is a real instance built from that template.

---

## Example 1: A Simple `Person` Class

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

| Part | What it does |
|---|---|
| `class Person` | Defines the blueprint |
| `__init__` | Runs automatically when an object is created |
| `self` | Refers to the object itself — Python passes it in automatically |
| `self.name`, `self.age` | **Attributes** — data stored inside the object |

**Creating an object:**

```python
p1 = Person("Alice", 20)
p1.name  # "Alice"
p1.age   # 20
```

`p1` is an **instance** (object) of the class `Person`.

---

## Example 2: A `TreeNode` Class

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

Each `TreeNode` object has **three attributes**:

| Attribute | Meaning |
|---|---|
| `self.val` | The value stored in this node |
| `self.left` | The left child (another `TreeNode`, or `None`) |
| `self.right` | The right child (another `TreeNode`, or `None`) |

**Building a tree step by step:**

```python
root = TreeNode(10)
# root.val = 10, root.left = None, root.right = None

root.left = TreeNode(4)
# root.left is a NEW TreeNode object with:
#   root.left.val   = 4
#   root.left.left  = None
#   root.left.right = None
```

> **Key idea:** Each node is its own **object**. Because `root.left` is a `TreeNode` object (not just a number), you can go deeper — e.g. `root.left.left = TreeNode(2)`. If it were just a plain value, you couldn't do that.

---

## The LeetCode Solution Class

```python
class Solution:
    def checkTree(self, root: Optional[TreeNode]) -> bool:
```

Breaking this down:

| Part | Meaning |
|---|---|
| `class Solution` | A wrapper class (LeetCode requires this format) |
| `def checkTree(...)` | The method (function) we define inside it |
| `self` | The `Solution` object itself — passed in automatically |
| `root: Optional[TreeNode]` | Input: a `TreeNode` object, or `None` |
| `-> bool` | Output: must return `True` or `False` |

**How LeetCode actually calls your code behind the scenes:**

```python
root = TreeNode(5, TreeNode(3), TreeNode(1))
#       val=5
#      /     \
#   val=3   val=1

Solution().checkTree(root)
# Solution()         → creates a Solution object
# .checkTree(root)   → calls the method on that object
```

---

## The Solution

```python
class Solution:
    def checkTree(self, root: Optional[TreeNode]) -> bool:
        return root.val == root.left.val + root.right.val
```

Since the problem guarantees the tree always has exactly a root, a left child, and a right child, we simply check whether the root's value equals the sum of its two children's values.
