---
layout: post
title:  "Data Structure Reversal Array - Python"
date:   2025-04-28
desc: "I need to excercise my brain with python daily with DSA problem solving challenge and today is with Array reversal problem. Its quite easy. Good start with Hackerrank"
---
# Reverse an Array - My First Data Structure Problem on HackerRank!

---

## Problem Statement

The task was simple yet fundamental:
> **Given an array of integers, reverse the order of the elements and return the reversed array.**

This was my first data structure problem solved on HackerRank, and it helped me understand array manipulation basics.

---

## My Python Solution

Here is the code I wrote to solve it:

```python
#!/bin/python3

import math
import os
import random
import re
import sys

# Complete the 'reverseArray' function below.
# The function is expected to return an INTEGER_ARRAY.
# The function accepts INTEGER_ARRAY a as parameter.

def reverseArray(a):
    # Create an empty list to store the reversed elements
    a_reverse = []
    # Iterate from the last index to the first
    for i in range(len(a) - 1, -1, -1):
        a_reverse.append(a[i])
    return a_reverse

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    arr_count = int(input().strip())
    arr = list(map(int, input().rstrip().split()))

    res = reverseArray(arr)

    fptr.write(' '.join(map(str, res)))
    fptr.write('\n')

    fptr.close()
```

---

## Step-by-Step Explanation

### 1. Input Handling
- `arr_count` reads the number of elements.
- `arr` reads the array as a list of integers, split by spaces.

### 2. Logic to Reverse the Array
- I created an empty list `a_reverse`.
- I used a **for loop** to iterate from the **last index** to the **first**.
- At each iteration, I appended the element to `a_reverse`.
- After the loop, `a_reverse` contained the elements in reversed order.

### 3. Output Handling
- The reversed array was joined into a space-separated string and written to a file using `fptr.write()`.


---

## Key Python Concepts I Learned

- **List traversal in reverse** using `range(start, stop, step)` with a step of `-1`.
- **Handling standard input and output** with `input()` and environment-based file writing (`os.environ['OUTPUT_PATH']`).
- **Using `map()`** to quickly convert list elements to integers.
- **Joining list elements** into a single string for output.

---

## Reflection

This problem gave me my first taste of how important array manipulation is in solving real-world problems. Even though it was simple, it laid the foundation for understanding more complex data structure operations like:

- Reversing linked lists
- Using stacks for reversal problems
- Understanding indexing and iteration patterns

I'm excited to solve more such problems and build a solid understanding of Data Structures and Algorithms (DSA)!

---

## Final Output Example

Input:
```
5
1 2 3 4 5
```

Output:
```
5 4 3 2 1
```

---

Stay tuned as I continue this DSA learning journey! 🚀

> "Success is the sum of small efforts, repeated day in and day out." - Robert Collier