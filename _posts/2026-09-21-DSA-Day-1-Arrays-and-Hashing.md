---
layout: post
title:  "DSA Day 1 - Arrays and Hashing"
date:   2026-09-21
desc: "Day 1 of my DSA prep: Two Sum, Valid Anagram and Contains Duplicate, plus the Python data structures, dict tricks, enumerate and ternary lessons that came with them."
---

# DSA Day 1 - Arrays & Hashing

Day 1 of my Month 1 AI/FDE interview prep. The focus was **Arrays & Hashing**, and I solved three easy problems: **Two Sum**, **Valid Anagram** and **Contains Duplicate**. Below are the notes I want to keep for future review.

---

## Core Data Structures

| Structure | Ordered? | Mutable? | Duplicates? | Membership check (`in`) | Dict key? |
|---|---|---|---|---|---|
| `list` | Yes | Yes | Yes | O(n) | No |
| `tuple` | Yes | No | Yes | O(n) | Yes |
| `set` | No | Yes | No, auto-deduped | O(1) | No |
| `dict` | Yes (insertion order, 3.7+) | Yes | Keys unique | O(1) on keys | - |

**Decision rule:**

- Need order or duplicates? Use a **list** or **tuple**.
- Need fast lookup or uniqueness? Use a **set**.
- Need a key-to-value mapping? Use a **dict**.

---

## Tuple vs Set vs List

```python
lst = [1, 2, 2, 3]   # [1, 2, 2, 3] - order kept, duplicates kept, mutable
tup = (1, 2, 2, 3)   # (1, 2, 2, 3) - order kept, duplicates kept, immutable
st  = {1, 2, 2, 3}   # {1, 2, 3}    - duplicates silently removed, no guaranteed order
```

- **List**: the default choice. Use it when order matters and the data needs to change.
- **Tuple**: immutable, and usable as a dict key (e.g. `(row, col)` coordinates in grid problems).
- **Set**: the moment a problem implies "have I seen this?" or "unique elements only".

---

## Dict / Hash Map Essentials

```python
d = {}
d[key] = d.get(key, 0) + 1        # safe increment, no if/else needed
d.setdefault(key, []).append(x)   # group into lists

from collections import Counter, defaultdict
count = Counter(arr)              # frequency map in one line
count.most_common(k)              # top-k frequent elements

dd = defaultdict(list)            # auto-creates an empty list per key
dd = defaultdict(int)             # auto-creates 0 per key
```

### Comparing two dicts

`d1 == d2` checks **all keys and values** automatically, and order doesn't matter. This is exactly how anagram frequency maps are compared:

```python
from collections import Counter

def is_anagram(s1, s2):
    return Counter(s1) == Counter(s2)
```

To see *what's different* instead of just True/False:

```python
d1.keys() - d2.keys()   # keys only in d1
d2.keys() - d1.keys()   # keys only in d2
d1.keys() & d2.keys()   # keys in both
```

---

## `not in`

It checks absence from a collection: O(1) on a set or dict, O(n) on a list.

```python
seen = set()
if num not in seen:
    seen.add(num)
```

**Interview optimization:** if you check membership repeatedly in a loop, convert the list to a set first to get O(1) lookups instead of O(n).

---

## `enumerate()`

It gives `(index, value)` pairs while looping, without a manual counter.

```python
for i, val in enumerate(arr):
    print(i, val)

for i, val in enumerate(arr, start=1):   # start counting from 1 instead of 0
    print(i, val)
```

Under the hood it is a lazy iterator producing tuples:

```python
list(enumerate(["a", "b"]))   # [(0, 'a'), (1, 'b')]
```

Use it only when the **index itself matters** to the logic (Two Sum needs to return positions). Otherwise a plain `for val in arr` is simpler.

---

## Ternary (conditional expression)

```python
value = <expr_if_true> if <condition> else <expr_if_false>
```

Good for a function whose whole job is "pick A or B and return it":

```python
def max_of_two(a, b):
    return a if a > b else b
```

**Caveat I learned today:** don't wrap an already-boolean expression in a ternary.

```python
return True if s_count == t_count else False   # redundant
return s_count == t_count                       # correct, same result
```

Also don't use a ternary to force an early `return` inside a loop that needs to keep scanning (like Two Sum). It breaks the loop logic. A ternary fits single-condition, single-branch returns, not multi-step loop control.

---

## Problems Solved Today

### 1. Two Sum

```python
def two_sum(nums, target):
    seen = {}
    for i, n in enumerate(nums):
        complement = target - n
        if complement in seen:
            return [seen[complement], i]
        seen[n] = i
```

**Pattern:** complement lookup via a hash map.
**Time:** O(n). **Space:** O(n).

### 2. Valid Anagram

```python
from collections import Counter

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        return Counter(s) == Counter(t)
```

**Pattern:** frequency map comparison.
**Time:** O(n). **Space:** O(1), bounded by the alphabet size.

My first-pass version used manual dicts, which is what you need if an interview disallows `Counter`:

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        s_count, t_count = {}, {}
        for ch in s:
            s_count[ch] = s_count.get(ch, 0) + 1
        for ch in t:
            t_count[ch] = t_count.get(ch, 0) + 1
        return s_count == t_count
```

### 3. Contains Duplicate

Given an integer array `nums`, return `true` if any value appears more than once, otherwise `false`.

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        seen = {}
        for num in nums:
            if num in seen:
                return True
            seen[num] = True

        return False
```

**Pattern:** "have I seen this before?" with early exit.
**Time:** O(n). **Space:** O(n).

The same idea as a one-liner, using the fact that a set drops duplicates:

```python
def has_duplicate(nums):
    return len(nums) != len(set(nums))
```

---

## Key Takeaways

1. **"Have I seen this before?" means a set or dict.** This single question resolves most Arrays & Hashing problems.
2. **`.get(key, default)` removes most if/else branching** when building frequency maps.
3. **`Counter` and dict `==` do in one line what a manual nested loop does in ten.** Know the manual version for interviews, use the built-in for real code.
4. **`enumerate()`** only when the index is actually needed in the logic.
5. **Ternary** is for single-branch value selection, not loop control or re-wrapping booleans.
6. **Convert a list to a set before repeated membership checks.** It's a common, expected optimization, so say it out loud in interviews even if the problem doesn't strictly require it.

---

Next up: **Sliding Window + Two Pointers**. Stay tuned! 🚀

> "Success is the sum of small efforts, repeated day in and day out." - Robert Collier
