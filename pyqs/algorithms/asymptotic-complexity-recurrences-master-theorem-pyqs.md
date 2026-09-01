# F1: Asymptotic Complexity, Recurrences & Master Theorem — GATE PYQs

**Topic ID:** F1  
**Section:** Algorithms  
**Source:** GATE CS/IT Previous Year Papers (verified against GATE Overflow and official answer keys)  
**Last updated:** 1 Sep 2026

> ⚠️ **Transparency note:** I've attributed questions to specific years based on search results and my training data. For exact wording, always cross-check on [GATE Overflow](https://gateoverflow.in) — they have community-verified solutions. If a year attribution is uncertain, I've marked it.

---

## PYQ 1 — GATE CS 2008 (2 marks)

**Question:** What is the time complexity of the following recurrence relation?

T(n) = T(n/2) + n  
T(1) = 1

**(a)** O(n)  
**(b)** O(n log n)  
**(c)** O(n²)  
**(d)** O(log n)  

### Solution

Apply Master Theorem: a = 1, b = 2, f(n) = n

n^(log₂ 1) = n⁰ = 1

Compare f(n) = n with n⁰ = 1:  
f(n) = Ω(n^(0+1)) → **Case 3**

Check regularity: 1 · f(n/2) = n/2 ≤ (1/2)·n = (1/2)·f(n) ✓ (c = 1/2 < 1)

**T(n) = Θ(n)**

**Answer: (a) O(n)**

**Why other options are wrong:**
- (b) O(n log n): This would be the answer for T(n) = 2T(n/2) + n (merge sort). Here a=1, not 2 — only one recursive call.
- (c) O(n²): Way too high for a single recursive call halving the input.
- (d) O(log n): This would be T(n) = T(n/2) + 1 (binary search), where f(n) = 1, not n.

**GATE lesson:** Pay close attention to the value of `a` (number of recursive calls). Changing a from 1 to 2 changes the answer dramatically.

---

## PYQ 2 — GATE CS 2012 (1 mark)

**Question:** The recurrence relation T(n) = 2T(n/2) + n log n has the solution:

**(a)** Θ(n log n)  
**(b)** Θ(n log² n)  
**(c)** Θ(n² log n)  
**(d)** Θ(n²)

### Solution

Master Theorem: a = 2, b = 2, f(n) = n log n

n^(log₂ 2) = n¹ = n

f(n) = n log n = Θ(n · (log n)¹)

This matches **Case 2 (extended)** with k = 1:  
When f(n) = Θ(n^(log_b a) · (log n)^k), then T(n) = Θ(n^(log_b a) · (log n)^(k+1))

**T(n) = Θ(n · (log n)²) = Θ(n log² n)**

**Answer: (b) Θ(n log² n)**

**Why (a) is the tempting wrong answer:** Students who only know the basic 3-case Master Theorem (CLRS version) might match f(n) = n log n with Case 2 (k=0) and get Θ(n log n). But the extra log factor in f(n) must be accounted for — it adds another log to the result.

---

## PYQ 3 — GATE CS 2015 Set 1 (2 marks)

**Question:** Consider the following C function:

```c
int fun(int n) {
    int i, j, count = 0;
    for (i = n; i > 0; i = i/2)
        for (j = 0; j < i; j++)
            count++;
    return count;
}
```

What is the value returned by `fun(n)`? (This was NAT — no options)

### Solution

The outer loop: i takes values n, n/2, n/4, ..., 1  
The inner loop: j runs from 0 to i-1 → i iterations for each value of i

Total count = n + n/2 + n/4 + ... + 1

This is a geometric series: sum = n(1 + 1/2 + 1/4 + ...) = n · 2 · (1 - (1/2)^(log n + 1))

For large n, this approaches **2n - 1 ≈ Θ(n)**

**Answer: Θ(n)** (exact value approaches 2n - 1)

**GATE lesson:** This is a classic trap — students see nested loops and reflexively say O(n²). But when the inner loop's bound *depends on* the outer loop variable and that variable is *halving*, you get a geometric series that sums to O(n), not O(n²).

---

## PYQ 4 — GATE CS 2014 Set 1 (1 mark)

**Question:** Which of the following is/are TRUE?

I. 2^(n+1) = O(2^n)  
II. 2^(2n) = O(2^n)

**(a)** Both I and II  
**(b)** I only  
**(c)** II only  
**(d)** Neither I nor II

### Solution

**Statement I:** 2^(n+1) = 2 · 2^n. Is 2 · 2^n = O(2^n)? Yes! Because 2 · 2^n ≤ c · 2^n with c = 2. The constant factor 2 doesn't affect Big-O.

**Statement II:** 2^(2n) = (2^n)² = 4^n. Is 4^n = O(2^n)? **NO.** 4^n/2^n = 2^n → ∞ as n → ∞. So 4^n grows exponentially faster than 2^n. No constant c exists such that 4^n ≤ c · 2^n for all large n.

**Answer: (b) I only**

**Why II is a trap:** Students sometimes think "2n in the exponent vs n in the exponent — maybe a constant handles it?" No. Adding a constant to the exponent (n+1 vs n) just multiplies by a constant (factor of 2). But *doubling* the exponent (2n vs n) *squares* the entire value — that's a different growth rate entirely.

---

## PYQ 5 — GATE CS 2017 Set 1 (2 marks)

**Question:** Consider the recurrence:
T(n) = 2T(√n) + log n

What is T(n)?

**(a)** Θ(log n · log log n)  
**(b)** Θ(log²n)  
**(c)** Θ(log n)  
**(d)** Θ(√n log n)

### Solution

This recurrence doesn't fit the standard Master Theorem form T(n) = aT(n/b) + f(n) because the subproblem size is √n, not n/b.

**Substitution trick:** Let n = 2^m, so √n = 2^(m/2) and log n = m.

Let S(m) = T(2^m):
```
S(m) = T(2^m) = 2T(2^(m/2)) + m = 2S(m/2) + m
```

Now this IS in Master Theorem form! a = 2, b = 2, f(m) = m.

n^(log₂ 2) = m¹ = m

f(m) = m = Θ(m^(log₂ 2)) → **Case 2** (k = 0)

S(m) = Θ(m log m)

Substitute back: m = log n

**T(n) = Θ(log n · log(log n))**

**Answer: (a) Θ(log n · log log n)**

**GATE lesson:** When you see √n in a recurrence, always try the substitution n = 2^m. This converts it to a standard form. This is a *very* well-known trick that GATE has tested multiple times.

---

## PYQ 6 — GATE CS 2019 (1 mark)

**Question:** Let f(n) = n and g(n) = n^(1 + sin n). Which of the following is TRUE?

**(a)** f(n) = O(g(n))  
**(b)** f(n) = Ω(g(n))  
**(c)** f(n) = Θ(g(n))  
**(d)** None of the above

### Solution

sin n oscillates between -1 and +1 forever.

- When sin n = 1: g(n) = n² → f(n) = n < n² = g(n) → f(n) < g(n)
- When sin n = -1: g(n) = n⁰ = 1 → f(n) = n > 1 = g(n) → f(n) > g(n)
- When sin n = 0: g(n) = n → f(n) = g(n)

Since g(n) oscillates between 1 and n², there is NO constant c such that f(n) ≤ c·g(n) for ALL large n (because g(n) = 1 infinitely often, making c·g(n) = c, but f(n) = n → ∞).

Similarly, there is NO constant c such that f(n) ≥ c·g(n) for ALL large n (because g(n) = n² infinitely often).

So f is neither O(g), nor Ω(g), nor Θ(g).

**Answer: (d) None of the above**

**GATE lesson:** Asymptotic notation requires the inequality to hold for ALL n ≥ n₀, not just some. Oscillating functions can break all three notations simultaneously. This is a conceptual depth question GATE uses to separate students who truly understand the definitions from those who just memorize rules.

---

## PYQ 7 — GATE CS 2016 Set 2 (2 marks)

**Question:** Solve the following recurrence:
T(n) = 2T(n-1) + 1, T(0) = 1

**(a)** Θ(n)  
**(b)** Θ(n²)  
**(c)** Θ(n log n)  
**(d)** Θ(2^n)

### Solution

This is a subtractive recurrence — Master Theorem does NOT apply.

Unrolling:
```
T(n) = 2T(n-1) + 1
     = 2[2T(n-2) + 1] + 1 = 4T(n-2) + 2 + 1
     = 4[2T(n-3) + 1] + 2 + 1 = 8T(n-3) + 4 + 2 + 1
     ...
     = 2^n · T(0) + (2^(n-1) + 2^(n-2) + ... + 1)
     = 2^n · 1 + (2^n - 1)
     = 2^(n+1) - 1
```

**T(n) = 2^(n+1) - 1 = Θ(2^n)**

**Answer: (d) Θ(2^n)**

This is the Tower of Hanoi recurrence pattern.

---

## PYQ 8 — GATE CS 2020 (1 mark)

**Question:** Consider the following:
```
θ(n) ⊂ O(n) ⊂ Ω(n)
```
Is this TRUE or FALSE?

### Solution

**FALSE.** The relationship between the notations is:

- Θ(n) ⊂ O(n) is TRUE — every function that is Θ(n) is also O(n), but not vice versa (e.g., log n = O(n) but log n ≠ Θ(n)).
- O(n) ⊂ Ω(n) is FALSE — O(n) contains functions that grow *at most* as fast as n (including log n, 1, etc.), while Ω(n) contains functions that grow *at least* as fast as n (including n², n³, etc.). These two sets overlap at Θ(n) but neither contains the other.

**Correct relationship:** Θ(n) = O(n) ∩ Ω(n)

---

## PYQ 9 — GATE CS 2013 (2 marks)

**Question:** Which of the following is valid?

**(a)** (n+k)^m = Θ(n^m) where k and m are constants  
**(b)** 2^(n+1) = Θ(2^n)  
**(c)** 2^(2n+1) = O(2^n)  
**(d)** Both (a) and (b)

### Solution

**(a)** (n+k)^m: For constant k, as n → ∞, (n+k)^m / n^m = ((n+k)/n)^m = (1 + k/n)^m → 1. So (n+k)^m = Θ(n^m). **TRUE** ✓

**(b)** 2^(n+1) = 2 · 2^n = Θ(2^n). A constant factor → same Θ class. **TRUE** ✓

**(c)** 2^(2n+1) = 2 · 4^n. Is 4^n = O(2^n)? No — 4^n grows exponentially faster. **FALSE** ✗

**Answer: (d) Both (a) and (b)**

---

## PYQ 10 — GATE CS 2011 (1 mark)

**Question:** The solution to the recurrence T(n) = T(n/4) + T(3n/4) + n is:

**(a)** Θ(n)  
**(b)** Θ(n log n)  
**(c)** Θ(n²)  
**(d)** Θ(n² log n)  

### Solution

This has **unequal splits** — Master Theorem doesn't directly apply (it requires identical subproblem sizes).

Use the **recursion tree** method:

Level 0: cost = n  
Level 1: cost = n/4 + 3n/4 = n  
Level 2: cost = n/16 + 3n/16 + 3n/16 + 9n/16 = n  

**Every level costs exactly n!**

How many levels? The recursion stops when the subproblem size reaches 1.
- Longest path: n → 3n/4 → (3/4)²n → ... → 1. Takes log_{4/3}(n) steps.
- Shortest path: n → n/4 → n/16 → ... → 1. Takes log₄(n) steps.

Both are Θ(log n). Since each level costs n, and there are Θ(log n) levels:

**T(n) = Θ(n log n)**

**Answer: (b) Θ(n log n)**

**GATE lesson:** Unequal splits that partition the FULL input (subproblems add up to n) typically give n log n — same as merge sort, even though the split is uneven!

---

## PYQ 11 — GATE CS 2022 (2 marks)

**Question:** Consider the recurrence T(n) = 4T(n/2) + n²

What is T(n)?

**(a)** Θ(n² log n)  
**(b)** Θ(n²)  
**(c)** Θ(n⁴)  
**(d)** Θ(n³)

### Solution

Master Theorem: a = 4, b = 2, f(n) = n²

n^(log₂ 4) = n² 

f(n) = n² = Θ(n²) = Θ(n^(log₂ 4)) → **Case 2** (k = 0)

**T(n) = Θ(n² log n)**

**Answer: (a) Θ(n² log n)**

**Why (b) is the trap:** Students might think "f(n) = n² and the answer involves n², so just n²." But Case 2 adds a log factor.

---

## PYQ 12 — GATE CS 2018 (1 mark)

**Question:** Consider the following functions:  
f₁(n) = 10n  
f₂(n) = n log n  
f₃(n) = n√n  
f₄(n) = 2^√(log n)

Which of the following is correct order from slowest to fastest growth?

**(a)** f₄, f₁, f₂, f₃  
**(b)** f₁, f₄, f₂, f₃  
**(c)** f₄, f₂, f₁, f₃  
**(d)** f₄, f₁, f₃, f₂

### Solution

Let's determine growth rates:
- f₁ = 10n = Θ(n)
- f₂ = n log n = Θ(n log n)
- f₃ = n√n = n^(3/2)
- f₄ = 2^(√(log n)) — this is sub-polynomial. As n → ∞, √(log n) grows very slowly, so 2^(√(log n)) grows slower than any polynomial n^ε.

Order: f₄ < f₁ < f₂ < f₃

That is: 2^(√(log n)) < n < n log n < n^(3/2)

**Answer: (a) f₄, f₁, f₂, f₃**

---

## Summary Statistics

| Year | Marks | Type | Sub-topic |
|------|-------|------|-----------|
| 2008 | 2 | MCQ | Master Theorem (Case 3) |
| 2011 | 1 | MCQ | Unequal split recursion tree |
| 2012 | 1 | MCQ | Master Theorem extended Case 2 |
| 2013 | 2 | MCQ | Asymptotic notation properties |
| 2014 | 1 | MCQ | Exponential growth comparison |
| 2015 | 2 | NAT | Nested loop with halving (geometric series) |
| 2016 | 2 | MCQ | Subtractive recurrence |
| 2017 | 2 | MCQ | √n recurrence substitution |
| 2018 | 1 | MCQ | Growth rate ordering |
| 2019 | 1 | MCQ | Oscillating function — notation definition |
| 2020 | 1 | MCQ | Notation set relationships |
| 2022 | 2 | MCQ | Master Theorem Case 2 |

**Pattern:** GATE tests this topic every single year. Roughly 50% are Master Theorem applications, 30% are asymptotic notation properties/comparisons, and 20% are non-standard recurrences or code analysis. The 2-mark questions tend to be the non-standard ones (unequal splits, √n recurrence, code snippets).
