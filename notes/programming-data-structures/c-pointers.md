# C Programming: Pointers, Memory Models & Address Arithmetic
**Section:** Programming & Data Structures (E1)  
**GATE Weightage:** ~4–6 Marks every year (high-frequency trap zone)  
**Calibrated for:** GATE CS/IT 2027 & 2028  

---

## 1. Intuition Before Formalism: The Memory Grid

Think of physical RAM as a single, contiguous linear byte-addressed array:
```
Byte Address:  [ 1000 ] [ 1001 ] [ 1002 ] [ 1003 ] [ 1004 ] ...
Stored Value:  [ 0x00 ] [ 0x00 ] [ 0x00 ] [ 0x05 ] [ ...  ]
```
A **variable** is just a human-friendly nickname for a specific starting address with an associated type (which dictates how many bytes to read and how to interpret their bit pattern).

A **pointer** is simply a variable whose stored value is *itself an address* of another location in memory.

### The Big Mistake Students Make
Students think a pointer is just "an arrow." In GATE, pointers are **strictly typed numerical addresses** subjected to scaling arithmetic. The type tells the compiler: *“When I say `+ 1`, how many bytes forward should I jump?”*

---

## 2. Core Formal Mechanics & The Scaling Law

### A. The Address-Of (`&`) and Dereference (`*`) Inverses
* `&x`: Yields the memory address where `x` begins. Type of `&x` is `T*` if `x` is of type `T`.
* `*p`: Accesses the object located at memory address stored in `p`.

### B. Pointer Arithmetic Scaling Invariant
For any pointer `T *p` and integer `k`:
$$\text{Address}(p + k) = \text{Address}(p) + k \times \text{sizeof}(T)$$

```c
int *p = (int*) 1000;   // Assume sizeof(int) = 4 bytes
p + 1;                  // Evaluates to address 1004
p + 3;                  // Evaluates to address 1012

char *cp = (char*) 1000;
cp + 1;                 // Evaluates to address 1001
```

### C. Array-Pointer Decay Rule
In C, the name of an array `arr` decays into a pointer to its first element (`&arr[0]`) in almost all expressions, **EXCEPT in exactly two cases**:
1. When it is the operand of `sizeof(arr)`: returns total bytes of the entire array.
2. When it is the operand of unary `&` (`&arr`): returns a pointer to the **entire array** (`int (*)[N]`), NOT a pointer to an element (`int*`).

```c
int a[5] = {10, 20, 30, 40, 50};
// a      -> type: int*        (value: 1000)
// &a[0]  -> type: int*        (value: 1000)
// &a     -> type: int (*)[5]  (value: 1000, BUT pointer-to-array-of-5-ints)
```

> [!IMPORTANT]
> Notice that `a` and `&a` have the **exact same numeric address** (e.g. 1000), but **different types**!
> * `a + 1` jumps `1 * sizeof(int) = 4 bytes` (points to `a[1]`).
> * `&a + 1` jumps `1 * sizeof(int[5]) = 20 bytes` (points right past the end of the array)!

---

## 3. The 2D Array Address Calculus (GATE's Favorite Matrix Trap)

Given `int a[3][4]`:
* `a` is an array of 3 elements, where each element is an `int[4]`.
* `a[i][j]` is syntactically translated by the compiler as:
  $$*(*(a + i) + j)$$

```
Expression       Type            Jump step when '+ 1' is added
----------------------------------------------------------------------
a                int (*)[4]      4 * sizeof(int) = 16 bytes (row step)
a + i            int (*)[4]      jumps i whole rows
*(a + i)         int *           1 * sizeof(int) = 4 bytes (column step)
*(a + i) + j     int *           jumps j integers in row i
*(*(a + i) + j)  int             the integer value stored at a[i][j]
```

---

## 4. Operator Precedence Traps (Unwinding C Expressions)

In GATE, evaluate expressions using strict precedence rules:

| Rank | Operators | Associativity |
|---|---|---|
| 1 | `()` `[]` `->` `.` `postfix++` `postfix--` | Left to Right |
| 2 | `unary*` `unary&` `prefix++` `prefix--` `sizeof` | **Right to Left** |
| 3 | `+` `-` (Binary arithmetic) | Left to Right |

### Key Precedence Traps:
1. `*p++`: Postfix `++` has higher precedence than `*`. It evaluates to `*(p++)`:
   - Returns the value pointed to by `p` (`*p`).
   - Increments pointer `p` to the next element after the expression is evaluated.
2. `*++p`: Prefix `++` and `*` have same precedence, evaluated **Right to Left**:
   - First increments `p` (`++p`).
   - Then dereferences the new address (`*(++p)`).
3. `(*p)++`: Parentheses override precedence:
   - Fetches value at `*p`.
   - Increments the *value in memory* by 1.
4. `++*p`: Right-to-left:
   - Evaluates to `++(*p)`. Increments the value pointed to by `p` before using it.

---

## 5. Pointer to Functions & Complex Declarations

GATE occasionally tests decoding complex C declarations using the **Clockwise / Spiral Rule**:
* Start at the identifier, move right if possible (parentheses or brackets), then left (asterisks).

```c
int *arr[10];       // arr is an array of 10 pointers to int
int (*arr)[10];     // arr is a POINTER to an array of 10 ints
int (*fp)(int, int);// fp is a POINTER to a FUNCTION taking (int, int) returning int
int *fp(int, int);  // fp is a FUNCTION taking (int, int) returning pointer to int
```

---

## 6. How GATE Tests This: Edge Cases & Exam Traps

1. **The Post-Array Offset Trap**:
   ```c
   int a[5] = {1, 2, 3, 4, 5};
   int *ptr = (int*)(&a + 1);
   printf("%d", *(ptr - 1)); // Prints 5!
   ```
   Examiners test whether you realize `&a + 1` advances by 20 bytes, and `ptr - 1` steps back by 4 bytes to `a[4]`.

2. **Pointer Subtraction Trap**:
   `p2 - p1` does **NOT** return the number of bytes between two pointers! It returns:
   $$\frac{\text{Address}(p2) - \text{Address}(p1)}{\text{sizeof}(T)}$$
   (i.e., the number of elements of type $T$ between them). Subtracting pointers of different types is illegal in C.

3. **String Literal Modification Trap (Segmentation Fault)**:
   ```c
   char *s = "GATE"; // Stored in Read-Only Data (Text/ROData segment)
   s[0] = 'K';       // CRASH! Bus error / Segmentation fault at runtime.
   char arr[] = "GATE"; // Stored in Stack (modifiable copy)
   arr[0] = 'K';     // OK! Modifies stack array to "KATE".
   ```

4. **Dangling Pointer Trap**:
   Returning the address of an automatic local stack variable from a function:
   ```c
   int* getVal() {
       int x = 42;
       return &x; // DANGER: Activation record popped on return!
   }
   ```
