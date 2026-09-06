# 📘 Design & Analysis of Algorithms — Complete Sessional Handbook

> A single, self-contained reference covering definitions, algorithms, dry-runs, complexity analysis, recurrence relations, and the highest-yield exam questions — polished and expanded for revision.

---

## 🗺️ Table of Contents

1. [Algorithm — Definition & Characteristics](#part-1)
2. [Complexity of an Algorithm](#part-2)
3. [Best, Average, and Worst Case](#part-3)
4. [Asymptotic Notations (O, Ω, Θ)](#part-4)
5. [Common Complexity Classes](#part-5)
6. [In-place vs Out-of-place Sorting](#part-6)
7. [Stable vs Unstable Sorting](#part-7)
8. [Comparison vs Non-Comparison Sorting](#part-8)
9. [Insertion Sort](#part-9)
10. [Shell Sort](#part-10)
11. [Selection Sort](#part-11)
12. [Quick Sort](#part-12)
13. [Merge Sort](#part-13)
14. [Counting Sort](#part-14)
15. [Radix Sort](#part-15)
16. [Master Comparison Table](#part-16)
17. [Quick vs Merge vs Radix](#part-17)
18. [Insertion vs Shell Sort](#part-18)
19. [Recurrence Relations](#part-19)
20. [Methods to Solve Recurrences](#part-20)
21. [Substitution Method](#part-21)
22. [Recursion Tree Method (Bonus)](#part-21b)
23. [Master Theorem](#part-22)
24. [Merge Sort via Master Theorem](#part-23)
25. [Quick Sort via Recurrence](#part-24)
26. [Radix Sort — Full Worked Example](#part-25)
27. [Core Complexity Concepts](#part-26)
28. [Analyzing Loop Complexity](#part-27)
29. [Key Definitions Glossary](#part-28)
30. [Important Exam Questions (Ranked)](#part-29)
31. [One-Page Revision Table](#part-30)
32. [Memorize vs Understand](#part-31)
33. [Final Mental Map & Study Order](#part-32)

---

<a name="part-1"></a>
# PART 1 — ALGORITHM

## 1.1 What Is an Algorithm?

An **algorithm** is a finite sequence of well-defined, unambiguous instructions used to solve a specific problem or accomplish a specific task, given some input and producing some output.

> **One-line definition:** *An algorithm is a step-by-step procedure for solving a problem in a finite amount of time.*

### Worked Example

**Problem:** Find the largest number among `10, 25, 15`.

**Algorithm:**
```
1. Start
2. Assume 10 is the largest (call it `max`)
3. Compare 25 with max
4. Since 25 > 10, set max = 25
5. Compare 15 with max
6. Since 15 < 25, keep max = 25
7. Output max (= 25)
8. Stop
```

This tiny example already demonstrates every property an algorithm must have — it has a clear input, a clear output, unambiguous steps, it terminates, and every step is something a computer can actually execute.

## 1.2 Characteristics of an Algorithm

A well-formed algorithm must satisfy **five core properties**. Examiners frequently ask you to *name and explain* these — memorize them as a set.

| # | Property | Meaning | Example |
|---|----------|---------|---------|
| 1 | **Input** | Accepts zero or more well-defined inputs | Two numbers `A`, `B` |
| 2 | **Output** | Produces at least one well-defined output | `A + B` |
| 3 | **Definiteness** | Every step is precise and unambiguous — no room for interpretation | "Add 5 to the number" (good) vs. "Add a suitable number" (bad) |
| 4 | **Finiteness** | Must terminate after a finite number of steps — no infinite loops | A `for` loop bounded by `n` |
| 5 | **Effectiveness** | Every operation must be basic enough to be carried out exactly, in finite time, using existing resources | Basic arithmetic, comparisons, assignments |

### ⭐ Model Exam Answer

> "An algorithm is a finite sequence of well-defined instructions used to solve a particular problem. Its five essential characteristics are input, output, definiteness, finiteness, and effectiveness. Input and output ensure the algorithm interacts meaningfully with data; definiteness ensures no ambiguity in execution; finiteness guarantees termination; and effectiveness ensures every step is practically executable."

**Common pitfall:** Students often forget *effectiveness* and *definiteness* are two different things — definiteness is about clarity of the *instruction*, while effectiveness is about whether the *operation itself* is simple/basic enough to execute directly.

---

<a name="part-2"></a>
# PART 2 — COMPLEXITY OF AN ALGORITHM

**Complexity** measures how the resource requirements (time or memory) of an algorithm scale as the input size `n` grows. It answers: *"How does this algorithm behave as data grows larger?"*

There are two primary types:

### 2.1 Time Complexity
The number of elementary operations (or the running time) as a function of input size `n`. This is the most frequently examined metric.

### 2.2 Space Complexity
The total memory used by the algorithm, including:
- **Auxiliary space** — extra space used besides the input itself (this is usually what's meant when people casually say "space complexity")
- **Input space** — space used to store the input itself

### Worked Example

```java
for (int i = 0; i < n; i++) {
    System.out.println(i);
}
```

The loop body executes exactly `n` times, and each iteration takes constant time.

**Time Complexity = O(n)**
**Space Complexity = O(1)** (no extra memory grows with `n`)

> **Tip:** When answering "find the complexity" questions, always state *both* time and space complexity, even if only one is asked — examiners award bonus clarity marks for it.

---

<a name="part-3"></a>
# PART 3 — BEST, AVERAGE, AND WORST CASE

The same algorithm can take different amounts of time depending on the *arrangement* of the input, not just its size. This is why we describe algorithms using three distinct cases.

| Case | Meaning |
|------|---------|
| **Best Case** | The minimum time taken, over all possible inputs of size `n` — the most favorable arrangement |
| **Average Case** | The expected time, averaged over all possible inputs (assuming some probability distribution, often uniform) |
| **Worst Case** | The maximum time taken over all possible inputs — the most unfavorable arrangement, and the most commonly quoted guarantee |

### Worked Example — Linear Search

Array: `[10, 20, 30, 40, 50]`

- **Search for `10`** (first element): found immediately → **Best case = O(1)**
- **Search for `50`** (last element, or absent): must scan the entire array → **Worst case = O(n)**
- **Search for a random element:** on average, about `n/2` comparisons → constants are dropped → **Average case = O(n)**

> **Exam insight:** Worst-case analysis is preferred in practice because it gives a *guarantee* — a promise that the algorithm will never take longer than this bound, regardless of input.

---

<a name="part-4"></a>
# PART 4 — ASYMPTOTIC NOTATIONS

Asymptotic notation is a mathematical language used to describe the **growth rate** of an algorithm's running time as `n → ∞`, while ignoring machine-dependent constants and lower-order terms.

The three fundamental notations:

## 4.1 Big-O Notation (O) — Upper Bound

Big-O gives an **upper bound** on the growth rate — the algorithm will *never* do worse than this. It is most commonly used to describe the **worst case**.

**Formal definition:** `f(n) = O(g(n))` if there exist positive constants `c` and `n₀` such that `0 ≤ f(n) ≤ c·g(n)` for all `n ≥ n₀`.

**Worked Example:**
```
T(n) = 3n² + 5n + 10
```
As `n` grows large, `n²` dominates the other terms — constants and lower-order terms become insignificant.
```
T(n) = O(n²)
```

## 4.2 Omega Notation (Ω) — Lower Bound

Omega gives a **lower bound** — the algorithm will *never* do better than this. It is most commonly used to describe the **best case**.

**Formal definition:** `f(n) = Ω(g(n))` if there exist positive constants `c` and `n₀` such that `0 ≤ c·g(n) ≤ f(n)` for all `n ≥ n₀`.

**Worked Example:**
```
T(n) = 3n² + 5n + 10  ⟹  T(n) = Ω(n²)
```

## 4.3 Theta Notation (Θ) — Tight Bound

Theta gives a **tight bound** — the function is *sandwiched* between a constant multiple of `g(n)` from above and below. This means the algorithm's growth rate is *exactly* `g(n)`, asymptotically.

**Formal definition:** `f(n) = Θ(g(n))` if `f(n) = O(g(n))` **and** `f(n) = Ω(g(n))` simultaneously.

**Worked Example:**
```
T(n) = 3n² + 5n + 10  ⟹  T(n) = Θ(n²)
```

## ⭐ Quick Reference Table

| Notation | Bound Type | Represents | Analogy |
|----------|-----------|------------|---------|
| **O (Big-O)** | Upper bound | "At most this much" | Worst case |
| **Ω (Omega)** | Lower bound | "At least this much" | Best case |
| **Θ (Theta)** | Tight bound | "Exactly this much" (up to constants) | Exact growth |

### Memory Trick
> **O → Upper** (ceiling), **Ω → Lower** (floor), **Θ → Exact/Tight** (sandwiched between the two)

**Common exam trap:** Students often say "Big-O is for worst case and Omega is for best case" as if that's a *rule*. This is **not strictly true** — O, Ω, and Θ are just mathematical bounds and can technically be applied to best, worst, or average case running times. The *association* with worst/best case is a common convention, not a mathematical law. Mention this nuance for bonus marks.

---

<a name="part-5"></a>
# PART 5 — COMMON COMPLEXITY CLASSES

Ranked from fastest-growing (best) to slowest-growing (worst), for large `n`:

```
O(1)  <  O(log n)  <  O(n)  <  O(n log n)  <  O(n²)  <  O(n³)  <  O(2ⁿ)  <  O(n!)
```

| Complexity | Name | Typical Example |
|------------|------|------------------|
| O(1) | Constant | Array index access |
| O(log n) | Logarithmic | Binary Search |
| O(n) | Linear | Linear Search, single loop |
| O(n log n) | Linearithmic | Merge Sort, Quick Sort (avg) |
| O(n²) | Quadratic | Nested loops, Bubble/Insertion/Selection Sort |
| O(n³) | Cubic | Triple nested loops, naive matrix multiplication |
| O(2ⁿ) | Exponential | Naive recursive Fibonacci, subsets generation |
| O(n!) | Factorial | Brute-force Traveling Salesman, permutations |

> **Visual intuition:** For `n = 1000` — `log n ≈ 10`, `n = 1000`, `n log n ≈ 10,000`, `n² = 1,000,000`. The gap between `n log n` and `n²` widens dramatically as `n` grows — this is *why* Merge/Quick Sort beat Bubble/Insertion Sort at scale.

---

<a name="part-6"></a>
# PART 6 — IN-PLACE VS OUT-OF-PLACE SORTING

This is a **frequent theory question** — know both definitions cold, plus examples.

## In-Place Sorting
An algorithm that sorts the data using only a **small, constant amount of extra memory** (typically O(1), sometimes O(log n) for recursion stacks), rather than allocating a new structure proportional to input size.

**Examples:** Insertion Sort, Selection Sort, Quick Sort (array-based), Shell Sort, Bubble Sort

> **Nuance:** Quick Sort is usually called in-place with respect to the *array itself* (no separate output array), but its recursive calls consume **O(log n)** stack space on average and **O(n)** in the worst case — so it isn't *strictly* O(1) extra space.

## Out-of-Place Sorting
An algorithm that requires **additional memory proportional to the input size** (typically O(n)) to perform the sort — usually because it builds a separate output structure.

**Example:** Merge Sort — requires O(n) additional memory for the temporary arrays used during merging.

## Comparison Table

| Feature | In-Place | Out-of-Place |
|---------|----------|---------------|
| Extra memory | Small (usually O(1)) | Significant (usually O(n)) |
| Memory efficient | ✅ Yes | ❌ No |
| Example | Insertion Sort, Quick Sort | Merge Sort |
| Typical extra space | O(1) | O(n) |
| Good for memory-constrained systems | ✅ Yes | ❌ No |

---

<a name="part-7"></a>
# PART 7 — STABLE VS UNSTABLE SORTING

## Stable Sorting
A sort is **stable** if two elements with equal keys retain their **original relative order** in the output.

**Worked Example:**

Consider records with (Name, Score): `A(90), B(80), C(90)`

After sorting by score ascending:
```
B(80), A(90), C(90)
```
`A` still appears **before** `C` (their original relative order among equal-score elements is preserved) → **Stable**.

## Unstable Sorting
If equal-key elements *may* swap their relative order during sorting, the algorithm is **unstable**. This matters in real applications — e.g., sorting a list of students first by name and then (stably) by grade should preserve the name-order *within* each grade group. An unstable sort could scramble that secondary ordering.

## Stability Reference Table

| Algorithm | Stable? | Notes |
|-----------|---------|-------|
| Bubble Sort | ✅ Yes | Only swaps adjacent unequal elements |
| Insertion Sort | ✅ Yes | Shifts, never swaps equal keys past each other |
| Merge Sort | ✅ Yes | As long as merge step takes left-side elements first on ties |
| Counting Sort | ✅ Yes* | Stable *only* if implemented by iterating output construction backward |
| Radix Sort | ✅ Yes* | Stable *only if* the underlying digit-sort (usually Counting Sort) is stable |
| Selection Sort | ❌ Usually No | Swapping the minimum into place can jump it past equal elements |
| Quick Sort | ❌ Usually No | Partitioning swaps elements non-locally |
| Shell Sort | ❌ No | Gapped comparisons swap elements across long distances, disrupting order |

`*` = Stability is implementation-dependent, not an inherent guarantee of the algorithm's core idea.

---

<a name="part-8"></a>
# PART 8 — COMPARISON VS NON-COMPARISON SORTING

## Comparison-Based Sorting
Determines the relative order of elements strictly by comparing them pairwise, using operations like `<`, `>`, `≤`, `≥`.

**Examples:** Bubble, Selection, Insertion, Shell, Merge, Quick Sort

**Theoretical limit:** Any comparison-based sort requires **Ω(n log n)** comparisons in the worst case — this is provable using a decision-tree argument (there are `n!` possible orderings, and each comparison gives at most 1 bit of information, so you need at least `log₂(n!) ≈ n log n` comparisons).

## Non-Comparison Sorting
Sorts elements by exploiting structural properties of the *keys themselves* — such as their digits, range, or frequency — rather than comparing elements to one another.

**Examples:** Counting Sort, Radix Sort, Bucket Sort

**Why they can beat O(n log n):** Because they don't rely on comparisons, they aren't bound by the Ω(n log n) comparison lower bound — they can achieve linear time O(n) or O(n+k) under the right conditions (bounded key range, limited digits, etc.).

---

<a name="part-9"></a>
# PART 9 — INSERTION SORT

**Analogy:** Sorting playing cards in your hand — you pick up one card at a time and insert it into its correct position among the cards you're already holding.

## 9.1 Idea

Maintain two conceptual regions within the array:
```
[ Sorted Region | Unsorted Region ]
```
Repeatedly take the first element of the unsorted region (the "key") and insert it into its correct position within the sorted region, shifting larger elements rightward to make room.

## 9.2 Worked Example

Array: `5  3  4  1  2`

| Step | Action | Array State |
|------|--------|-------------|
| Start | `5` is trivially sorted | `[5] \| 3 4 1 2` |
| 1 | Take `3`; compare with `5` (5>3, shift); insert `3` | `[3 5] \| 4 1 2` |
| 2 | Take `4`; compare with `5` (shift), then `3` (stop); insert `4` | `[3 4 5] \| 1 2` |
| 3 | Take `1`; shift `5,4,3` right; insert `1` at front | `[1 3 4 5] \| 2` |
| 4 | Take `2`; shift `5,4,3` right, stop at `1`; insert `2` | `[1 2 3 4 5]` |

## 9.3 Algorithm (Pseudocode)

```text
InsertionSort(A, n)
    for i = 1 to n-1
        key = A[i]
        j = i - 1
        while j >= 0 and A[j] > key
            A[j+1] = A[j]     // shift larger element right
            j = j - 1
        A[j+1] = key          // insert key into its correct slot
```

## 9.4 Complexity Analysis

| Case | Complexity | Reasoning |
|------|-----------|-----------|
| **Best** | O(n) | Array already sorted — inner `while` never executes, only the outer scan runs |
| **Average** | O(n²) | On average, about half the sorted portion is shifted per element |
| **Worst** | O(n²) | Array in reverse order — every new key must shift past *all* previously sorted elements |

- **Space:** O(1) — sorts in place using no extra array
- **Stable:** ✅ Yes
- **In-place:** ✅ Yes
- **Best used when:** the array is nearly sorted, or the dataset is small (Insertion Sort has very low overhead and beats O(n log n) sorts for tiny `n`)

---

<a name="part-10"></a>
# PART 10 — SHELL SORT

Shell Sort is essentially a **generalization/improvement of Insertion Sort** that allows elements to move in bigger jumps rather than one position at a time, reducing the total number of shifts needed.

## 10.1 Idea

Instead of comparing only adjacent elements, Shell Sort first compares elements that are far apart (separated by a **gap**), gradually shrinking the gap until it becomes `1` — at which point it behaves exactly like ordinary Insertion Sort, but on an already "nearly sorted" array (which Insertion Sort handles very efficiently).

## 10.2 Worked Example

Array: `8  5  3  7  6  2  1  4`   (n = 8)

Initial gap: `gap = n/2 = 4`

**Pass with gap = 4:** compare/sort elements 4 apart: (index 0,4), (1,5), (2,6), (3,7)
```
Compare A[0]=8 & A[4]=6 → swap → 6 5 3 7 8 2 1 4
Compare A[1]=5 & A[5]=2 → swap → 6 2 3 7 8 5 1 4
Compare A[2]=3 & A[6]=1 → swap → 6 2 1 7 8 5 3 4
Compare A[3]=7 & A[7]=4 → swap → 6 2 1 4 8 5 3 7
```

**Reduce gap → 2:** compare/sort elements 2 apart, using insertion-style shifting within each gapped subsequence, eventually yielding a more ordered array.

**Reduce gap → 1:** standard Insertion Sort finishes the job on the now nearly-sorted array.

## 10.3 Algorithm (Pseudocode)

```text
ShellSort(A, n)
    gap = n / 2
    while gap > 0
        for i = gap to n-1
            temp = A[i]
            j = i
            while j >= gap and A[j-gap] > temp
                A[j] = A[j-gap]
                j = j - gap
            A[j] = temp
        gap = gap / 2
```

## 10.4 Complexity Analysis

Shell Sort's complexity depends heavily on the **gap sequence** chosen. With the simple halving sequence `n/2, n/4, ..., 1`:

| Case | Complexity |
|------|-----------|
| Best | ~O(n log n) (some analyses) |
| Average | Depends on gap sequence (commonly cited around O(n^1.25) for good sequences) |
| Worst | O(n²) with the simple halving sequence |

> **Exam-safe statement:** *"Shell Sort's time complexity depends on the chosen gap sequence. With the basic `n/2` halving sequence, the worst-case complexity is O(n²); better gap sequences (like Hibbard's or Sedgewick's) can push the worst case down to roughly O(n^1.5) or better."*

- **Space:** O(1)
- **Stable:** ❌ No (gapped swaps can reorder equal elements)
- **In-place:** ✅ Yes

---

<a name="part-11"></a>
# PART 11 — SELECTION SORT

## 11.1 Idea

Repeatedly **select** the smallest (or largest) remaining element from the unsorted portion of the array and swap it into its correct position at the boundary of the sorted portion.

## 11.2 Worked Example

Array: `64  25  12  22  11`

| Pass | Minimum Found | Array After Swap |
|------|---------------|-------------------|
| 1 | `11` (index 4) | `11 25 12 22 64` |
| 2 | `12` (index 2, within remaining) | `11 12 25 22 64` |
| 3 | `22` (index 3, within remaining) | `11 12 22 25 64` |
| 4 | `25` already in place | `11 12 22 25 64` |

## 11.3 Algorithm (Pseudocode)

```text
SelectionSort(A, n)
    for i = 0 to n-2
        min = i
        for j = i+1 to n-1
            if A[j] < A[min]
                min = j
        swap A[i] and A[min]
```

## 11.4 Complexity Analysis

| Case | Complexity |
|------|-----------|
| Best | O(n²) |
| Average | O(n²) |
| Worst | O(n²) |

Selection Sort **always** performs the same number of comparisons regardless of the initial arrangement (it always scans the entire remaining unsorted region to find the minimum) — this is why all three cases are identical, unlike Insertion Sort.

- **Space:** O(1)
- **Stable:** ❌ Usually No (a distant minimum can jump over equal elements when swapped)
- **In-place:** ✅ Yes
- **Advantage:** Minimizes the *number of swaps* — exactly `n-1` swaps in the worst case, which is useful when write operations are expensive (e.g., writing to flash memory).

---

<a name="part-12"></a>
# PART 12 — QUICK SORT ⭐⭐⭐

One of the **most heavily tested** algorithms in any DAA sessional. It uses the **divide-and-conquer** paradigm.

## 12.1 Basic Idea

1. Choose a **pivot** element.
2. **Partition** the array so all elements smaller than the pivot go to its left, and all larger elements go to its right.
3. Recursively apply Quick Sort to the left and right sub-arrays.
4. Base case: sub-arrays of size 0 or 1 are already sorted.

## 12.2 Worked Example

Array: `8  3  1  7  0  10  2` — suppose pivot = `2` (for illustration)

After partitioning around pivot `2`:
```
1  0  |  2  |  8  3  7  10
```
Recursively sort the left sub-array `[1, 0]` and the right sub-array `[8, 3, 7, 10]`.

Continuing recursively eventually yields:
```
0  1  2  3  7  8  10
```

## 12.3 Quick Sort Algorithm

```text
QuickSort(A, low, high)
    if low < high
        p = Partition(A, low, high)
        QuickSort(A, low, p-1)
        QuickSort(A, p+1, high)
```

## 12.4 Partition Scheme (Lomuto)

```text
Partition(A, low, high)
    pivot = A[high]          // choose last element as pivot
    i = low - 1
    for j = low to high-1
        if A[j] <= pivot
            i = i + 1
            swap A[i], A[j]
    swap A[i+1], A[high]
    return i+1                // final position of pivot
```

> **Note:** This is the *Lomuto partition scheme*. The *Hoare partition scheme* is an alternative that does fewer swaps on average and is the original scheme Tony Hoare proposed — mention it if asked to compare partitioning strategies.

## 12.5 Complexity Analysis

### Best Case — Balanced Partition
The pivot splits the array into two (roughly) equal halves each time:
```
T(n) = 2T(n/2) + O(n)  ⟹  O(n log n)
```

### Average Case
Even with reasonably unbalanced (but not always worst-case) splits, the expected complexity remains:
```
O(n log n)
```

### Worst Case — Maximally Unbalanced Partition
Occurs when the pivot is always the smallest or largest element (e.g., sorting an already-sorted array while always picking the last element as pivot):
```
T(n) = T(n-1) + O(n)  ⟹  O(n²)
```

## 12.6 Space Complexity

| Case | Recursion Stack Depth |
|------|------------------------|
| Average | O(log n) |
| Worst | O(n) |

## 12.7 Advantages & Disadvantages

**Advantages:**
- Very fast in practice (excellent constant factors, good cache locality)
- In-place with respect to the array (no auxiliary array needed)
- Average-case O(n log n)

**Disadvantages:**
- Worst-case O(n²) — mitigated in practice using randomized pivot selection or median-of-three pivot strategies
- Usually unstable
- Performance is sensitive to pivot choice

> **Mitigation technique worth mentioning:** *Randomized Quick Sort* — picking the pivot randomly (or using median-of-three) makes the worst case extremely unlikely for any given input, giving expected O(n log n) performance regardless of the initial arrangement.

---

<a name="part-13"></a>
# PART 13 — MERGE SORT ⭐⭐⭐

Another cornerstone **divide-and-conquer** algorithm, and the most reliable of the comparison sorts because it *guarantees* O(n log n) in every case.

## 13.1 Basic Idea

1. **Divide** the array into two halves.
2. Recursively sort each half.
3. **Merge** the two sorted halves back into a single sorted array.

## 13.2 Worked Example

Array: `38  27  43  3  9  82  10`

**Divide (recursively split until singletons):**
```
38 27 43 3  |  9 82 10
38 27 | 43 3  |  9 82 | 10
38|27 | 43|3  |  9|82 | 10
```

**Merge back up:**
```
27 38  |  3 43  |  9 82  |  10
3 27 38 43  |  9 10 82
3 9 10 27 38 43 82
```

## 13.3 Merge Sort Algorithm

```text
MergeSort(A, left, right)
    if left < right
        mid = (left + right) / 2
        MergeSort(A, left, mid)
        MergeSort(A, mid+1, right)
        Merge(A, left, mid, right)
```

## 13.4 The Merge Step (Worked Example)

Merging two already-sorted sub-arrays:
```
Left:  2  5  8
Right: 1  3  7
```

| Comparison | Winner | Result so far |
|------------|--------|----------------|
| 2 vs 1 | 1 (right) | `1` |
| 2 vs 3 | 2 (left) | `1 2` |
| 5 vs 3 | 3 (right) | `1 2 3` |
| 5 vs 7 | 5 (left) | `1 2 3 5` |
| 8 vs 7 | 7 (right) | `1 2 3 5 7` |
| remaining | 8 (left) | `1 2 3 5 7 8` |

```text
Merge(A, left, mid, right)
    create temp arrays L[] and R[] copying A[left..mid] and A[mid+1..right]
    i = j = 0; k = left
    while i < len(L) and j < len(R)
        if L[i] <= R[j]:  A[k] = L[i]; i++
        else:             A[k] = R[j]; j++
        k++
    copy any remaining elements of L[] and R[] into A[]
```

## 13.5 Complexity Analysis

At each level of the recursion tree, merging all sub-arrays takes **O(n)** total work. There are **log n** levels (since the array halves each time). Therefore:

```
Total work = O(n) × O(log n) = O(n log n)
```

This holds for **all three cases**:

| Case | Complexity |
|------|-----------|
| Best | O(n log n) |
| Average | O(n log n) |
| Worst | O(n log n) |

- **Space:** O(n) — auxiliary arrays needed during merging
- **Stable:** ✅ Yes (when the merge step takes from the left sub-array on ties)
- **In-place:** ❌ No (standard implementation needs O(n) extra space)

> **Why Merge Sort is preferred for linked lists / external sorting:** It doesn't require random access to shift elements (unlike Quick Sort's partitioning), and its predictable O(n log n) guarantee makes it ideal for sorting massive datasets that don't fit in memory (external merge sort).

---

<a name="part-14"></a>
# PART 14 — COUNTING SORT ⭐⭐

A **non-comparison** sorting algorithm that works exceptionally well when the range of key values (`k`) is not significantly larger than the number of elements (`n`).

## 14.1 Worked Example

Array: `4  2  2  8  3  3  1`  → Range of values: `1` to `8`

**Step 1 — Build the count array** (frequency of each value):

| Value | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------|---|---|---|---|---|---|---|---|
| Count | 1 | 2 | 2 | 1 | 0 | 0 | 0 | 1 |

**Step 2 — Reconstruct the sorted array** by repeating each value according to its count:
```
1, 2, 2, 3, 3, 4, 8
```

**Step 3 (for stability) — Prefix-sum + backward placement:** To make Counting Sort **stable**, convert the count array into a *cumulative/prefix-sum* array (each cell tells you the last output position for that value), then iterate the *original* array from **right to left**, placing each element at its computed output position and decrementing the count. This preserves the original relative order of equal elements.

## 14.2 Complexity Analysis

Let `n` = number of elements, `k` = range of values.

| Metric | Complexity |
|--------|-----------|
| Time | O(n + k) |
| Space | O(n + k) |

## 14.3 Advantages & Disadvantages

**Advantages:**
- Can beat the O(n log n) comparison-sort lower bound when `k = O(n)`
- No pairwise comparisons at all
- Naturally stable when implemented correctly

**Disadvantages:**
- Impractical when `k` is very large relative to `n` (e.g., sorting floating-point numbers or a huge key range wastes memory)
- Only works with discrete/integer-like keys (or keys mappable to small integer ranges)

---

<a name="part-15"></a>
# PART 15 — RADIX SORT ⭐⭐⭐

Another **non-comparison** algorithm that sorts numbers **digit by digit**, typically processing digits from the **Least Significant Digit (LSD)** to the **Most Significant Digit (MSD)** — this variant is called **LSD Radix Sort**.

## 15.1 Worked Example (Quick Pass)

Sort: `170, 45, 75, 90, 802, 24, 2, 66`

**Step 1 — Sort by units digit:**
```
170(0) 90(0) 802(2) 2(2) 24(4) 45(5) 75(5) 66(6)
→ 170 90 802 2 24 45 75 66
```

**Step 2 — Sort by tens digit** (stably, preserving order from Step 1):
```
802(0) 2(0) 24(2) 45(4) 66(6) 170(7) 75(7) 90(9)
→ 802 2 24 45 66 170 75 90
```

**Step 3 — Sort by hundreds digit** (stably):
```
2, 24, 45, 66, 75, 90, 170, 802
```

**Sorted!**

## 15.2 Why Must Radix Sort Use a Stable Digit Sort?

This is a **frequently asked conceptual question**.

When sorting by the tens digit (Step 2), elements that share the same tens digit must retain the relative order they had *after* the units-digit pass (Step 1) — otherwise the units-digit sorting work gets destroyed. If the digit-level sort were unstable, correctness would break down: an element correctly placed by a lower-order digit could get shuffled out of order relative to another element with the same higher-order digit. **Counting Sort** is the conventional choice for the digit-sort subroutine because it is easy to implement stably and runs in linear time for a fixed digit range (0–9).

## 15.3 Complexity Analysis

Let `n` = number of elements, `d` = number of digits in the largest number, `k` = the radix/base (10 for decimal).

```
Time = O(d × (n + k))
```

Since `k = 10` is a constant for decimal numbers, this simplifies to:
```
Time ≈ O(d × n)
```

- **Space:** O(n + k)
- **Stable:** ✅ Yes, provided the digit-sort subroutine is stable
- **In-place:** ❌ No

---

<a name="part-16"></a>
# PART 16 — MASTER COMPARISON TABLE ⭐⭐⭐

The single most exam-valuable table in this handbook — memorize it.

| Algorithm | Best | Average | Worst | Space | Stable | In-place |
|-----------|------|---------|-------|-------|--------|----------|
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ Yes | ✅ Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ No* | ✅ Yes |
| Shell Sort | Depends | Depends | O(n²)** | O(1) | ❌ No | ✅ Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) avg | ❌ No | ✅ Yes*** |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ Yes | ❌ No |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(n+k) | ✅ Yes* | ❌ No |
| Radix Sort | O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) | ✅ Yes* | ❌ No |

`*` Stability depends on implementation details.
`**` With the simple `n/2` halving gap sequence; smarter sequences can improve this.
`***` Standard array-based Quick Sort is in-place with respect to the array, but recursion consumes stack space (O(log n) average, O(n) worst).

---

<a name="part-17"></a>
# PART 17 — QUICK SORT VS MERGE SORT VS RADIX SORT

A particularly popular comparative essay question.

| Feature | Quick Sort | Merge Sort | Radix Sort |
|---------|-----------|------------|------------|
| Technique | Divide & conquer | Divide & conquer | Digit-based, non-comparison |
| Comparison-based? | ✅ Yes | ✅ Yes | ❌ No |
| Best case | O(n log n) | O(n log n) | O(d(n+k)) |
| Average case | O(n log n) | O(n log n) | O(d(n+k)) |
| Worst case | O(n²) | O(n log n) | O(d(n+k)) |
| Extra space | O(log n) avg | O(n) | O(n+k) |
| Stable? | Usually No | ✅ Yes | ✅ Yes (with stable digit-sort) |
| In-place? | Usually Yes | ❌ No | ❌ No |
| Best suited for | General-purpose, in-memory sorting, average-case speed | Guaranteed performance, linked lists, external/large-scale sorting | Fixed-format numeric/string keys with bounded digit count |

**Essay-style summary:** *"Quick Sort offers the best average-case practical performance and low memory overhead due to in-place partitioning, but its worst-case degrades to O(n²) without safeguards like randomized pivoting. Merge Sort trades memory (O(n) extra space) for a guaranteed O(n log n) in all cases and natural stability, making it ideal when consistency matters more than raw speed. Radix Sort sidesteps the comparison-based lower bound entirely, achieving near-linear time when digit count `d` is small relative to `n`, but is restricted to keys with a well-defined digit/positional structure."*

---

<a name="part-18"></a>
# PART 18 — INSERTION SORT VS SHELL SORT

Shell Sort is best understood as a direct generalization of Insertion Sort.

| Aspect | Insertion Sort | Shell Sort |
|--------|----------------|------------|
| Comparison distance | Always adjacent (gap = 1) | Starts with large gaps, shrinks over time |
| Movement per step | One position at a time | Elements can jump long distances early on |
| Worst case | O(n²) | O(n²) with simple gap sequence, better with smart sequences |
| Stability | ✅ Stable | ❌ Not stable |

### Key Conceptual Difference
> **"Insertion Sort moves elements one position at a time, whereas Shell Sort allows elements to move across larger gaps first — this dramatically reduces the total number of shift operations needed before the array becomes 'nearly sorted,' at which point the final gap=1 pass (plain Insertion Sort) finishes quickly."**

---

<a name="part-19"></a>
# PART 19 — RECURRENCE RELATIONS ⭐⭐⭐

A **recurrence relation** expresses the running time of a recursive algorithm as a function of the running time on smaller inputs — because loop-counting techniques don't directly apply to recursive code.

## Why We Need Them

```text
MergeSort()
    MergeSort(left half)
    MergeSort(right half)
    Merge(...)
```

Since `MergeSort` calls itself, we can't just "count loop iterations" — instead we express its total work as an equation relating `T(n)` (time for size `n`) to `T(n/2)` (time for the half-sized sub-problems), plus the extra non-recursive work done at each step (here, the `O(n)` merge step).

## Worked Example — Deriving Merge Sort's Recurrence

If an algorithm divides a problem of size `n` into **two equal halves** and spends `O(n)` time combining the results:
```
T(n) = 2T(n/2) + n
```
This is precisely the recurrence relation for Merge Sort — `2` recursive calls, each on a problem of size `n/2`, plus `O(n)` for the merge step.

---

<a name="part-20"></a>
# PART 20 — METHODS TO SOLVE RECURRENCE RELATIONS

There are three classical techniques:

1. **Substitution Method** — guess the solution, then prove it by induction.
2. **Recursion Tree Method** — visually expand the recursion into a tree and sum the cost at each level.
3. **Master Theorem** — a direct "plug-and-chug" formula for recurrences of a specific standard form.

> Most sessional syllabi (including this one) focus specifically on **Substitution** and the **Master Theorem** — prioritize those two, but understanding the Recursion Tree method (Part 21b below) makes both of the others far more intuitive.

---

<a name="part-21"></a>
# PART 21 — SUBSTITUTION METHOD ⭐⭐⭐

The substitution method proceeds in **two stages**:

### Stage 1 — Guess the Answer
Based on intuition or the recursion's structure, hypothesize a closed-form bound (e.g., "I think this is O(n log n)").

### Stage 2 — Prove the Guess by Induction
Substitute the guessed form back into the recurrence and verify the inequality holds for an appropriate constant.

## Worked Example

Consider:
```
T(n) = 2T(n/2) + n
```

**Guess:** `T(n) = O(n log n)`, i.e., assume `T(n) ≤ c·n·log n` for some constant `c` and all sufficiently large `n`.

**Inductive hypothesis:** Assume this bound holds for the smaller sub-problem:
```
T(n/2) ≤ c(n/2)·log(n/2)
```

**Substitute into the recurrence:**
```
T(n) ≤ 2[c(n/2)log(n/2)] + n
     = cn·log(n/2) + n
```

**Simplify using `log(n/2) = log n - 1`:**
```
= cn(log n - 1) + n
= cn·log n - cn + n
```

**Choose `c ≥ 1`** so that `-cn + n ≤ 0`:
```
T(n) ≤ cn·log n
```

This confirms the inductive step, so:
```
T(n) = O(n log n)   ✅ Proven
```

> **Exam tip:** Always explicitly state your inductive hypothesis, show the algebraic substitution step, and explicitly identify the constant condition (e.g., "choosing c ≥ 1 makes the inequality hold") — partial marks are awarded for each of these steps individually.

---

<a name="part-21b"></a>
# PART 21b — RECURSION TREE METHOD (Bonus, Builds Intuition)

Although your syllabus emphasizes Substitution and Master Theorem, understanding the **Recursion Tree** makes both far more intuitive — include this as a bonus tool.

## Idea
Draw the recursion as a tree: each node represents the *cost of the non-recursive work* done at that call, and children represent the recursive sub-calls. Sum the cost **level by level**, then sum across all levels.

## Worked Example — Merge Sort's `T(n) = 2T(n/2) + n`

```
Level 0:            n                    → cost = n
Level 1:        n/2   n/2                → cost = n/2 + n/2 = n
Level 2:      n/4 n/4 n/4 n/4            → cost = 4 × (n/4) = n
   ...                                   → each level costs n
Level log n:  1 1 1 1 1 1 ... (n leaves) → cost = n
```

Every level costs exactly `n`, and there are `log n + 1` levels (from `n` down to `1` by repeated halving).

**Total cost:**
```
Total = n × (log n + 1) = O(n log n)
```

This matches both the Substitution proof and the Master Theorem result (Part 22/23) — all three methods should always agree, so use the Recursion Tree as a sanity check on your Master Theorem answers.

---

<a name="part-22"></a>
# PART 22 — MASTER THEOREM ⭐⭐⭐⭐⭐

The **Master Theorem** provides a direct formula to solve recurrences of the standard "divide and conquer" form:

$$
T(n) = a \cdot T(n/b) + f(n)
$$

Where:
- **`a`** = number of recursive sub-problems generated at each call
- **`n/b`** = size of each sub-problem (input shrinks by factor `b`)
- **`f(n)`** = cost of the work done *outside* the recursive calls (dividing + combining)

## Step 1 — Compute the Watershed Function

$$
n^{\log_b a}
$$

This represents the cost if all the work were concentrated at the *leaves* of the recursion tree.

## Step 2 — Compare `f(n)` Against `n^(log_b a)`

There are **three cases**:

### CASE 1 — Leaves Dominate
If `f(n) = O(n^(log_b a - ε))` for some constant `ε > 0` (i.e., `f(n)` grows **polynomially slower**):
$$
T(n) = \Theta(n^{\log_b a})
$$

**Worked Example:**
```
T(n) = 4T(n/2) + n
a = 4, b = 2, f(n) = n
n^(log₂4) = n²
```
Since `f(n) = n` grows slower than `n²`:
```
T(n) = Θ(n²)     [Case 1]
```

### CASE 2 — Balanced Growth
If `f(n) = Θ(n^(log_b a) · log^k n)` for some `k ≥ 0` (i.e., `f(n)` matches the watershed function, possibly times a log factor):
$$
T(n) = \Theta(n^{\log_b a} \cdot \log^{k+1} n)
$$

**Worked Example — Merge Sort:**
```
T(n) = 2T(n/2) + n
a = 2, b = 2, f(n) = n
n^(log₂2) = n¹ = n
```
Since `f(n) = Θ(n)` matches `n^(log_b a) = n` exactly (with `k = 0`):
```
T(n) = Θ(n log n)     [Case 2]
```

### CASE 3 — Combine Step Dominates
If `f(n) = Ω(n^(log_b a + ε))` for some `ε > 0` (i.e., `f(n)` grows **polynomially faster**), **and** the regularity condition (`a·f(n/b) ≤ c·f(n)` for some `c < 1` and large `n`) holds:
$$
T(n) = \Theta(f(n))
$$

**Worked Example:**
```
T(n) = 2T(n/2) + n²
a = 2, b = 2
n^(log₂2) = n
f(n) = n²  (grows faster than n)
```
```
T(n) = Θ(n²)     [Case 3]
```

## ⭐ Master Theorem Cheat Sheet

For `T(n) = aT(n/b) + f(n)`, first compute `n^(log_b a)`, then compare with `f(n)`:

| Comparison | Case | Result |
|------------|------|--------|
| `f(n)` grows **slower** | 1 | `Θ(n^(log_b a))` |
| `f(n)` grows **at the same rate** | 2 | `Θ(n^(log_b a) · log n)` |
| `f(n)` grows **faster** (+ regularity holds) | 3 | `Θ(f(n))` |

> **Important limitation to mention in exams:** The Master Theorem does **not** apply to *every* recurrence — it only works for this specific `aT(n/b) + f(n)` form with constant `a ≥ 1`, `b > 1`. Recurrences like `T(n) = T(n-1) + T(n-2)` (Fibonacci-style) or `T(n) = 2T(n/2) + n log n` (Case 2 doesn't cleanly apply here — actually needs the extended/generalized Master Theorem, since `n log n` is `Θ(n log^1 n)` which *does* fit Case 2 with `k=1`, giving `Θ(n log²n)`) require other techniques or the extended version of the theorem.

---

<a name="part-23"></a>
# PART 23 — MERGE SORT USING MASTER THEOREM ⭐⭐⭐⭐⭐

Merge Sort's recurrence:
```
T(n) = 2T(n/2) + O(n)
```

Mapping to the standard form `T(n) = aT(n/b) + f(n)`:
```
a = 2,  b = 2,  f(n) = n
```

**Compute the watershed:**
```
n^(log_b a) = n^(log₂2) = n¹ = n
```

**Compare:** `f(n) = n` is `Θ(n^(log_b a))` exactly → **Case 2** applies with `k = 0`.

**Result:**
```
T(n) = Θ(n log n)
```

Hence, for Merge Sort:
```
Best    = O(n log n)
Average = O(n log n)
Worst   = O(n log n)
```

---

<a name="part-24"></a>
# PART 24 — QUICK SORT VIA RECURRENCE

## 24.1 Best/Balanced Case

When the pivot splits the array roughly in half each time:
```
T(n) = 2T(n/2) + O(n)
```
By the Master Theorem (identical structure to Merge Sort, Case 2):
```
T(n) = O(n log n)
```

## 24.2 Worst Case — Full Derivation

When the pivot is always the smallest or largest element (e.g., an already-sorted array with last-element pivoting), one sub-problem has size `0` and the other has size `n-1`:
```
T(n) = T(n-1) + O(n)
```

**Expand step by step:**
```
T(n) = T(n-1) + n
     = [T(n-2) + (n-1)] + n
     = T(n-2) + (n-1) + n
     = T(n-3) + (n-2) + (n-1) + n
     ...
     = T(1) + 2 + 3 + ... + (n-1) + n
```

Using the arithmetic series formula `1 + 2 + ... + n = n(n+1)/2`:
```
T(n) = T(1) + [n(n+1)/2 - 1]
     = O(n²)
```

**Note:** This recurrence `T(n) = T(n-1) + O(n)` does **not** fit the Master Theorem's standard form (since it isn't of the form `aT(n/b) + f(n)` with a proportional sub-problem size) — this is a good example to point out that the *Substitution* or *Recursion Tree* method must be used here, not the Master Theorem directly.

---

<a name="part-25"></a>
# PART 25 — RADIX SORT — FULL WORKED EXAMPLE ⭐⭐⭐

Sort the following 3-digit numbers:
```
329, 457, 657, 839, 436, 720, 355
```

Since all numbers have 3 digits, Radix Sort performs exactly **3 passes** (one per digit position).

## Pass 1 — Sort by Units Digit

| Number | Units Digit |
|--------|-------------|
| 329 | 9 |
| 457 | 7 |
| 657 | 7 |
| 839 | 9 |
| 436 | 6 |
| 720 | 0 |
| 355 | 5 |

Stable sort by units digit → **`720, 436, 355, 457, 657, 329, 839`**

## Pass 2 — Sort by Tens Digit

| Number | Tens Digit |
|--------|------------|
| 720 | 2 |
| 436 | 3 |
| 355 | 5 |
| 457 | 5 |
| 657 | 5 |
| 329 | 2 |
| 839 | 3 |

Stable sort by tens digit → **`720, 329, 436, 839, 355, 457, 657`**

Notice: among elements sharing the same tens digit (e.g., `355, 457, 657` all have tens digit `5`), their relative order from Pass 1 (`355` before `457` before `657`) is **preserved** — this is the stability requirement in action.

## Pass 3 — Sort by Hundreds Digit

| Number | Hundreds Digit |
|--------|-----------------|
| 720 | 7 |
| 329 | 3 |
| 436 | 4 |
| 839 | 8 |
| 355 | 3 |
| 457 | 4 |
| 657 | 6 |

Stable sort by hundreds digit → **Final Sorted Order:**
```
329, 355, 436, 457, 657, 720, 839
```

✅ Fully sorted in exactly 3 passes, each running in O(n + k) time with k = 10 (decimal digits) — total time O(3(n+10)) = **O(dn)** where `d = 3`.

---

<a name="part-26"></a>
# PART 26 — CORE COMPLEXITY CONCEPTS

## O(1) — Constant Time
Independent of input size — the same number of operations regardless of how large `n` is.
```java
x = arr[5];   // always exactly 1 operation
```

## O(log n) — Logarithmic Time
Problem size shrinks by a constant factor (commonly halved) at each step.
```
n → n/2 → n/4 → n/8 → ... → 1
```
Classic example: **Binary Search** — each comparison eliminates half the remaining search space.

## O(n) — Linear Time
A single complete pass/traversal over the input.
```java
for (int i = 0; i < n; i++) { ... }
```

## O(n²) — Quadratic Time
Typically arises from **nested loops**, each running proportional to `n`.
```java
for (i = 0; i < n; i++)
    for (j = 0; j < n; j++) { ... }
```
Roughly `n × n = n²` total operations.

## O(n log n) — Linearithmic Time
Common in efficient divide-and-conquer sorting algorithms.
```
Merge Sort (all cases)
Quick Sort (average/best case)
```

---

<a name="part-27"></a>
# PART 27 — ANALYZING LOOP COMPLEXITY

A step-by-step toolkit for determining Big-O from code structure — extremely common as a "find the complexity" question.

### Example 1 — Single Linear Loop
```java
for (int i = 0; i < n; i++) { ... }
```
Runs exactly `n` times → **O(n)**

### Example 2 — Nested Linear Loops
```java
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++) { ... }
```
Inner loop runs `n` times for each of the `n` outer iterations → `n × n` → **O(n²)**

### Example 3 — Multiplicative Loop Variable
```java
for (int i = 1; i < n; i *= 2) { ... }
```
The variable `i` takes values `1, 2, 4, 8, 16, ...` — it doubles each time. The number of iterations until `i ≥ n` is `log₂ n` → **O(log n)**

### Example 4 — Combined Linear + Logarithmic Loops
```java
for (int i = 0; i < n; i++)
    for (int j = 1; j < n; j *= 2) { ... }
```
Outer loop: `O(n)` iterations. Inner loop: `O(log n)` iterations *for each* outer iteration. Total → **O(n log n)**

> **General rule of thumb:** *Nested* loops **multiply** their individual complexities; *sequential* (one-after-another) loops **add** their complexities (and the larger one dominates after dropping lower-order terms).

---

<a name="part-28"></a>
# PART 28 — KEY DEFINITIONS GLOSSARY

Quick-reference one-liners — perfect for last-minute revision or "define the following" questions.

| Term | Definition |
|------|------------|
| **Sorting** | The process of arranging data elements into a particular order, usually ascending or descending. |
| **Stable Sorting** | A sorting algorithm that preserves the relative order of elements with equal keys. |
| **In-place Sorting** | A sorting algorithm that rearranges elements using only a small, constant amount of additional memory. |
| **Comparison Sorting** | A sorting algorithm that determines element order strictly by comparing elements pairwise. |
| **Non-comparison Sorting** | A sorting algorithm that determines element order using structural properties of the keys (digits, range, frequency) rather than direct comparisons. |
| **Algorithm** | A finite sequence of well-defined, unambiguous instructions to solve a specific problem. |
| **Time Complexity** | A measure of the number of operations an algorithm performs as a function of input size. |
| **Space Complexity** | A measure of the total memory an algorithm requires as a function of input size. |
| **Recurrence Relation** | An equation that expresses the running time of a recursive algorithm in terms of its running time on smaller inputs. |
| **Divide and Conquer** | An algorithmic paradigm that solves a problem by dividing it into smaller sub-problems, solving each recursively, and combining their solutions. |

---

<a name="part-29"></a>
# PART 29 — IMPORTANT EXAM QUESTIONS (RANKED BY PRIORITY)

## 🔴 Very High Priority

**Q1. Define an algorithm. Explain its characteristics.**
→ Cover: Definition, Input, Output, Definiteness, Finiteness, Effectiveness (Part 1).

**Q2. Explain asymptotic notations with examples.**
→ Cover: O, Ω, Θ — formal definitions, examples, and the memory trick (Part 4).

**Q3. Explain best, average, and worst-case complexity.**
→ Use Linear Search as your worked example (Part 3).

**Q4. Explain Quick Sort with algorithm and complexity analysis.**
→ Must cover: Pivot selection, Partition scheme, Recursive structure, Best/Average/Worst case derivations, Space complexity (Part 12).

**Q5. Explain Merge Sort with algorithm and complexity analysis.**
→ Must cover: Divide step, Recursive sorting, Merge step, Recurrence relation, Master Theorem application, final O(n log n) result (Part 13, 23).

**Q6. Solve a given recurrence relation using the Master Theorem.**
→ Practice especially: `T(n) = 2T(n/2) + n` (Part 22, 23).

**Q7. Explain the substitution method for solving recurrence relations.**
→ Show both the "guess" and the "prove by induction" stages explicitly (Part 21).

**Q8. Explain Radix Sort with a worked example and complexity analysis.**
→ Must cover: Digit-by-digit sorting, LSD ordering, why stability is required, digit count `d`, final O(d(n+k)) (Part 15, 25).

## 🟠 Medium Priority

**Q9.** Explain Insertion Sort with algorithm and complexity (Part 9).
**Q10.** Explain Shell Sort and how it improves upon Insertion Sort (Part 10, 18).
**Q11.** Compare Quick Sort, Merge Sort, and Radix Sort (Part 17).
**Q12.** Explain in-place and out-of-place sorting with examples (Part 6).
**Q13.** Explain stable and unstable sorting with examples (Part 7).
**Q14.** Explain Counting Sort with algorithm and complexity (Part 14).
**Q15.** Compare all sorting algorithms by time and space complexity (Part 16).

## 🟡 Good to Know

**Q16.** Explain the Recursion Tree method and use it to verify a Master Theorem result (Part 21b).
**Q17.** Why doesn't the Master Theorem apply to every recurrence? Give a counter-example (Part 22, 24).
**Q18.** What is the lower bound for comparison-based sorting, and why? (Part 8)
**Q19.** How does randomization help mitigate Quick Sort's worst case? (Part 12.7)

---

<a name="part-30"></a>
# PART 30 — ONE-PAGE REVISION TABLE

Memorize this table in full before your exam — it condenses ~80% of the numerical/comparative question space.

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Insertion | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | ❌ |
| Shell | Depends | Depends | O(n²)* | O(1) | ❌ |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) avg | ❌ |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ |
| Counting | O(n+k) | O(n+k) | O(n+k) | O(n+k) | ✅* |
| Radix | O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) | ✅* |

`*` = implementation/gap-sequence dependent, as detailed in the relevant sections above.

---

<a name="part-31"></a>
# PART 31 — MEMORIZE VS UNDERSTAND

Don't try to memorize this entire handbook word-for-word — focus your effort strategically.

## ✅ Memorize These

**Definitions:**
- Algorithm, Complexity, Asymptotic notation, Stable sorting, In-place sorting, Out-of-place sorting, Comparison sorting

**Formulas:**
```
Quick Sort:    Average = O(n log n),  Worst = O(n²)
Merge Sort:    All cases = O(n log n)
Counting Sort: O(n + k)
Radix Sort:    O(d(n + k))
```

**Master Theorem structure:**
```
T(n) = aT(n/b) + f(n)
```
...and the three cases that follow from comparing `f(n)` to `n^(log_b a)`.

## 🧠 Understand These (Don't Just Memorize)

| Algorithm | Core Mental Model |
|-----------|--------------------|
| Quick Sort | Pivot → Partition → Recursively sort each side |
| Merge Sort | Divide → Recursively sort → Merge sorted halves |
| Insertion Sort | Take key → Shift larger elements right → Insert key in the gap |
| Shell Sort | Large gap → Progressively smaller gap → gap = 1 (plain insertion) |
| Counting Sort | Count frequencies → Reconstruct output from counts |
| Radix Sort | Sort stably by units → tens → hundreds → ... |
| Recurrence Relations | Recursive work (sub-problems) + non-recursive work (combine step) |

---

<a name="part-32"></a>
# PART 32 — FINAL MENTAL MAP & STUDY ORDER

## 🧠 Conceptual Map

```
                              ALGORITHMS
                                   │
                 ┌─────────────────┴─────────────────┐
                 │                                    │
          Algorithm Basics                        Complexity
                 │                                    │
         Characteristics                ┌─────────────┼─────────────┐
     (Input/Output/Definiteness/        │             │             │
      Finiteness/Effectiveness)       Best         Average        Worst
                                                       │
                                            Asymptotic Notation
                                                       │
                                        ┌──────────────┼──────────────┐
                                        │              │              │
                                        O              Ω              Θ
                                   (Upper bound)  (Lower bound)  (Tight bound)

                                        SORTING
                                           │
                     ┌─────────────────────┴──────────────────────┐
                     │                                             │
             Comparison Sorting                          Non-Comparison Sorting
                     │                                             │
      ┌──────┬───────┼────────┬────────┐                  ┌───────┴────────┐
      │      │       │        │        │                  │                │
 Insertion Shell  Selection  Quick   Merge              Counting          Radix
                                │        │
                       Divide & Conquer (both)
                                │
                          Recurrence Relations
                                │
                 ┌──────────────┼───────────────┐
                 │               │               │
          Substitution    Recursion Tree    Master Theorem
```

## 🎯 Recommended Study Order (If Time Is Limited)

```
 1. Asymptotic Notations
 2. Best / Average / Worst Case
 3. Insertion Sort
 4. Quick Sort              ⭐
 5. Merge Sort              ⭐
 6. Recurrence Relations    ⭐
 7. Master Theorem          ⭐⭐⭐
 8. Radix Sort              ⭐
 9. Counting Sort
10. Shell Sort
11. Stable / In-place / Comparison concepts
12. Comparison table (Part 16)
```

## 🏆 Highest-Value Combination

If your time is severely limited, master this exact combination — it covers the overwhelming majority of both conceptual and numerical sessional questions:

> **Quick Sort + Merge Sort + Radix Sort + Asymptotic Notation + Recurrence Relations + Master Theorem**

Be able to write, from memory, for each of the three sorting algorithms above:
1. A one-paragraph explanation of the core idea
2. The pseudocode
3. One fully worked dry-run example
4. Best/Average/Worst case complexity **with justification** (not just the final answer)
5. Space complexity and stability

If you can reproduce all five of those points cleanly on paper for Quick Sort, Merge Sort, and Radix Sort — plus confidently apply the Master Theorem to a fresh recurrence — you are well prepared for the sessional exam.
