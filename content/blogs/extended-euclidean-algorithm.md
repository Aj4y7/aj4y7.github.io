+++
date = '2025-12-19T12:00:00+00:00'
draft = false
title = "Extended Euclidean Algorithm: Deriving Bezout's Coefficients"
tags = ["algorithms", "number-theory", "cpp"]
categories = ["article"]
summary = "Derivation and C++ implementation of the extended Euclidean algorithm and Bezout coefficients."
math = true
+++

# Extended Euclidean Algorithm

## Prerequisites

To understand this article, you should be familiar with:

- **Standard Euclidean Algorithm**: The basic algorithm for finding the Greatest Common Divisor (GCD) of two integers using the property that $\gcd(a, b) = \gcd(b, a \bmod b)$.
- **Basic Number Theory**: Understanding of divisibility, GCD, and modular arithmetic.
- **Recursion**: Comfort with recursive algorithms and how they unwind.

## Overview

The Standard Euclidean Algorithm is used to find the Greatest Common Divisor (GCD) of two numbers. However, for many applications in number theory and competitive programming (like finding modular inverses), we need to find the integer coefficients that satisfy **Bezout's Identity**.

## Bezout's Identity

Bezout's identity states that for any integers $a$ and $b$:

$$
\exists \space x,y \in Z \space such \space that \space ax + by = gcd(a,b)
$$

Our goal is to find the pair $(x, y)$. We can achieve this by extending the Euclidean algorithm.

## Derivation

### 1. The Base Case

In the standard Euclidean algorithm, the recursion stops when $b = 0$. At this point, we have:

$$
a = gcd(a,b) \space and \space b = 0
$$

Substituting these into the equation $ax + by = gcd(a,b)$, we get:

$$
a \cdot 1 + 0 \cdot 0 = gcd(a,b)
$$

Thus, for the base case, we can say that $x = 1$ and $y = 0$ is true.

### 2. The Recursive Step

Let us assume we found the coefficients $(x_{1}, y_{1})$ for the pair $(b, a \space mod \space b)$ in the recursive call. This satisfies the equation:

$$
b \cdot x_{1} + (a \space mod \space b) \cdot y_{1} = g
$$

We want to find the pair $(x,y)$ for the original inputs $(a,b)$ such that:

$$
a \cdot x + b \cdot y = g
$$

#### Proving the Modulo Identity

We can represent $a \space mod \space b$ using integer division. This follows directly from **Euclid's Division Lemma**:

**Euclid's Division Lemma** states that for any integers $a$ and $b$ (with $b > 0$), there exist unique integers $q$ (quotient) and $r$ (remainder) such that:

$$
a = b \cdot q + r, \quad \text{where} \quad 0 \leq r < b
$$

Here, $r = a \space mod \space b$ and $q = \lfloor \frac{a}{b} \rfloor$.

Rearranging the lemma to solve for $r$:

$$
r = a - b \cdot q
$$

Substituting $r = a \space mod \space b$ and $q = \lfloor \frac{a}{b} \rfloor$:

$$
a \space mod \space b = a - \lfloor \frac{a}{b} \rfloor \cdot b
$$

This identity allows us to substitute the modulo operation in our recursive equation.

Substituting this expression back into the coefficient equation of $(x_{1}, y_{1})$ gives:

$$
g = b \cdot x_{1} + (a \space mod \space b) \cdot y_{1}
$$

$$
g = b \cdot x_{1} + \left( a - \lfloor \frac{a}{b} \rfloor \cdot b \right) \cdot y_{1}
$$

Now, we rearrange the terms to group them by $a$ and $b$:

$$
g = b \cdot x_{1} + a \cdot y_{1} - b \cdot \lfloor \frac{a}{b} \rfloor \cdot y_{1}
$$

$$
\implies g = a \cdot y_{1} + b \cdot \left( x_{1} - y_{1} \cdot \lfloor \frac{a}{b} \rfloor \right)
$$

### 3. Comparing Coefficients

By comparing the rearranged equation with our target $a \cdot x + b \cdot y = g$, we get the recurrence relation:

$$
x = y_{1} \space and \space y = x_{1} - y_{1} \cdot \lfloor \frac{a}{b} \rfloor
$$

## Implementation

Below is the C++ implementation. We use pass-by-reference to update `x` and `y` as the recursion unwinds.

```cpp
int gcd(int a, int b, int& x, int& y) {
    // Base Case
    if (b == 0) {
        x = 1;
        y = 0;
        return a;
    }

    // Recursive Step: Solve for (b, a%b)
    int x1, y1;
    int d = gcd(b, a % b, x1, y1);

    // Update coefficients using the derived formulas
    x = y1;
    y = x1 - y1 * (a / b);

    return d;
}
```

## Applications

The Extended Euclidean Algorithm has numerous applications across computer science and mathematics:

- Modular Multiplicative Inverse
- Solving Linear Diophantine Equations
- Chinese Remainder Theorem
- Combinatorial Optimization and Integer Linear Programming
