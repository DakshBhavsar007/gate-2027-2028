# F1: Asymptotic Complexity, Recurrences & Master Theorem

**Section:** Algorithms  
**GATE weightage:** High — at least 1–2 questions every year (1-mark and 2-mark combined). This is the *language* in which every other algorithm topic is discussed. You can't answer any DS/Algo question without this.  
**Date:** 1 Sep 2026 (Day 1)

---

## Part 1: Asymptotic Notation — The Big Three

### Intuition first

Imagine you're comparing two runners over longer and longer races. You don't care who's faster at 100m — you care about who wins as the race gets infinitely long. Asymptotic notation is about **growth rate as n → ∞**, ignoring constants and lower-order terms.

**Analogy:** If runner A runs at speed `3n² + 5n` and runner B at `100n²`, they're both "quadratic runners" — over a long enough race, the constant 100 vs 3 stops mattering. What matters is the `n²` part.

### Formal Definitions

#### Big-O: O(g(n)) — Upper bound ("at most this fast")

> f(n) = O(g(n)) if ∃ positive constants c and n₀ such that  
> **f(n) ≤ c · g(n)** for all n ≥ n₀

**What it says:** f grows *no faster* than g, up to a constant factor, for large enough n.

**Example:** 3n² + 5n = O(n²) because for c = 4, n₀ = 5: 3n² + 5n ≤ 4n² when n ≥ 5.

#### Big-Ω: Ω(g(n)) — Lower bound ("at least this fast")

> f(n) = Ω(g(n)) if ∃ positive constants c and n₀ such that  
> **f(n) ≥ c · g(n)** for all n ≥ n₀

**What it says:** f grows *at least as fast* as g.

**Example:** 3n² + 5n = Ω(n²) because for c = 3, n₀ = 1: 3n² + 5n ≥ 3n².

#### Big-Θ: Θ(g(n)) — Tight bound ("exactly this fast")

> f(n) = Θ(g(n)) if ∃ positive constants c₁, c₂, and n₀ such that  
> **c₁ · g(n) ≤ f(n) ≤ c₂ · g(n)** for all n ≥ n₀

**What it says:** f grows at *exactly* the same rate as g (sandwiched between two constant multiples).

**Example:** 3n² + 5n = Θ(n²) — we proved both O(n²) and Ω(n²) above.

### Little-o and Little-ω (less common in GATE, but know the difference)

- **f(n) = o(g(n)):** f grows *strictly slower* than g. (For *all* c > 0, f(n) < c·g(n) eventually.) Example: n = o(n²).
- **f(n) = ω(g(n)):** f grows *strictly faster* than g. Example: n² = ω(n).

### The Growth Rate Ladder (memorize this ordering)

```
1 < log(log n) < log n < (log n)^k < n^ε < n < n log n < n² < n³ < ... < 2^n < 3^n < n! < n^n
```

where ε is any small positive constant and k is any positive constant.

### ⚠️ GATE Traps on Asymptotic Notation

1. **"Which of the following is TRUE?"** — GATE loves giving 4 statements mixing O, Θ, Ω, o, ω and asking which combination is correct. The trap: students confuse O (upper bound) with Θ (tight bound).
   - **n² = O(n³)** is TRUE (n² grows slower, so n³ is a valid upper bound)
   - **n² = Θ(n³)** is FALSE (n² is not sandwiched by n³)
   - **n² = O(n²)** is TRUE (every function is Big-O of itself)
   - **n² = o(n²)** is FALSE (little-o means *strictly* slower — a function is NOT little-o of itself)

2. **Constants matter for the definition, not for the answer.** GATE might say "Is 5n² + 3n + 7 = Θ(n²)?" — yes, always. Drop constants and lower-order terms.

3. **log base doesn't matter in Big-O.** log₂n = Θ(log₁₀n) because log bases differ by a constant factor (change of base formula). GATE may try to trick you with different log bases.

4. **Polynomials vs exponentials:** n^1000 = o(2^n). ANY exponential with base > 1 eventually dwarfs ANY polynomial. This comes up surprisingly often.

---

## Part 2: Analyzing Simple Code Snippets

GATE frequently gives a code snippet and asks "What is the time complexity?"

### Common Patterns

| Pattern | Complexity | Example |
|---------|-----------|---------|
| Single loop: `for i=1 to n` | O(n) | Linear scan |
| Nested loops: `for i=1 to n` → `for j=1 to n` | O(n²) | Bubble sort |
| Loop variable doubles: `i = 1; while i < n: i = i*2` | O(log n) | Binary search |
| Loop variable squares: `i = 2; while i < n: i = i²` | O(log log n) | Rare but tested |
| Two nested, inner depends on outer: `for i=1 to n` → `for j=1 to i` | O(n²) | Sum = n(n+1)/2 |
| `for i=1 to n` → `for j=1 to n; j = j*2` | O(n log n) | Outer O(n), inner O(log n) |

### Summing technique for dependent loops

When the inner loop depends on the outer variable:
```
for i = 1 to n:
    for j = 1 to i:
        // work
```
Total work = 1 + 2 + 3 + ... + n = **n(n+1)/2 = Θ(n²)**

Another variant:
```
for i = 1 to n:
    for j = i to n:
        // work
```
Total work = n + (n-1) + ... + 1 = **n(n+1)/2 = Θ(n²)** — same thing.

### The halving/doubling pattern
```
i = n
while i > 1:
    i = i / 2   // or i = i >> 1
```
Runs **⌊log₂ n⌋** times → **Θ(log n)**

---

## Part 3: Recurrence Relations

### What is a recurrence?

A recurrence defines a function in terms of its value on smaller inputs. They arise naturally from recursive algorithms.

**Example:** Merge Sort
```
T(n) = 2T(n/2) + Θ(n)
```
- Split into 2 subproblems, each of size n/2 → **2T(n/2)**
- Merge step costs linear time → **Θ(n)**

### Methods to Solve Recurrences

There are 3 methods. For GATE, you need all three because GATE specifically designs some questions that can't use the Master Theorem.

#### Method 1: Substitution (Guess and Prove by Induction)

1. Guess the answer (from experience or recursion tree).
2. Prove it correct by induction.

**Example:** T(n) = 2T(n/2) + n, guess T(n) = O(n log n)

Assume T(k) ≤ ck log k for all k < n:
```
T(n) = 2T(n/2) + n
     ≤ 2 · c(n/2)log(n/2) + n
     = cn(log n - 1) + n
     = cn·log n - cn + n
     ≤ cn·log n          (when c ≥ 1)
```
✓ Proved.

#### Method 2: Recursion Tree

Draw a tree where:
- Each node represents the non-recursive cost at that level.
- Children represent the recursive calls.
- Sum up all levels.

**Example:** T(n) = 2T(n/2) + n

```
Level 0:         n              → cost: n
Level 1:    n/2     n/2         → cost: n
Level 2:  n/4 n/4 n/4 n/4      → cost: n
...
Level k: n/2^k each             → cost: n
```

Number of levels = log₂ n (until n/2^k = 1)  
Total cost = n × log n levels = **Θ(n log n)** ✓

#### Method 3: Master Theorem (the workhorse)

Applies to recurrences of the form:

> **T(n) = aT(n/b) + f(n)**

where a ≥ 1, b > 1, and f(n) is asymptotically positive.

**Compute:** n^(log_b(a)) — this is the "critical exponent"

**Three cases:**

| Case | Condition | Result |
|------|-----------|--------|
| **Case 1** | f(n) = O(n^(log_b(a) - ε)) for some ε > 0 | T(n) = **Θ(n^(log_b(a)))** |
| **Case 2** | f(n) = Θ(n^(log_b(a)) · (log n)^k) for k ≥ 0 | T(n) = **Θ(n^(log_b(a)) · (log n)^(k+1))** |
| **Case 3** | f(n) = Ω(n^(log_b(a) + ε)) for some ε > 0, AND regularity: af(n/b) ≤ cf(n) for some c < 1 | T(n) = **Θ(f(n))** |

> **Note on Case 2:** The standard textbook (CLRS) only covers k = 0, giving T(n) = Θ(n^(log_b a) · log n). The extended version with (log n)^k handles things like T(n) = 2T(n/2) + n log n → Θ(n log² n). GATE has tested both.

### ⚠️ Critical GATE trap: When Master Theorem DOESN'T apply

The Master Theorem does **NOT** apply when:
- f(n) is between the cases (no polynomial gap). Example: T(n) = 2T(n/2) + n/log n — f(n) = n/log n is asymptotically smaller than n but NOT by a polynomial factor (no ε > 0 such that n/log n = O(n^(1-ε))).
- The subproblem sizes are unequal: T(n) = T(n/3) + T(2n/3) + n — not in the required form.
- The recurrence is not a divide-and-conquer type: T(n) = T(n-1) + n — subtractive, not divisive.

For these, use **recursion tree** or **substitution**.

### Common Subtractive Recurrences (not Master Theorem — memorize these)

| Recurrence | Solution | Algorithm |
|-----------|----------|-----------|
| T(n) = T(n-1) + 1 | Θ(n) | Linear recursion |
| T(n) = T(n-1) + n | Θ(n²) | Sum 1+2+...+n |
| T(n) = T(n-1) + log n | Θ(n log n) | — |
| T(n) = 2T(n-1) + 1 | Θ(2^n) | Tower of Hanoi |
| T(n) = T(n-1) + T(n-2) + 1 | Θ(φ^n) where φ ≈ 1.618 | Fibonacci (naive) |

### Worked Examples with Master Theorem

**Example 1:** T(n) = 9T(n/3) + n
- a=9, b=3, f(n)=n
- n^(log₃ 9) = n^2
- Compare f(n) = n with n² → f(n) = O(n^(2-1)) → **Case 1**
- **T(n) = Θ(n²)**

**Example 2:** T(n) = 2T(n/2) + n
- a=2, b=2, f(n)=n
- n^(log₂ 2) = n¹ = n
- f(n) = Θ(n) = Θ(n^(log₂ 2)) → **Case 2** (k=0)
- **T(n) = Θ(n log n)**  ← Merge Sort!

**Example 3:** T(n) = 3T(n/4) + n log n
- a=3, b=4, f(n) = n log n
- n^(log₄ 3) ≈ n^0.793
- f(n) = n log n = Ω(n^(0.793 + ε)) for ε ≈ 0.2 → **Case 3**
- Check regularity: 3(n/4)log(n/4) ≤ (3/4)n log n for large n ✓
- **T(n) = Θ(n log n)**

**Example 4:** T(n) = 2T(n/2) + n log n
- a=2, b=2, f(n) = n log n
- n^(log₂ 2) = n
- f(n) = n log n = Θ(n · (log n)¹) → **Case 2 extended** (k=1)
- **T(n) = Θ(n log² n)**

**Example 5 (Master Theorem FAILS):** T(n) = 2T(n/2) + n/log n
- a=2, b=2 → n^(log₂ 2) = n
- f(n) = n/log n — smaller than n but NOT by a polynomial factor
- Falls in the "gap" between Case 1 and Case 2 → **Master Theorem does not apply**
- Use recursion tree: each level costs n/log(n/2^k), sum over log n levels → **Θ(n log log n)**

---

## Part 4: Quick-Reference Cheat Sheet

### Standard Complexity Classes (GATE-relevant algorithms)

| Complexity | Algorithms |
|-----------|-----------|
| O(1) | Hash lookup (amortized), array index |
| O(log n) | Binary search, balanced BST operations |
| O(n) | Linear search, counting sort, single pass |
| O(n log n) | Merge sort, heap sort, best comparison sorts |
| O(n²) | Bubble/insertion/selection sort, naive matrix ops |
| O(n³) | Matrix multiplication (naive), Floyd-Warshall |
| O(2^n) | Subset enumeration, Tower of Hanoi |
| O(n!) | Permutation enumeration, brute-force TSP |

### Comparison-based sorting lower bound

> Any comparison-based sorting algorithm requires **Ω(n log n)** comparisons in the worst case.

Proof: decision tree has n! leaves → height ≥ log₂(n!) = Θ(n log n) by Stirling's approximation.

This is a *very* frequently tested fact.

---

## Summary — What to take away from Day 1

1. **Θ is the "right" notation** for most GATE answers — it gives the tight bound. Use O only when you genuinely mean "upper bound without claiming tightness."
2. **Master Theorem covers ~70% of GATE recurrence questions.** The remaining 30% deliberately break it (subtractive recurrences, gap between cases, unequal splits). Know how to handle those.
3. **The growth rate ladder** (1 < log n < √n < n < n log n < n² < 2^n < n!) is tested directly — memorize it.
4. **Log base never matters** in asymptotic notation.
5. **When in doubt, draw the recursion tree** — it always works, even when Master Theorem doesn't.

---

*Reference: Cormen et al. (CLRS) — Chapter 3 (Growth of Functions) and Chapter 4 (Divide-and-Conquer / Recurrences)*
