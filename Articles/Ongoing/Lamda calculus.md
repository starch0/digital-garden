# Lamda Calculus introduction.

Lambda calculus, developed by Alonzo Church in the 1930s, is a formal system in mathematical logic and computer science for expressing computation based on function abstraction and application. Despite its minimalism, lambda calculus is Turing complete and forms the theoretical foundation for functional programming languages and much of modern computation theory.

---

## 1. Syntax of Lambda Calculus

The lambda calculus is built from three syntactic forms:

- **Variables:** `x`, `y`, `z`, ...
- **Abstractions:** `λx.M` (function definition; `x` is the parameter, `M` is the body)
- **Applications:** `M N` (function application; apply function `M` to argument `N`)

**Formal Grammar:**

```
M ::= x            (variable)
    | λx.M         (abstraction)
    | M N          (application)
```

Parentheses are used for clarity: `λx.(λy.(x y))` vs. `λx.λy.x y`.

---

## 2. Semantics

**Variables** are placeholders for values or functions.

**Abstraction** (`λx.M`) defines an anonymous function with parameter `x` and body `M`. For example, `λx.x` is the identity function.

**Application** (`M N`) means the function `M` is applied to argument `N`.

---

## 3. Free and Bound Variables

A variable is **bound** if it appears as the parameter of a λ-abstraction. Otherwise, it is **free**.

- In `λx.x`, `x` is bound.
- In `λx.y`, `y` is free.

The set of free variables in `M` is denoted `FV(M)`.

---

## 4. Alpha Conversion (Renaming)

Variables bound by λ-abstraction can be renamed:

```
λx.x ≡ λy.y
```

This is called **alpha-conversion**. It avoids variable name collisions during substitution.

---

## 5. Beta Reduction (Function Application)

**Beta reduction** is the core computation rule in lambda calculus.

```
(λx.M) N → M[x := N]
```

This means: Replace all free occurrences of `x` in `M` with `N`.

**Example:**
```
(λx.x) y → y
```

**Example with Nested Abstraction:**
```
(λx.λy.x) a b
= (λy.a) b    // Apply first lambda to a
= a           // Apply second lambda to b, but body is just a
```

---

## 6. Eta Conversion (Extensionality)

Eta-conversion expresses that two functions are equivalent if they produce the same result for all arguments.

```
λx.(f x) ≡ f    if x is not free in f
```

---

## 7. Reduction Strategies

- **Normal Order:** Always reduce the leftmost, outermost redex first. (Guaranteed to find normal form if it exists.)
- **Applicative Order:** Always reduce the leftmost, innermost redex first. (Used by most programming languages.)

---

## 8. Encoding Data and Operators

### Booleans

```
TRUE  ≡ λx.λy.x
FALSE ≡ λx.λy.y

AND ≡ λp.λq.p q p
OR  ≡ λp.λq.p p q
NOT ≡ λb.b FALSE TRUE
```

### Numbers (Church Numerals)

```
0 ≡ λf.λx.x
1 ≡ λf.λx.f x
2 ≡ λf.λx.f (f x)
n ≡ λf.λx.f^n x
```

**Successor Function:**
```
SUCC ≡ λn.λf.λx.f (n f x)
```

**Addition:**
```
ADD ≡ λm.λn.λf.λx.m f (n f x)
```

### Pairs

```
PAIR ≡ λx.λy.λf.f x y
FIRST ≡ λp.p (λx.λy.x)
SECOND ≡ λp.p (λx.λy.y)
```

---

## 9. Recursion

Lambda calculus does not have named functions or recursion directly. Recursion is encoded via fixed-point combinators such as the **Y combinator**:

```
Y ≡ λf.(λx.f (x x)) (λx.f (x x))
```

Using `Y`, we can define recursive functions.

---

## 10. Example: Factorial

Encoding factorial using the Y combinator:

```
FACT ≡ Y (λf.λn. ISZERO n 1 (MULT n (f (PRED n))))
```
Where `ISZERO`, `MULT`, and `PRED` are lambda encodings of zero test, multiplication, and predecessor.

---

## 11. Lambda Calculus and Computation

Lambda calculus is Turing complete—any computable function can be represented.

- **Untyped Lambda Calculus**: As described above; allows self-application and leads to paradoxes (like Russell’s paradox), but is powerful.
- **Typed Lambda Calculus**: Adds a type system to prevent certain forms of invalid computation.

---

## 12. Practical Impact

- Forms the basis of functional programming languages (e.g., Haskell, Lisp, Scheme, ML).
- Underlies the theory of programming languages and type systems.
- Used in proof assistants and type-theoretic foundations.

---

## 13. References

- Alonzo Church, "An Unsolvable Problem of Elementary Number Theory," American Journal of Mathematics, 1936.
- Barendregt, H. P., "The Lambda Calculus: Its Syntax and Semantics," 1984.
- Benjamin C. Pierce, "Types and Programming Languages," MIT Press, 2002.

---

**Summary:**  
Lambda calculus, with its minimal syntax and powerful expressive capacity, provides the fundamental building blocks for reasoning about computation and programming language design. Understanding its low-level rules and encodings is essential for theoretical computer science and the implementation of modern functional languages.