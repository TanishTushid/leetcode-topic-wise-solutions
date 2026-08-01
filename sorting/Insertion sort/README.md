# Insertion Sort

## Definition

Insertion Sort is a **simple comparison-based sorting algorithm** that builds the sorted array one element at a time.

It works by taking each element and inserting it into its correct position in the already sorted portion of the array.

---

# Core Idea

```text
Pick → Compare → Shift → Insert
```

---

# Working Structure

Insertion Sort divides the array conceptually into two portions:

```text
Sorted Portion | Unsorted Portion
```

Initially, the first element is considered sorted.

Example:

```text
[5, 3, 4, 1, 2]

[5] | [3, 4, 1, 2]
```

Take `3` and insert it into the correct position:

```text
[3, 5] | [4, 1, 2]
```

Take `4`:

```text
[3, 4, 5] | [1, 2]
```

Continue until the complete array is sorted.

```text
[1, 2, 3, 4, 5]
```

---

# Step 1 — Pick an Element

Start from the second element because the first element is already considered sorted.

```python
for i in range(1, len(arr)):
    key = arr[i]
```

Here:

```text
key = current element
```

---

# Step 2 — Compare with Sorted Portion

Compare the `key` with elements before it.

```python
j = i - 1

while j >= 0 and arr[j] > key:
```

If the previous element is greater than `key`, move it one position to the right.

---

# Step 3 — Shift Elements

```python
arr[j + 1] = arr[j]
j -= 1
```

Example:

```text
[3, 5, 7, 2]

key = 2

7 > 2 → shift 7
5 > 2 → shift 5
3 > 2 → shift 3
```

Array temporarily becomes:

```text
[3, 3, 5, 7]
```

---

# Step 4 — Insert Key

After finding the correct position:

```python
arr[j + 1] = key
```

Result:

```text
[2, 3, 5, 7]
```

---

# Insertion Sort Code

```python
def insertion_sort(arr):

    for i in range(1, len(arr)):

        # current element
        key = arr[i]

        # last element of sorted portion
        j = i - 1

        # shift elements greater than key
        while j >= 0 and arr[j] > key:

            arr[j + 1] = arr[j]
            j -= 1

        # insert key at correct position
        arr[j + 1] = key

    return arr


arr = [5, 3, 4, 1, 2]

print(insertion_sort(arr))
```

### Output

```text
[1, 2, 3, 4, 5]
```

---

# Dry Run

Consider:

```text
arr = [5, 3, 4, 1, 2]
```

### Iteration 1

```text
key = 3
```

Compare:

```text
5 > 3
```

Shift `5`:

```text
[5, 5, 4, 1, 2]
```

Insert `3`:

```text
[3, 5, 4, 1, 2]
```

---

### Iteration 2

```text
key = 4
```

Compare:

```text
5 > 4
```

Shift `5`:

```text
[3, 5, 5, 1, 2]
```

Insert `4`:

```text
[3, 4, 5, 1, 2]
```

---

### Iteration 3

```text
key = 1
```

Shift:

```text
5 → 4 → 3
```

Result:

```text
[1, 3, 4, 5, 2]
```

---

### Iteration 4

```text
key = 2
```

Shift:

```text
5 → 4 → 3
```

Final:

```text
[1, 2, 3, 4, 5]
```

---

# Important Variables

```python
key = arr[i]
```

`key` stores the element that we want to insert.

```python
j = i - 1
```

`j` points to the last element of the sorted portion.

```python
arr[j + 1] = arr[j]
```

Shifts a larger element one position to the right.

```python
arr[j + 1] = key
```

Places the key in its correct position.

---

# Time Complexity

## Best Case

```text
O(n)
```

The array is already sorted.

Example:

```text
[1, 2, 3, 4, 5]
```

Only one comparison is needed for each element.

---

## Average Case

```text
O(n²)
```

Elements generally need to be shifted.

---

## Worst Case

```text
O(n²)
```

The array is sorted in reverse order.

Example:

```text
[5, 4, 3, 2, 1]
```

Every element needs to be compared and shifted.

---

# Space Complexity

```text
O(1)
```

Insertion Sort is an **in-place sorting algorithm**.

It does not require an additional array.

---

# Properties

| Property     | Insertion Sort |
| ------------ | -------------- |
| Best Case    | O(n)           |
| Average Case | O(n²)          |
| Worst Case   | O(n²)          |
| Space        | O(1)           |
| Stable       | Yes            |
| In-place     | Yes            |
| Adaptive     | Yes            |

---

# Advantages

* Simple and easy to understand
* Easy to implement
* Requires O(1) extra space
* Stable sorting algorithm
* Efficient for small datasets
* Efficient when the array is nearly sorted
* Adaptive algorithm

---

# Disadvantages

* O(n²) average and worst-case time
* Not efficient for large unsorted datasets
* Slower than Merge Sort, Quick Sort, and Heap Sort for large inputs

---

# When to Use Insertion Sort?

Insertion Sort is useful when:

```text
Small dataset
        ↓
Nearly sorted array
        ↓
Need simple implementation
        ↓
Memory is limited
```

It is also commonly useful when elements are being added one at a time and the collection is kept sorted.

---

# Mental Model

Remember this:

```text
Sorted Portion | Current Element
       ↓
      Pick
       ↓
 Compare with previous elements
       ↓
 Shift larger elements
       ↓
 Insert key
       ↓
 Sorted Portion grows
```

---

# Key Takeaway

Insertion Sort always follows:

```text
Pick
↓
Compare
↓
Shift
↓
Insert
```

The most important concept is:

```python
key = arr[i]
```

The left side of the array is already sorted.

We compare `key` with the elements on the left and shift every element that is greater than `key`.

Finally:

```python
arr[j + 1] = key
```

places the element at its correct position.

---

# Quick Revision

```text
Algorithm: Insertion Sort

Best Case     = O(n)
Average Case  = O(n²)
Worst Case    = O(n²)
Space         = O(1)

Stable        = Yes
In-place      = Yes
Adaptive      = Yes

Pattern:
Pick → Compare → Shift → Insert
```
