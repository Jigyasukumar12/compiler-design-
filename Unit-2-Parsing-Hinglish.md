# 📗 UNIT 2 — Basic Parsing Techniques (Hinglish Notes)
### Topics: Parsers, Shift-Reduce, Top-Down, LR, SLR, CLR, LALR

---

## 🎯 Unit Ka Goal

> Is unit mein **Parsing** seekhenge — tokens leke parse tree kaise banate hain.
> **Yeh unit sabse zyada numericals wali hai** — tables banana aata hona chahiye.

---

## 📋 Syllabus Breakdown

| Topic | Priority | PYQ Frequency |
|-------|----------|--------------|
| Parsers types (Top-Down, Bottom-Up) | ⭐⭐⭐ HIGH | Har saal 2 marks |
| Shift-Reduce Parsing | ⭐⭐⭐ HIGH | Har saal — numerical |
| Predictive Parser / LL(1) | ⭐⭐⭐ HIGH | Table construction numerical |
| LR(0) Items + SLR Table | ⭐⭐⭐ HIGH | Har saal Section C |
| Canonical LR / CLR / LR(1) | ⭐⭐⭐ HIGH | Section B mein |
| LALR Parsing Table | ⭐⭐⭐ HIGH | Har saal aata hai |
| Operator Precedence Parsing | ⭐⭐ MEDIUM | Kabhi kabhi |

---

# 📘 Topic 1: Parsers & Shift-Reduce Parsing

## 📌 Quick Keywords
> `Top-Down` · `Bottom-Up` · `Shift` · `Reduce` · `Stack` · `Parse Tree` · `Syntax Tree`

## 1️⃣ Parser Kya Hai?
> **Parser (Syntax Analyzer)** = Compiler ki 2nd phase jo token stream se **Parse Tree** banata hai aur grammar rules check karta hai.

```
Token Stream → [PARSER] → Parse Tree
```

## 2️⃣ Top-Down vs Bottom-Up Parsing
| Feature | Top-Down | Bottom-Up |
|---------|----------|-----------|
| Direction | Root se leaves | Leaves se root |
| Strategy | Expand non-terminals | Reduce to non-terminals |
| Method | LL parsers, Predictive | LR parsers, Shift-Reduce |
| Grammar | Left recursion problem | Handles more grammars |
| Stack | Stores expected symbols | Stores seen symbols |
| Examples | LL(1), Recursive Descent | SLR, CLR, LALR |

## 3️⃣ Parse Tree vs Syntax Tree (AST) 🔥
> **PYQ:** Differentiate Parse tree and Syntax tree — Har saal 2 marks mein aata hai!

| Feature | Parse Tree | Syntax Tree (AST) |
|---------|-----------|-------------------|
| Contains | All grammar symbols including intermediate | Only essential nodes |
| Nodes | All non-terminals shown | Redundant nodes removed |
| Size | Larger | Smaller / Compact |

**Example for** `a + b * c`:
```
Parse Tree:                    Syntax Tree (AST):
      E                               +
    / | \                           /   \
   E  +  T                         a     *
   |    / | \                           / \
   T   T  *  F                         b   c
   |   |     |
   F   F     id(c)
   |   |
  id(a) id(b)
```

## 4️⃣ Shift-Reduce Parsing
> **Shift-Reduce** = Bottom-up parsing method using a **stack**.
> Two operations: **Shift** (push token) or **Reduce** (apply production rule)

### Example: Parse `id * (id + id)`
Grammar: `E → E + T | T`, `T → T * F | F`, `F → ( E ) | id`

| Stack | Input | Action |
|-------|-------|--------|
| $ | id*(id+id)$ | Shift |
| $id | *(id+id)$ | Reduce F→id |
| $F | *(id+id)$ | Reduce T→F |
| $T | *(id+id)$ | Shift |
... (continues until `$E` and empty input → ACCEPT)

### Conflicts:
- **Shift-Reduce Conflict:** Stack top can be reduced, OR next token can be shifted.
- **Reduce-Reduce Conflict:** Same stack symbols can be reduced by 2 different rules.

---

# 📘 Topic 2: Top-Down Parsing & LL(1)

## 📌 Quick Keywords
> `LL(1)` · `FIRST` · `FOLLOW` · `Predictive Parser` · `Parsing Table` · `Left Recursion` · `Left Factoring`

## 1️⃣ Problems in Top-Down Parsing
**Problem 1: Left Recursion** (`E → E + T`) → Infinite loop!
**Solution:** Remove left recursion (`E → TE'`, `E' → +TE' | ε`)

**Problem 2: Backtracking** (`A → ab | ac`) → Inefficient!
**Solution:** Left Factoring (`A → aA'`, `A' → b | c`)

## 2️⃣ FIRST and FOLLOW Sets 🔥
> **FIRST(α)** = Set of terminals jo `α` se derive hone wali string ka pehla symbol ho sakta hai.
> **FOLLOW(A)** = Set of terminals that can appear **immediately after** A.

### Example FIRST & FOLLOW:
```
E  → TE'       FIRST(E) = {(, id}       FOLLOW(E) = {), $}
E' → +TE' | ε  FIRST(E') = {+, ε}       FOLLOW(E') = {), $}
T  → FT'       FIRST(T) = {(, id}       FOLLOW(T) = {+, ), $}
```

## 3️⃣ Construct LL(1) Parsing Table
1. For each `A → α`, for each terminal `a` in FIRST(α), add rule to `M[A, a]`
2. If ε in FIRST(α), add rule to `M[A, b]` for each `b` in FOLLOW(A)

---

# 📘 Topic 3: LR Parsers & SLR

## 📌 Quick Keywords
> `LR(0) Items` · `Closure` · `Goto` · `SLR` · `Action Table` · `Goto Table`

## 1️⃣ LR(0) Items & Closure
> **LR(0) Item** = Production with a **dot (·)** showing progress.
> `E → E · + T` (E parsed, + expected next)

> **Closure(I)** = Add items if dot is before a non-terminal.
> **Goto(I, X)** = Move dot past X and find Closure.

## 2️⃣ Building SLR Table
- **ACTION Table:** Uses Shift (`s`), Reduce (`r`), Accept.
  - SLR mein REDUCE action sirf un symbols pe hota hai jo **FOLLOW** set mein hain.
- **GOTO Table:** Transitions for non-terminals.

---

# 📘 Topic 4: CLR & LALR Parsing

## 📌 Quick Keywords
> `LR(1) Items` · `Lookahead` · `CLR Table` · `LALR` · `Merging States`

## 1️⃣ LR(1) Items
> SLR ki problem: Reduce action sab jagah daal deta hai FOLLOW ki wajah se.
> **LR(1) Item** = `[A → α · β, a]` (production + exact lookahead)

## 2️⃣ CLR vs SLR vs LALR
| Parser | Lookahead | States | Power |
|--------|-----------|--------|-------|
| SLR | FOLLOW sets | Fewest | Weakest |
| LALR | Merged from CLR | Same as SLR | Practical (Used in YACC) |
| CLR | Exact in items | Most | Strongest |

## 3️⃣ LALR Table Construction
> Merge CLR states that have the same **core** (LR(0) items without lookaheads).
```
I4: [A → α ·, a] and I7: [A → α ·, b]
Merged to I4,7: [A → α ·, a/b]
```
- Problem: Merging can sometimes create Reduce-Reduce conflicts.

---

## ✍️ Exam Answers — Unit 2

### 📝 2 Marks — "Explain shift-reduce parsing"
> **Shift-Reduce Parsing** is a bottom-up parsing technique that uses a stack. It has two main operations: **Shift** (pushes the next input token onto stack) and **Reduce** (replaces the matching RHS of a rule on stack with its LHS non-terminal).

### 📝 2 Marks — "Differentiate Top-Down and Bottom-Up"
> **Top-Down** starts from the start symbol, expanding non-terminals (e.g., LL(1)). It struggles with left recursion. **Bottom-Up** starts from the input, reducing it to the start symbol (e.g., LR, LALR). It handles a larger class of grammars.

### 📝 7 Marks — "Discuss CLR and LALR"
> **CLR (Canonical LR)** uses LR(1) items which include exact lookahead symbols to avoid invalid reductions. It is very powerful but creates too many states.
> **LALR (Lookahead LR)** merges CLR states that share the same core (same items but different lookaheads). It reduces state count significantly while keeping most of CLR's power. YACC uses LALR.

---
## ✅ Unit 2 Complete! ✅
