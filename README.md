# 🧩 Discrete Mathematics: Chapter 2 Proof Techniques (Part A)

Notes that **teach** how to write mathematical proofs from the ground up. This part covers the two techniques in the lecture: **direct proof** and **proof by contradiction**. Proof by induction is listed in the chapter topics but belongs to the next part of the lecture, so section 8 gives a short preview of it.

## 📑 Table of Contents

0. [Start Here: What Is a Proof?](#-0-start-here-what-is-a-proof)
1. [The Vocabulary You Must Know](#-1-the-vocabulary-you-must-know)
2. [Direct Proof](#-2-direct-proof)
3. [Direct Proof: Worked Examples](#-3-direct-proof-worked-examples)
4. [Proof by Contradiction](#-4-proof-by-contradiction)
5. [Proof by Contradiction: Worked Examples](#-5-proof-by-contradiction-worked-examples)
6. [Which Technique Should I Use?](#-6-which-technique-should-i-use)
7. [Common Mistakes](#-7-common-mistakes)
8. [Preview: Proof by Induction](#-8-preview-proof-by-induction)
9. [Practice Questions](#-9-practice-questions)
10. [Cheat Sheet and Exam Checklist](#-10-cheat-sheet-and-exam-checklist)

---

## 🧭 0. Start Here: What Is a Proof?

A **proof** is a chain of logical steps that shows a statement is **always true**, using only **definitions** and facts that are already accepted.

### Why examples are not proofs

Try the statement: *"The sum of any two even integers is even."*

```
2 + 4 = 6        even ✓
10 + 12 = 22     even ✓
100 + 38 = 138   even ✓
```

It works every time we try. But we cannot try every pair of even integers, because there are infinitely many. A proof covers **all of them at once**.

| Idea | What it does |
|---|---|
| **Examples** | Can **disprove** a claim (one counterexample is enough) but can **never prove** it |
| **Proof** | Shows the claim is true for **every** case, by logic and algebra |

You can use Python to hunt for a counterexample, but passing tests is **not** a proof:

```python
# searching for a counterexample to: "the sum of two even integers is even"
for x in range(-100, 101, 2):
    for y in range(-100, 101, 2):
        if (x + y) % 2 != 0:
            print("counterexample:", x, y)
print("no counterexample found (this still does not prove it)")
```

### Link to Chapter 1: most claims are "if … then …"

Most statements to prove are implications, `P → Q`:

- **P** is the **hypothesis**: what you are **given**.
- **Q** is the **conclusion**: what you must **show**.

Some claims hide the "if". Always rewrite them so you can see `P` and `Q`:

| Claim as written | Rewritten as "if … then …" |
|---|---|
| The sum of any two even integers is even. | **If** `x` and `y` are even integers, **then** `x + y` is even. |
| The sum of any two consecutive integers is odd. | **If** `a` and `a + 1` are integers, **then** `a + (a + 1)` is odd. |
| The square of an even integer is even. | **If** `n` is an even integer, **then** `n²` is even. |

### The two techniques in one line each

| Technique | The idea |
|---|---|
| **Direct proof** | **Assume** `P` is true, then walk step by step until you **reach** `Q`. |
| **Proof by contradiction** | **Pretend** the claim is false, walk until you hit something **impossible**, so the pretend was wrong and the claim is true. |

---

## 📖 1. The Vocabulary You Must Know

Almost every proof in this chapter works the same way: **replace a word by its algebra definition**. If you know the definitions below, most proofs write themselves.

| Word | Plain meaning | Algebra form (what you write) |
|---|---|---|
| **Integer** | A whole number: `…, −3, −2, −1, 0, 1, 2, 3, …` (no fractions) | `a`, `b`, `k`, `m`, `n` "where `a` is an integer" |
| **Even** | Divisible by 2 | `n = 2a` for some integer `a` |
| **Odd** | Not even, one more than an even number | `n = 2a + 1` for some integer `a` |
| **Divisible by d** | `d` goes in exactly, with no remainder | `n = d · a` for some integer `a` |
| **Consecutive integers** | Next to each other | `a` and `a + 1` |
| **Rational number** | A fraction of two integers | `r = a/b` where `a`, `b` are integers and **`b ≠ 0`** |
| **Prime** | An integer greater than 1 whose only positive divisors are 1 and itself | (29 is prime, 15 is not since `15 = 3 × 5`) |
| **Composite** | An integer greater than 1 that is **not** prime | `n = a × b` with `1 < a < n` and `1 < b < n` |

### 🔑 The closure facts (why "let m = a + b" is allowed)

When you add, subtract or multiply **integers**, you always get an **integer**:

- `a + b` is an integer
- `a − b` is an integer
- `a × b` is an integer

This is why you can write *"Let `m = a + b`, where `m` is an integer"*. It is a real fact, and the proof **needs** it.

### 🔑 How to turn a sentence into algebra

| The sentence says… | You write… |
|---|---|
| "Let `x` be an even integer" | `x = 2a`, where `a` is an integer |
| "Let `y` be an odd integer" | `y = 2b + 1`, where `b` is an integer |
| "`n` is divisible by 4" | `n = 4a`, where `a` is an integer |
| "`r` is rational" | `r = a/b`, where `a` and `b` are integers and `b ≠ 0` |
| "Two consecutive integers" | `a` and `a + 1` |

> ⚠️ **Use a different letter for each different number.** If `x` and `y` are two even integers, write `x = 2a` and `y = 2b`, **not** `y = 2a`. Using the same letter forces `x = y`, and then you have only proved the claim for equal numbers.

### 🎯 The finish line (know what you are aiming for)

Before you start, know what the **last line** must look like. Work towards it:

| To prove that something is… | The last line must look like |
|---|---|
| **even** | `2 × (an integer)` |
| **odd** | `2 × (an integer) + 1` |
| **divisible by d** | `d × (an integer)` |
| **rational** | `(an integer) / (a non-zero integer)` |

---

## 🧱 2. Direct Proof

**Direct proof** is one of the simplest methods of proving a proposition. You prove `P → Q` by **assuming `P` is true** and **logically deducing `Q`** from it.

```
Start: assume P  ──►  definitions + algebra  ──►  arrive at Q
```

### The 4 steps from the lecture

| Step | What to do |
|:-:|---|
| **1. State the assumptions** | Assume the premises or hypotheses are true (let the numbers be …). |
| **2. Use definitions** | Clearly define the key terms involved (even means `2a`, odd means `2a + 1`, …). |
| **3. Logical deductions** | Apply algebraic manipulation, properties or theorems to get towards the conclusion. |
| **4. Conclude** | After all the logical steps, state that the claim is proven. |

### ✍️ A template you can copy

```
Proposition: If P, then Q.

Proof:
Step 1 (Assume):     Let ... (this is P).
Step 2 (Definition): Since ..., we can write ..., where ... are integers.
Step 3 (Algebra):    (do the algebra)
                     = ...
                     Let m = ..., where m is an integer.
                     Therefore, ... = (the finish-line form).
Step 4 (Conclude):   Since ... satisfies the definition of ..., Q is true.
                     Hence, the proposition is proven.
```

---

## 🔨 3. Direct Proof: Worked Examples

Each example follows the template. The notes explain **why** each step is there.

### Example 1: The sum of any two even integers is even

**Rewritten:** if `x` and `y` are even integers, then `x + y` is even.

1. **Assume:** Let `x` and `y` be two even integers.
2. **Definition:** Since they are even, `x = 2a` and `y = 2b`, where `a` and `b` are integers.
3. **Algebra:**
   ```
   x + y = 2a + 2b
         = 2(a + b)          (factor out the 2)
   ```
   Let `m = a + b`, where `m` is an integer (closure). Therefore `x + y = 2m`.
4. **Conclude:** Since `2m` is divisible by 2, it satisfies the definition of an even integer. Hence the sum of any two even integers is even. **Proven.**

> 💡 **Why it works:** the goal is `2 × (an integer)`. Factoring out the common `2` produces exactly that form. Be careful with the last line: the conclusion is about two **even** integers (not "two integers").

### Example 2: If `n` is an even integer, then `n²` is even

1. **Assume:** Let `n` be an even integer.
2. **Definition:** `n = 2a`, where `a` is an integer.
3. **Algebra:**
   ```
   n² = (2a)²
      = 4a²
      = 2(2a²)
   ```
   Let `m = 2a²`, where `m` is an integer. Therefore `n² = 2m`.
4. **Conclude:** Since `2m` is divisible by 2, `n²` satisfies the definition of an even integer. Hence `n²` is even. **Proven.**

> 💡 **Trick:** squaring `2a` gives `4a²`. To reach the finish line `2 × (integer)`, split `4a²` as `2 × 2a²`.

### Example 3: If a number is divisible by 4, then it is even

1. **Assume:** Let `n` be an integer divisible by 4.
2. **Definition:** `n = 4a`, where `a` is an integer.
3. **Algebra:**
   ```
   n = 4a
     = 2(2a)
   ```
   Let `m = 2a`, where `m` is an integer. Therefore `n = 2m`.
4. **Conclude:** Since `n = 2m` with `m` an integer, `n` is even. **Proven.**

> 💡 **Idea:** 4 is `2 × 2`, so anything divisible by 4 is automatically divisible by 2.

### Example 4: The sum of any two consecutive integers is odd

1. **Assume:** Let the two consecutive integers be `a` and `a + 1`.
2. **Sum:**
   ```
   sum = a + (a + 1) = 2a + 1
   ```
3. **Definition check:** Since `a` is an integer, `2a` is an even integer. Therefore `2a + 1` is an odd integer.
4. **Conclude:** Hence the sum of any two consecutive integers is odd. **Proven.**

> 💡 **Why `a` and `a + 1`?** "Consecutive" means "next to each other", so one is always one more than the other. This time the finish line is `2 × (integer) + 1`, and `2a + 1` is exactly that.

### Example 5: If `x` and `y` are odd, then `x + y` is even

1. **Assume:** Let `x` and `y` be odd integers.
2. **Definition:** `x = 2a + 1` and `y = 2b + 1`, where `a` and `b` are integers.
3. **Algebra:**
   ```
   x + y = (2a + 1) + (2b + 1)
         = 2a + 2b + 2
         = 2(a + b + 1)
   ```
   Let `k = a + b + 1`, where `k` is an integer. Therefore `x + y = 2k`.
4. **Conclude:** Since `2k` is divisible by 2, `x + y` is even. **Proven.**

> 💡 **Trick:** the two `+ 1`s combine into `+ 2`, which is itself a multiple of 2, so the whole thing factors.

### Example 6: The sum of any two rational numbers is rational

1. **Assume:** Let `r` and `s` be rational numbers.
2. **Definition:** `r = a/b` and `s = c/d`, where `a`, `b`, `c`, `d` are integers and `b ≠ 0`, `d ≠ 0`.
3. **Add them** (common denominator, like adding fractions):
   ```
   r + s = a/b + c/d
         = (ad + bc) / (bd)
   ```
   Let `k = ad + bc` and `m = bd`, where `k` and `m` are integers (closure). So `r + s = k/m`.
4. **Conclude:** `k/m` is a rational number. Hence the sum of any two rational numbers is rational. **Proven.**

> 💡 **Do not forget:** a rational number needs a **non-zero** denominator. Here `m = bd` is not zero because `b ≠ 0` and `d ≠ 0`, so the product of two non-zero numbers is non-zero. Say this in the proof.

### ➕ More direct proofs for practice (not in the lecture)

**Even + odd is odd.** Let `x = 2a` and `y = 2b + 1`. Then `x + y = 2a + 2b + 1 = 2(a + b) + 1`, which is odd.

**Odd × odd is odd.** Let `x = 2a + 1` and `y = 2b + 1`. Then `xy = 4ab + 2a + 2b + 1 = 2(2ab + a + b) + 1`, which is odd.

**If `n` is odd, then `n²` is odd.** Let `n = 2k + 1`. Then `n² = 4k² + 4k + 1 = 2(2k² + 2k) + 1`, which is odd.

**If `n` is divisible by 6, then `n` is divisible by 3.** Let `n = 6a`. Then `n = 3(2a)` with `2a` an integer, so `n` is divisible by 3. (This is the same fact used in the Chapter 1 inference example about 870,232.)

---

## 🔄 4. Proof by Contradiction

**Proof by contradiction** assumes the **opposite** (the negation) of the statement you want to prove, then shows that this assumption leads to a **contradiction** or impossibility. That contradiction means the original statement **must be true**.

### 🧠 The idea in plain words

> "Suppose it is **not** true. Then something impossible happens. So it **must** be true."

Think of a detective: *"Suppose the suspect was at home. But the camera shows him in another city. Impossible. So he was not at home."*

### Why it is valid (link to Chapter 1)

A **contradiction** is a statement that is **always false** (like `p ∧ ¬p`). If assuming "the claim is false" forces a contradiction, then "the claim is false" cannot be true. So **the claim is true**.

### The 4 steps from the lecture

| Step | What to do |
|:-:|---|
| **1. Assume the opposite** | Assume the **negation** of the statement is true. |
| **2. Use definitions** | Clearly define the key terms involved. |
| **3. Logical deductions** | Use calculation or reasoning until you reach something **impossible** (a contradiction). |
| **4. Conclude** | The assumption is false, therefore the original statement is true. |

### What counts as a contradiction?

A contradiction is something that **cannot be true**. Typical forms:

| Type | Example |
|---|---|
| It **contradicts the given information** | We were told `n²` is even, but we calculated that `n²` is odd |
| It **contradicts a known fact** | "`N` is the greatest integer" but `N + 1 > N` |
| It says something is **both true and false** | `x + y` is both odd and even |

> 🎓 **Remember (from the lecture):** the key of proof by contradiction is that our **assumption must lead to a contradiction with the given information**. And when you find the contradiction, **reject the assumption, not the calculation.** The calculation was correct. The assumption was the only thing that was made up.

### ✍️ How to write "the opposite" correctly

This is where Chapter 1's **negation rules** are used.

| The claim | The opposite to assume |
|---|---|
| There is no greatest integer | There **is** a greatest integer |
| 29 is a prime number | 29 is **not** a prime number |
| `n` is even (for an integer) | `n` is **odd** |
| The sum of two even integers is even | `x` and `y` are even, **but `x + y` is odd** |
| **If `P` then `Q`** | **`P` is true AND `Q` is false** (because `¬(P → Q) ≡ P ∧ ¬Q`) |

The last row is the most important. To prove `if n² is even, then n is even`, you assume **both** `n²` is even (the given) **and** `n` is not even, i.e. odd (the opposite of the conclusion).

### ✍️ A template you can copy

```
Proposition: ...

Proof (by contradiction):
Step 1 (Assume the opposite): Suppose, for contradiction, that ... (the negation).
Step 2 (Definition):          Then, by definition, we can write ...
Step 3 (Deduce):              (calculations)
                              This contradicts ... (the given information / a known fact).
Step 4 (Conclude):            Therefore, our assumption is false.
                              Hence, the original statement is true. Proven.
```

---

## 🔎 5. Proof by Contradiction: Worked Examples

### Example 1: There is no greatest integer

An integer is a whole number such as `…, −3, −2, −1, 0, 1, 2, 3, …`

A "greatest integer" would mean there is some integer that is bigger than **every** other integer. (If someone claimed 100 is the greatest, it would mean `100 ≥ every integer`.)

1. **Assume the opposite:** There **is** a greatest integer, call it `N`.
2. **Find a contradiction:** Since `N` is an integer, `N + 1` is also an integer (closure).
3. **Compare:** `N + 1 > N`, so there is an integer bigger than `N`.
4. **Conclude:** This **contradicts** the assumption that `N` is the greatest integer. Hence there is no greatest integer. **Proven.**

> 💡 The contradiction is: `N` is supposed to be bigger than every integer, yet `N + 1` is bigger than `N`.

### Example 2: 29 is a prime number

1. **Assume the opposite:** Suppose 29 is **not** prime.
2. **Definition:** Then 29 has a factor other than 1 and 29, so we can write `29 = a × b`, where `a` and `b` are integers **greater than 1**.
3. **Which factors are possible?** If both `a` and `b` were 6 or more, then `a × b ≥ 36`, which is more than 29. So at least one factor must be **small**: `a` is one of 2, 3, 4, 5. This is the same as saying we only need to check factors up to `√29 ≈ 5.38`. Now check:
   ```
   29 ÷ 2 = 14.5     not an integer
   29 ÷ 3 = 9.67     not an integer
   29 ÷ 4 = 7.25     not an integer
   29 ÷ 5 = 5.8      not an integer
   ```
   None of these are factors of 29.
4. **Conclude:** This **contradicts** the assumption that 29 is not prime. Hence 29 is prime. **Proven.**

> 💡 **Why only check up to `√29`?** If a number has a factor pair `a × b`, one of the two factors is always at most the square root. So if no small factor works, none does. (Checking 4 is not really needed, since if 4 divided 29, then 2 would too.)

### Example 3: If `n²` is even, then `n` is even

This is a good example of **why** we need contradiction. Starting directly from `n² = 2k` leaves you stuck, because you cannot easily "undo" the square. Assuming `n` is odd gives you something easy to calculate with.

1. **Assume the opposite:** We are given that `n²` is even, and we assume `n` is **odd**.
2. **Definition:** Since `n` is odd, `n = 2k + 1`, where `k` is an integer.
3. **Calculate:**
   ```
   n² = (2k + 1)²
      = 4k² + 4k + 1
      = 2(2k² + 2k) + 1
   ```
   Since `2k² + 2k` is an integer, `n²` is **odd**.
4. **Contradiction:** We are **given** that `n²` is even, but the calculation shows `n²` is odd. It cannot be both. Therefore our assumption that `n` is odd is false.
   **Hence `n` is an even integer. Proven.**

> 🎓 **From the lecture:** *we reject our assumption that `n` is odd, not the calculation.* The algebra is fine. It is the made-up assumption that is wrong.

### Example 4: The sum of any two even integers is even

(We proved this directly in section 3. Here is the same claim by contradiction, to show how the method works.)

1. **Assume the opposite:** Let `x` and `y` be two even integers, but assume `x + y` is **odd**.
2. **Definition:** Since `x` and `y` are even, `x = 2a` and `y = 2b`, where `a` and `b` are integers.
3. **Sum:**
   ```
   x + y = 2a + 2b
         = 2(a + b)
   ```
   Since `a + b` is an integer, `x + y` is **even**.
4. **Conclude:** But we assumed `x + y` is odd. It cannot be both odd and even. So our assumption that `x + y` is odd must be false. Hence the sum of any two even integers is even. **Proven.**

> 💡 For this claim a direct proof is shorter. The lecture uses it to show how the contradiction pattern works.

### ➕ More contradiction proofs for practice (not in the lecture)

**There is no greatest even integer.** Suppose `N` is the greatest even integer. Then `N + 2` is also even, and `N + 2 > N`. This contradicts `N` being the greatest. Hence there is no greatest even integer.

**If `3n + 2` is odd, then `n` is odd.** Assume `3n + 2` is odd **and** `n` is even (the opposite of the conclusion). Then `n = 2k`, so `3n + 2 = 6k + 2 = 2(3k + 1)`, which is **even**. This contradicts `3n + 2` being odd. Hence `n` is odd.

---

## 🧭 6. Which Technique Should I Use?

| Try **direct proof** when… | Try **proof by contradiction** when… |
|---|---|
| You can start from the hypothesis and calculate forward | The claim is a **negative**: "there is **no** …", "is **not** …" |
| The claim is about sums, products or squares being even, odd or rational | Going forward from the hypothesis leaves you **stuck** |
| The definitions turn straight into algebra | Assuming the opposite gives you something **concrete to calculate with** |
| Example: *odd + odd is even* | Example: *there is no greatest integer*, *29 is prime*, *if `n²` is even then `n` is even* |

**Practical advice:**

1. **Try direct first.** It is usually shorter and clearer.
2. If you get stuck, **assume the opposite of the conclusion** and see whether something impossible appears.

**Why Example 3 (`n²` even ⇒ `n` even) needs contradiction:** directly, you know `n² = 2k`, but there is no clean algebra step to get `n = 2 × something`. If you assume `n` is odd, you can write `n = 2k + 1` and calculate easily.

---

## 🚧 7. Common Mistakes

| Mistake | Why it is wrong | Fix |
|---|---|---|
| Proving by **examples** ("2 + 4 = 6, so the sum of evens is even") | Examples never prove a general claim | Use letters and algebra |
| Using the **same letter** for two numbers (`x = 2a`, `y = 2a`) | Forces `x = y`, so only a special case is proved | `x = 2a`, `y = 2b` |
| Forgetting to say the new letter is an **integer** | The proof depends on closure | Write "where `m` is an integer" |
| Not reaching the **finish-line form** | "x + y = 2a + 2b" is not yet "even" | Factor to `2(a + b)` and name `m` |
| Rational proof without **`b ≠ 0`, `d ≠ 0`** | A fraction with denominator 0 is not rational | State `bd ≠ 0` |
| Writing the wrong opposite in a contradiction proof | You prove a different claim | `¬(P → Q) ≡ P ∧ ¬Q` |
| **Rejecting the calculation** instead of the assumption | The calculation was correct | Say "our assumption is false" |
| Using the **conclusion as a step** ("circular") | Assuming what you want to prove | Only assume the hypothesis (or the opposite, in contradiction) |
| Ending without stating the claim | The reader cannot see you finished | Finish with "Hence …, proven" |

---

## 🔮 8. Preview: Proof by Induction

The chapter lists **proof by induction** as its third technique. It is not in this part of the lecture, but here is the idea so the next part feels familiar.

Induction proves a statement `P(n)` for **all whole numbers** `n ≥ 1`, like knocking down an endless line of dominoes:

| Step | Meaning |
|---|---|
| **1. Base case** | Show `P(1)` is true (the first domino falls). |
| **2. Inductive hypothesis** | **Assume** `P(k)` is true for some number `k`. |
| **3. Inductive step** | Using that, **prove** `P(k + 1)` is true (each domino knocks down the next). |
| **4. Conclude** | So `P(n)` is true for all `n ≥ 1`. |

**Example:** prove `1 + 2 + 3 + … + n = n(n + 1)/2`.

- **Base case** (`n = 1`): left side `= 1`, right side `= 1 × 2 / 2 = 1`. ✓
- **Assume** it is true for `k`: `1 + 2 + … + k = k(k + 1)/2`.
- **Step:** add `k + 1` to both sides:
  ```
  1 + 2 + … + k + (k + 1) = k(k + 1)/2 + (k + 1)
                          = (k + 1)(k/2 + 1)
                          = (k + 1)(k + 2)/2
  ```
  This is the formula with `n = k + 1`. ✓
- **Conclude:** the formula is true for all `n ≥ 1`.

---

## 📝 9. Practice Questions

Try each one **before** opening the answer. Write the full 4-step proof, as you would in a test.

**Q1.** Write the **opposite** you would assume in a proof by contradiction:
(a) "The sum of two odd integers is even."
(b) "There is no greatest even integer."
(c) "If `n²` is odd, then `n` is odd."

<details>
<summary>Answer</summary>

- (a) Let `x` and `y` be odd integers, but **assume `x + y` is odd**.
- (b) **There is** a greatest even integer.
- (c) Assume `n²` is odd **and `n` is even** (`¬(P → Q) ≡ P ∧ ¬Q`).

</details>

**Q2.** Direct proof: *If `n` is odd, then `n + 1` is even.*

<details>
<summary>Answer</summary>

1. Let `n` be an odd integer.
2. Then `n = 2k + 1`, where `k` is an integer.
3. `n + 1 = (2k + 1) + 1 = 2k + 2 = 2(k + 1)`. Let `m = k + 1`, an integer. So `n + 1 = 2m`.
4. Since `2m` is divisible by 2, `n + 1` is even. **Proven.**

</details>

**Q3.** Direct proof: *The product of an even integer and any integer is even.*

<details>
<summary>Answer</summary>

1. Let `x` be an even integer and `y` any integer.
2. `x = 2a`, where `a` is an integer.
3. `xy = (2a)y = 2(ay)`. Let `m = ay`, an integer (closure). So `xy = 2m`.
4. `xy` is even. **Proven.**

</details>

**Q4.** Direct proof: *If `n` is divisible by 9, then `n` is divisible by 3.*

<details>
<summary>Answer</summary>

1. Let `n` be an integer divisible by 9.
2. `n = 9a`, where `a` is an integer.
3. `n = 9a = 3(3a)`. Let `m = 3a`, an integer. So `n = 3m`.
4. `n` is divisible by 3. **Proven.**

</details>

**Q5.** Direct proof: *The sum of three consecutive integers is divisible by 3.*

<details>
<summary>Answer</summary>

1. Let the integers be `a`, `a + 1`, `a + 2`.
2. Sum `= a + (a + 1) + (a + 2) = 3a + 3`.
3. `3a + 3 = 3(a + 1)`. Let `m = a + 1`, an integer. So the sum `= 3m`.
4. The sum is divisible by 3. **Proven.**

</details>

**Q6.** Direct proof: *The difference of two odd integers is even.*

<details>
<summary>Answer</summary>

1. Let `x` and `y` be odd integers.
2. `x = 2a + 1` and `y = 2b + 1`, where `a`, `b` are integers.
3. `x − y = (2a + 1) − (2b + 1) = 2a − 2b = 2(a − b)`. Let `m = a − b`, an integer. So `x − y = 2m`.
4. `x − y` is even. **Proven.**

</details>

**Q7.** Direct proof: *The product of two rational numbers is rational.*

<details>
<summary>Answer</summary>

1. Let `r` and `s` be rational numbers.
2. `r = a/b` and `s = c/d`, where `a`, `b`, `c`, `d` are integers and `b ≠ 0`, `d ≠ 0`.
3. `rs = (a/b)(c/d) = ac/(bd)`. Let `k = ac` and `m = bd`, integers, with `m ≠ 0` because `b ≠ 0` and `d ≠ 0`. So `rs = k/m`.
4. `k/m` is rational. **Proven.**

</details>

**Q8.** Contradiction: *If `n²` is odd, then `n` is odd.*

<details>
<summary>Answer</summary>

1. We are given `n²` is odd, and **assume `n` is even**.
2. Then `n = 2k`, where `k` is an integer.
3. `n² = (2k)² = 4k² = 2(2k²)`, and `2k²` is an integer, so `n²` is **even**.
4. This contradicts the given fact that `n²` is odd. So our assumption is false. **Hence `n` is odd. Proven.**

</details>

**Q9.** Contradiction: *There is no greatest even integer.*

<details>
<summary>Answer</summary>

1. **Assume** there is a greatest even integer, `N`.
2. Since `N` is even, `N = 2a`, and `N + 2 = 2a + 2 = 2(a + 1)` is also even.
3. But `N + 2 > N`.
4. This contradicts `N` being the greatest even integer. **Hence there is no greatest even integer. Proven.**

</details>

**Q10.** Contradiction: *31 is a prime number.*

<details>
<summary>Answer</summary>

1. **Assume** 31 is **not** prime.
2. Then `31 = a × b` with integers `a`, `b` greater than 1.
3. If both were 6 or more, `a × b ≥ 36 > 31`. So one factor is at most 5 (`√31 ≈ 5.57`). Check:
   `31 ÷ 2 = 15.5`, `31 ÷ 3 = 10.33`, `31 ÷ 4 = 7.75`, `31 ÷ 5 = 6.2`. None are integers.
4. This contradicts the assumption. **Hence 31 is prime. Proven.**

</details>

**Q11.** Contradiction: *If `3n + 2` is odd, then `n` is odd.*

<details>
<summary>Answer</summary>

1. We are given `3n + 2` is odd, and **assume `n` is even**.
2. Then `n = 2k`, where `k` is an integer.
3. `3n + 2 = 3(2k) + 2 = 6k + 2 = 2(3k + 1)`, and `3k + 1` is an integer, so `3n + 2` is **even**.
4. This contradicts the given fact that `3n + 2` is odd. So our assumption is false. **Hence `n` is odd. Proven.**

</details>

**Q12.** Which technique would you try first, and why?
(a) The sum of two even integers is even.
(b) There is no greatest integer.
(c) If `n²` is even, then `n` is even.
(d) If `n` is odd, then `n²` is odd.

<details>
<summary>Answer</summary>

- (a) **Direct.** Write `x = 2a`, `y = 2b` and add.
- (b) **Contradiction.** It is a "there is no …" claim.
- (c) **Contradiction.** Going forward from `n²` is hard, but assuming `n` is odd is easy to calculate with.
- (d) **Direct.** Write `n = 2k + 1` and square it.

</details>

**Q13.** (Chapter 1 link) Write the negation of *"If `n²` is even, then `n` is even"* in symbols and in words.

<details>
<summary>Answer</summary>

Let `p`: `n²` is even, `q`: `n` is even. The claim is `p → q`, and `¬(p → q) ≡ p ∧ ¬q`.

In words: **`n²` is even and `n` is not even (odd).** This is exactly the assumption made at the start of the contradiction proof.

</details>

**Q14.** Find the error in each "proof".

(a) *Claim: the sum of two even integers is even. "Let `x = 2a` and `y = 2a`. Then `x + y = 4a = 2(2a)`, which is even."*
(b) *Claim: the sum of two even integers is even. "2 + 4 = 6, 8 + 10 = 18, 20 + 6 = 26. All even. Proven."*
(c) *Claim: the sum of two rational numbers is rational. "`a/b + c/d = (ad + bc)/(bd)`, so it is a fraction of integers. Proven."*
(d) *Claim: if `n²` is even then `n` is even. "Assume `n` is odd. Then `n² = 4k² + 4k + 1`, which is odd. This contradicts the given fact, so my algebra must be wrong."*

<details>
<summary>Answer</summary>

- (a) **Same letter** for two numbers. It forces `x = y`. Use `y = 2b`.
- (b) **Examples are not a proof.** They only test a few cases.
- (c) It never says that `bd ≠ 0`. A rational number needs a non-zero denominator, which follows from `b ≠ 0` and `d ≠ 0`.
- (d) It **rejects the calculation**. The correct statement is: the contradiction shows **the assumption** (`n` is odd) is false.

</details>

---

## 📋 10. Cheat Sheet and Exam Checklist

### Definitions to memorise

| Word | Algebra |
|---|---|
| Even | `n = 2a` |
| Odd | `n = 2a + 1` |
| Divisible by d | `n = d · a` |
| Consecutive | `a`, `a + 1` |
| Rational | `a/b`, with `b ≠ 0` |
| Prime | `> 1`, only divisors are 1 and itself |

Integers are **closed** under `+`, `−`, `×`: the result is an integer.

### Direct proof (assume P, reach Q)

1. **Assume** the hypothesis (let the numbers be …).
2. **Definitions:** turn words into algebra (different letter for each number).
3. **Algebra:** simplify and factor towards the finish-line form, and say the new letter is an integer.
4. **Conclude:** state the claim and write "Proven".

### Contradiction (assume the opposite, reach nonsense)

1. **Assume the opposite** (for `P → Q`, assume `P` **and** `¬Q`).
2. **Definitions:** turn words into algebra.
3. **Deduce** until it **contradicts** the given information or a known fact.
4. **Conclude:** the assumption is false, so the original statement is true. (Reject the assumption, **not** the calculation.)

### The finish lines

| To show it is… | End with |
|---|---|
| even | `2 × (integer)` |
| odd | `2 × (integer) + 1` |
| divisible by d | `d × (integer)` |
| rational | `(integer) / (non-zero integer)` |

### Exam checklist

- ☐ Rewrite the claim as "if … then …" so you can see what you are given and what to show.
- ☐ Use a **different letter** for every different number.
- ☐ Write "where … is an integer" every time you introduce a letter.
- ☐ Reach the **finish-line form** before concluding.
- ☐ Rational proofs: say the denominator is **not zero**.
- ☐ Contradiction: write the **opposite correctly**, name the **contradiction**, then say the **assumption is false**.
- ☐ End with the original claim and "Proven".

---

<p align="center">⭐ Notes in progress. Part B (including induction) to be added. ⭐</p>
