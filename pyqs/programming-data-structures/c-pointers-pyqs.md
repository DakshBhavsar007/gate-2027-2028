# GATE CS/IT Past Year Questions: C Pointers & Address Arithmetic
**Section:** Programming & Data Structures (Topic E1)  
**Coverage:** 2010 – 2024 Past Papers  
**Focus:** Step-by-Step Proofs & Distractor Traps  

---

### Question 1 (GATE CS 2019 — 2 Marks)
**Consider the following C code snippet:**
```c
#include <stdio.h>
int main() {
    int a[5] = {1, 2, 3, 4, 5};
    int *ptr = (int*)(&a + 1);
    printf("%d, %d", *(a + 1), *(ptr - 1));
    return 0;
}
```
**What is the output of the program?**  
(A) 2, 5  
(B) 1, 5  
(C) 2, 4  
(D) Garbage, 5  

#### Step-by-Step Proof:
1. `a` is an array of 5 integers. Suppose its base address is `1000`.
   - `sizeof(int) = 4` bytes.
   - `a[0]` is at 1000, `a[1]` at 1004, `a[2]` at 1008, `a[3]` at 1012, `a[4]` at 1016.
2. In `*(a + 1)`:
   - `a` decays to pointer to first element (`int*`).
   - `a + 1` advances by `1 * sizeof(int) = 4` bytes to address `1004`.
   - `*(a + 1)` accesses `a[1]`, which is **`2`**.
3. In `&a + 1`:
   - `&a` has type `int (*)[5]` (pointer to an entire array of 5 integers).
   - Therefore, `&a + 1` advances by `1 * sizeof(int[5]) = 20` bytes to address `1020` (just past `a[4]`).
   - `(int*)` casts this address to a normal integer pointer `ptr = 1020`.
4. In `*(ptr - 1)`:
   - `ptr` is of type `int*`, so subtracting 1 steps backward by `1 * sizeof(int) = 4` bytes.
   - Address becomes `1020 - 4 = 1016`.
   - Address `1016` holds `a[4]`, which is **`5`**.
5. Output is **`2, 5`**.

#### Distractor Trap Analysis:
* **Why Option (B) `1, 5` is tempting:** Students forget that array indexing is 0-based and assume `*(a+1)` is `a[0]`.
* **Why Option (C) `2, 4` is tempting:** Students treat `&a` as `int*`, computing `&a + 1` as `1004`, then `ptr - 1` as `1000` or think `ptr - 1` points to `a[3] = 4`.
* **Correct Answer:** **(A)**

---

### Question 2 (GATE CS 2017 — 2 Marks)
**Consider the following C program:**
```c
#include <stdio.h>
void fun(int *p, int *q) {
    p = q;
    *p = 2;
}
int i = 0, j = 1;
int main() {
    fun(&i, &j);
    printf("%d %d\n", i, j);
    return 0;
}
```
**What does the program print?**  
(A) 0 2  
(B) 2 1  
(C) 2 2  
(D) 0 1  

#### Step-by-Step Proof:
1. `main` passes addresses of global variables `i` and `j` to `fun`.
   - `fun` receives formal pointer parameters `p` and `q`. In C, **parameters are strictly passed by value** — meaning `p` and `q` are local pointer variables inside `fun`'s stack frame initialized to `&i` and `&j`.
2. Inside `fun`:
   - `p = q;`: Local pointer variable `p` is reassigned to hold the address stored in `q` (which is `&j`). Note that this does **NOT** modify variable `i` or the caller's arguments.
   - `*p = 2;`: Dereferences `p` (now pointing to `j`) and writes `2` into memory of `j`.
3. When `fun` returns:
   - `i` was never modified (`i = 0`).
   - `j` was updated to `2`.
4. `printf("%d %d\n", i, j)` prints **`0 2`**.

#### Distractor Trap Analysis:
* **Why Option (C) `2 2` or (B) `2 1` is chosen:** Students falsely believe `p = q` copies the value from `q` into the variable pointed to by `p` (confusing `p = q` with `*p = *q`).
* **Correct Answer:** **(A)**

---

### Question 3 (GATE CS 2015 — 2 Marks)
**Consider the following C code on a standard 64-bit architecture:**
```c
#include <stdio.h>
void f(int a[100]) {
    printf("%zu", sizeof(a));
}
int main() {
    int arr[100];
    f(arr);
    return 0;
}
```
**What is the output?**  
(A) 8  
(B) 400  
(C) 100  
(D) 4  

#### Step-by-Step Proof:
1. In C, an array parameter in a function header `void f(int a[100])` is **automatically converted by the compiler to a pointer declaration**:
   `void f(int *a)`
   The dimension `[100]` is completely ignored by the compiler.
2. Inside `f`, `a` is a pointer variable (`int*`).
3. On a 64-bit architecture, **all memory pointers occupy exactly 8 bytes** (64 bits), regardless of the data type they point to.
4. Hence, `sizeof(a)` evaluates to `sizeof(int*) = 8`.

#### Distractor Trap Analysis:
* **Why Option (B) `400` is the primary trap:** Students calculate `100 * sizeof(int) = 400 bytes`. That is true for `sizeof(arr)` inside `main`, but NOT inside function `f` where it has decayed to a pointer!
* **Correct Answer:** **(A)**

---

### Question 4 (GATE CS 2021 — 1 Mark)
**What does the following C code print?**
```c
#include <stdio.h>
int main() {
    int arr[] = {10, 20, 30, 40};
    int *p = arr;
    printf("%d ", *p++);
    printf("%d", *p);
    return 0;
}
```
**Options:**  
(A) 10 20  
(B) 20 20  
(C) 11 20  
(D) 10 10  

#### Step-by-Step Proof:
1. `p` is initialized to `arr` (points to `arr[0] = 10`).
2. First `printf`: `*p++`:
   - Postfix `++` has higher precedence than dereference `*`.
   - The expression evaluates to `*(p++)`.
   - Because it is postfix, the current address `p` is used for dereferencing: `*p` yields `10`.
   - As a side-effect, `p` is incremented to point to `arr[1]`.
   - First value printed: **`10`**.
3. Second `printf`: `*p`:
   - `p` now points to `arr[1]`, so `*p` yields **`20`**.
4. Output is **`10 20`**.

#### Distractor Trap Analysis:
* **Why Option (C) `11 20` is chosen:** Confusing `*p++` with `(*p)++` (which increments the integer value in memory, not the pointer).
* **Correct Answer:** **(A)**
