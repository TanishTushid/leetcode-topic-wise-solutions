# Selection Sort

## Definition

Selection Sort is a **comparison-based sorting algorithm** that repeatedly finds the **smallest element from the unsorted portion** of the array and places it at the beginning of that portion.

The main idea is:

```text
Find Minimum → Swap → Repeat
```

---

# Core Idea

Selection Sort divides the array conceptually into two portions:

```text
Sorted Portion | Unsorted Portion
```

Initially:

```text
[] | [5, 3, 8, 1, 2]
```

Find the minimum element from the unsorted portion:

```text
Minimum = 1
```

Swap it with the first unsorted element:

```text
[1] | [3, 8, 5, 2]
```

Repeat:

```text
[1, 2] | [8, 5, 3]
```

Then:

```text
[1, 2, 3] | [5, 8]
```

Finally:

```text
[1, 2, 3, 5, 8]
```

---

# Working Structure

Consider:

```text
[5, 3, 8, 1, 2]
```

### Pass 1

Unsorted portion:

```text
[5, 3, 8, 1, 2]
```

Find minimum:

```text
1
```

Swap `1` with `5`:

```text
[1, 3, 8, 5, 2]
```

Now `1` is in its correct position.

---

### Pass 2

Unsorted portion:

```text
[3, 8, 5, 2]
```

Find minimum:

```text
2
```

Swap:

```text
[1, 2, 8, 5, 3]
```

---

### Pass 3

Unsorted portion:

```text
[8, 5, 3]
```

Find minimum:

```text
3
```

Swap:

```text
[1, 2, 3, 5, 8]
```

---

# Step 1 — Assume Minimum

At the beginning of every pass, assume the first unsorted element is the minimum.

```python
min_index = i
```

Example:

```text
[5, 3, 8, 1, 2]
 ↑
min
```

Initially:

```text
min_index = 0
```

---

# Step 2 — Find Minimum

Compare the current minimum with the remaining unsorted elements.

```python
for j in range(i + 1, n):

    if arr[j] < arr[min_index]:
        min_index = j
```

If a smaller element is found, update `min_index`.

---

# Step 3 — Swap

After finding the minimum element:

```python
arr[i], arr[min_index] = arr[min_index], arr[i]
```

This places the minimum element at its correct position.

---

# Selection Sort Code

```python
def selection_sort(arr):

    n = len(arr)

    for i in range(n - 1):

        # assume current element is minimum
        min_index = i

        # find minimum in unsorted portion
        for j in range(i + 1, n):

            if arr[j] < arr[min_index]:
                min_index = j

        # place minimum at correct position
        arr[i], arr[min_index] = arr[min_index], arr[i]

    return arr


arr = [5, 3, 8, 1, 2]

print(selection_sort(arr))
```

### Output

```text
[1, 2, 3, 5, 8]
```

---

# Dry Run

Consider:

```text
arr = [5, 3, 8, 1, 2]
```

---

## Pass 1

```text
i = 0
min_index = 0
```

Current:

```text
[5, 3, 8, 1, 2]
 ↑
 min
```

Compare:

```text
3 < 5
```

Update:

```text
min_index = 1
```

Compare:

```text
8 < 3
```

False.

Compare:

```text
1 < 3
```

True.

Update:

```text
min_index = 3
```

Compare:

```text
2 < 1
```

False.

Minimum:

```text
1
```

Swap:

```text
[1, 3, 8, 5, 2]
```

---

## Pass 2

```text
i = 1
```

Unsorted portion:

```text
[3, 8, 5, 2]
```

Assume:

```text
min = 3
```

Compare:

```text
8 < 3 → False
5 < 3 → False
2 < 3 → True
```

Minimum:

```text
2
```

Swap:

```text
[1, 2, 8, 5, 3]
```

---

## Pass 3

Unsorted portion:

```text
[8, 5, 3]
```

Minimum:

```text
3
```

Swap:

```text
[1, 2, 3, 5, 8]
```

---

## Pass 4

Unsorted portion:

```text
[5, 8]
```

Minimum:

```text
5
```

No meaningful change.

Final:

```text
[1, 2, 3, 5, 8]
```

---

# Important Variables

## `i`

```python
for i in range(n - 1):
```

`i` represents the **first position of the unsorted portion**.

Everything before `i` is already sorted.

---

## `min_index`

```python
min_index = i
```

Stores the index of the smallest element found so far.

---

## `j`

```python
for j in range(i + 1, n):
```

`j` scans the remaining unsorted elements to find the minimum.

---

# Visual Representation

```text
Sorted Portion | Unsorted Portion

[1] | [3, 8, 5, 2]
 ↓
Find minimum
 ↓
[1, 2] | [8, 5, 3]
 ↓
Find minimum
 ↓
[1, 2, 3] | [5, 8]
 ↓
Find minimum
 ↓
[1, 2, 3, 5, 8]
```

---

# Time Complexity

## Best Case

```text
O(n²)
```

Even if the array is already sorted, Selection Sort still scans the unsorted portion to find the minimum.

Example:

```text
[1, 2, 3, 4, 5]
```

It still performs the comparisons.

---

## Average Case

```text
O(n²)
```

---

## Worst Case

```text
O(n²)
```

The algorithm performs approximately:

```text
n + (n-1) + (n-2) + ... + 1
```

comparisons.

Therefore:

```text
O(n²)
```

---

# Space Complexity

```text
O(1)
```

Selection Sort is an **in-place sorting algorithm**.

It only uses a few extra variables such as:

```text
min_index
j
i
```

No additional array is required.

---

# Properties

| Property     | Selection Sort |
| ------------ | -------------- |
| Best Case    | O(n²)          |
| Average Case | O(n²)          |
| Worst Case   | O(n²)          |
| Space        | O(1)           |
| Stable       | No*            |
| In-place     | Yes            |
| Adaptive     | No             |

`*` Standard Selection Sort is not stable.

---

# Why Is Selection Sort Not Stable?

Consider elements with equal values:

```text
[4A, 2, 4B, 1]
```

After selecting `1` and swapping:

```text
[1, 2, 4B, 4A]
```

The relative order of `4A` and `4B` changed.

Therefore, standard Selection Sort is **not stable**.

---

# Advantages

* Simple to understand
* Easy to implement
* Requires O(1) extra space
* Performs a small number of swaps
* Useful when memory is limited
* Good for learning sorting fundamentals

---

# Disadvantages

* O(n²) time in all cases
* Not efficient for large datasets
* Not adaptive
* Standard implementation is not stable
* Performs many comparisons even when the array is already sorted

---

# Selection Sort vs Bubble Sort

| Feature        | Selection Sort      | Bubble Sort             |
| -------------- | ------------------- | ----------------------- |
| Best Case      | O(n²)               | O(n)*                   |
| Average Case   | O(n²)               | O(n²)                   |
| Worst Case     | O(n²)               | O(n²)                   |
| Space          | O(1)                | O(1)                    |
| Stable         | No                  | Yes                     |
| In-place       | Yes                 | Yes                     |
| Main Operation | Find minimum + Swap | Compare adjacent + Swap |

`*` Optimized Bubble Sort.

---

# Selection Sort vs Insertion Sort

| Feature        | Selection Sort | Insertion Sort |
| -------------- | -------------- | -------------- |
| Best Case      | O(n²)          | O(n)           |
| Average Case   | O(n²)          | O(n²)          |
| Worst Case     | O(n²)          | O(n²)          |
| Space          | O(1)           | O(1)           |
| Stable         | No             | Yes            |
| In-place       | Yes            | Yes            |
| Main Operation | Find minimum   | Shift + Insert |

---

# When to Use Selection Sort?

Selection Sort can be useful when:

```text
Small dataset
      ↓
Simple implementation needed
      ↓
Memory is limited
      ↓
Number of swaps should be minimized
```

It is mainly useful for learning and for situations where minimizing writes/swaps is more important than minimizing comparisons.

---

# Mental Model

Remember:

```text
Unsorted Portion
       ↓
Find Minimum
       ↓
Swap with First Unsorted Element
       ↓
Sorted Portion Grows
       ↓
Repeat
```

---

# Key Takeaway

Selection Sort always follows:

```text
Find Minimum
↓
Swap
↓
Move Boundary
↓
Repeat
```

The most important line is:

```python
if arr[j] < arr[min_index]:
    min_index = j
```

This finds the smallest element in the unsorted portion.

Then:

```python
arr[i], arr[min_index] = arr[min_index], arr[i]
```

places that minimum element at its correct position.

---

# Quick Revision

```text
Algorithm: Selection Sort

Best Case     = O(n²)
Average Case  = O(n²)
Worst Case    = O(n²)
Space         = O(1)

Stable        = No
In-place      = Yes
Adaptive      = No

Pattern:
Find Minimum → Swap → Repeat
```

## One-Line Memory Trick

```text
Selection Sort = Select Minimum → Put at Correct Position
```
