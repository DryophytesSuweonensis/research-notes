# Reversible Embedding in Quantum Computing

## 1. Why Reversible Embedding Is Necessary

A classical function is generally written as

$$
f : X \rightarrow Y.
$$

For example,

$$
f(x)=x^2
$$

or

$$
f(a,b)=a\land b.
$$

However, many classical functions are **not reversible**.

For a reversible transformation, every output must uniquely determine its input.

Mathematically, the mapping must be one-to-one.

If two different inputs produce the same output,

$$
x_1 \neq x_2,
\qquad
f(x_1)=f(x_2),
$$

then the original input cannot be recovered from the output alone.

Such a transformation cannot directly be implemented as a unitary quantum operation.

---

## 2. Example: AND Gate

Consider the classical AND function:

$$
f(a,b)=a\land b.
$$

Its truth table is

| $a$ | $b$ | $a\land b$ |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Three different inputs produce the same output:

$$
(0,0),(0,1),(1,0)
\rightarrow
0.
$$

Therefore,

$$
(a,b)\rightarrow a\land b
$$

is many-to-one and is not reversible.

A unitary operator cannot implement this transformation directly.

---

## 3. The Basic Idea of Reversible Embedding

Instead of replacing the input with the output, quantum computation usually preserves the input and writes the function value into another register.

The standard reversible embedding is

$$
U_f |x\rangle |y\rangle = |x\rangle |y\oplus f(x)\rangle.
$$

Here,

- $|x\rangle$ is the input register,
- $|y\rangle$ is the target register,
- $f(x)$ is the classical function,
- $\oplus$ denotes bitwise XOR,
- $U_f$ is the reversible quantum implementation of $f$.

If the target register is initialized to zero,

$$
|y\rangle=|0\rangle,
$$

then

$$
U_f|x\rangle|0\rangle = |x\rangle|f(x)\rangle.
$$

Therefore, the function value is obtained without deleting the original input.

---

## 4. Why XOR Makes the Operation Reversible

The important part is

$$
y\rightarrow y\oplus f(x).
$$

XOR has the property

$$
a\oplus b\oplus b=a.
$$

Therefore, applying the same transformation twice gives

$$
U_f^2=I
$$

for this standard Boolean oracle construction.

For example,

$$
|x\rangle|y\rangle
\rightarrow
|x\rangle|y\oplus f(x)\rangle
$$

and applying the same operation again gives

$$
|x\rangle|(y\oplus f(x))\oplus f(x)\rangle = |x\rangle|y\rangle.
$$

Therefore,

$$
U_f^{-1}=U_f.
$$

The transformation is reversible.

---

## 5. Why We Cannot Simply Use

A common initial idea is to write

$$
|x\rangle
\rightarrow
|f(x)\rangle.
$$

This only works if $f$ itself is one-to-one.

Suppose

$$
f(0)=0,
\qquad
f(1)=0.
$$

Then we would require

$$
|0\rangle\rightarrow|0\rangle
$$

and

$$
|1\rangle\rightarrow|0\rangle.
$$

However, unitary transformations preserve inner products.

Initially,

$$
\langle 0|1\rangle=0.
$$

After the transformation, we would obtain

$$
\langle 0|0\rangle=1.
$$

This violates inner-product preservation.

Therefore, such a many-to-one operation cannot be unitary.

---

## 6. Inner-Product Preservation

For a unitary operator $U$,

$$
U^\dagger U=I.
$$

Therefore,

$$
\langle U\psi|U\phi\rangle = \langle\psi|\phi\rangle.
$$

This means that orthogonal quantum states must remain orthogonal after unitary evolution.

If

$$
|x_1\rangle
\quad\text{and}\quad
|x_2\rangle
$$

represent two distinct computational basis states, then

$$
\langle x_1|x_2\rangle=0.
$$

A many-to-one mapping would map both states to the same state:

$$
|x_1\rangle\rightarrow|f\rangle,
$$

$$
|x_2\rangle\rightarrow|f\rangle.
$$

Then

$$
\langle f|f\rangle=1,
$$

which contradicts unitary evolution.

This provides a more fundamental explanation of why reversible embedding is required.

---

## 7. Example: Reversible AND Using a Toffoli Gate

The classical AND operation can be embedded reversibly using a Toffoli gate.

The transformation is

$$
|a,b,c\rangle
\rightarrow
|a,b,c\oplus(a\land b)\rangle.
$$

If the target qubit is initialized as

$$
c=0,
$$

then

$$
|a,b,0\rangle
\rightarrow
|a,b,a\land b\rangle.
$$

The original input bits $a$ and $b$ are preserved.

This is the reversible version of the AND computation.

For example,

$$
|1,1,0\rangle
\rightarrow
|1,1,1\rangle
$$

while

$$
|1,0,0\rangle
\rightarrow
|1,0,0\rangle.
$$

Because the original inputs are retained, the entire transformation remains one-to-one.

---

## 8. Reversible Embedding of General Classical Functions

Suppose a classical function is

$$
f:\{0,1\}^n\rightarrow\{0,1\}^m.
$$

The standard reversible embedding is

$$
U_f:
|x\rangle|y\rangle
\rightarrow
|x\rangle|y\oplus f(x)\rangle.
$$

The input register requires $n$ qubits and the output register requires $m$ qubits.

Therefore, even if the original classical function is

$$
f(x),
$$

the quantum implementation generally acts on a larger state space:

$$
(x,y)
\rightarrow
(x,y\oplus f(x)).
$$

This is one of the first examples of how quantum implementations may require additional resources compared with the original classical description.

---

## 9. Intermediate Computation

In practice, $f(x)$ may be complicated.

Suppose

$$
f(x)=h(g(x)).
$$

A reversible circuit may first compute

$$
|x\rangle|0\rangle|0\rangle
\rightarrow
|x\rangle|g(x)\rangle|0\rangle,
$$

then

$$
|x\rangle|g(x)\rangle|0\rangle
\rightarrow
|x\rangle|g(x)\rangle|f(x)\rangle.
$$

At this point, the intermediate state $g(x)$ remains in the circuit.

The state is

$$
|x\rangle|g(x)\rangle|f(x)\rangle.
$$

If $g(x)$ is no longer needed, it becomes garbage.

This leads directly to the concept of **uncomputation**.

---

## 10. Compute-Copy-Uncompute Pattern

A common reversible-computation pattern is

$$
\text{Compute}
\rightarrow
\text{Copy}
\rightarrow
\text{Uncompute}.
$$

Suppose

$$
U_g:
|x\rangle|0\rangle
\rightarrow
|x\rangle|g(x)\rangle.
$$

After computing the desired output, a reversible copy operation can transfer the useful result to a clean register.

Then $U_g^\dagger$ is applied:

$$
|x\rangle|g(x)\rangle
\rightarrow
|x\rangle|0\rangle.
$$

The intermediate register returns to its initial state and can be reused.

This technique is fundamental in quantum oracle construction.

---

## 11. Reversible Embedding and Quantum Superposition

An important point is that the reversible circuit must work not only for one classical input but also for quantum superpositions.

Suppose

$$
|\psi\rangle = \sum_x \alpha_x |x\rangle.
$$

Then the oracle acts as

$$
U_f\left(\sum_x \alpha_x |x\rangle|0\rangle\right) = \sum_x\alpha_x|x\rangle|f(x)\rangle.
$$

Thus, the function is evaluated coherently over all basis states in the superposition.

However, this does **not** mean that all classical values $f(x)$ can be read out simultaneously.

Measurement still produces only limited classical information.

The computational advantage arises from interference and amplitude manipulation rather than from directly obtaining all function values.

---

## 12. Why Reversible Embedding Matters for Quantum Oracles

Many quantum algorithms assume an oracle

$$
U_f.
$$

For example, Grover search assumes that a predicate

$$
f(x)\in\{0,1\}
$$

can be evaluated coherently.

A common theoretical description is

$$
U_f|x\rangle|y\rangle = |x\rangle|y\oplus f(x)\rangle.
$$

However, this notation hides the cost of constructing the reversible circuit for $f$.

If $f(x)$ involves

- arithmetic,
- comparisons,
- sorting,
- matrix operations,
- lookup tables,
- optimization objectives,
- communication-channel calculations,

then each classical operation must be implemented in a reversible form.

Therefore,

$$
T_{\mathrm{oracle}}
$$

may include substantial costs from

- additional ancilla qubits,
- reversible arithmetic,
- Toffoli gates,
- uncomputation,
- data loading,
- and circuit depth.

---

## 13. Reversible Embedding and Quantum Speedup

Suppose a classical search requires

$$
O(N)
$$

evaluations of a function $f(x)$.

Grover search reduces the number of oracle calls to

$$
O(\sqrt{N}).
$$

The theoretical comparison is often written as

$$
O(N)
\quad\text{vs.}\quad
O(\sqrt{N}).
$$

However, a more realistic comparison is

$$
T_{\mathrm{classical}} = NT_f^{\mathrm{classical}}
$$

versus

$$
T_{\mathrm{quantum}}
\approx
\sqrt{N}
T_f^{\mathrm{reversible}}.
$$

The reversible implementation cost

$$
T_f^{\mathrm{reversible}}
$$

may be significantly larger than the classical cost

$$
T_f^{\mathrm{classical}}.
$$

Therefore, a query-complexity speedup does not automatically imply an end-to-end computational speedup.

---

## 14. Resource Cost of Reversible Embedding

A realistic implementation should consider at least the following quantities:

$$
N_{\mathrm{logical\ qubits}}
$$

$$
N_{\mathrm{ancilla}}
$$

$$
N_{\mathrm{Toffoli}}
$$

$$
N_T
$$

$$
D_T
$$

$$
D_{\mathrm{circuit}}.
$$

The exact resource cost depends strongly on how the classical function is synthesized into a reversible circuit.

Therefore, reversible circuit synthesis itself is an important research problem.

---

## 15. Important Distinction

Reversible embedding does not mean that the original classical function becomes mathematically reversible.

For example, the AND function

$$
f(a,b)=a\land b
$$

remains irreversible.

Instead, the function is placed inside a larger reversible transformation:

$$
(a,b,c)
\rightarrow
(a,b,c\oplus(a\land b)).
$$

Therefore,

$$
\boxed{
\text{Irreversible function}
\rightarrow
\text{reversible embedding in a larger state space}
}
$$

is the correct interpretation.

---

## 16. Key Takeaway

The central idea of reversible embedding is

$$
\boxed{
|x\rangle
\rightarrow
|f(x)\rangle
\quad\text{is generally impossible}
}
$$

but

$$
\boxed{
|x\rangle|y\rangle
\rightarrow
|x\rangle|y\oplus f(x)\rangle
}
$$

can be implemented reversibly.

The price of this transformation is that the quantum circuit must preserve information that a classical computation would normally discard.

This leads to additional requirements such as

- extra registers,
- ancilla qubits,
- garbage management,
- uncomputation,
- larger circuit depth,
- and higher fault-tolerant resource cost.

For this reason, reversible embedding is not merely a mathematical detail.

It is one of the central implementation constraints that determines whether a theoretical quantum algorithm can be transformed into a practical quantum circuit.
