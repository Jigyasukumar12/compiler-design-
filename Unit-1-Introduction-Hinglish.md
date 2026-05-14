# 📗 UNIT 1 — Introduction to Compiler (Hinglish Notes)
### Topics: Phases, Passes, Bootstrapping, FSM, Regex, LEX, CFG, BNF, YACC

---

## 🎯 Unit Ka Goal

> Is unit mein compiler ka poora **introduction** hai — woh kya hai, kaise kaam karta hai, aur lexical analysis kaise hoti hai.

---

## 📋 Syllabus Breakdown

| Topic | Priority | PYQ Frequency |
|-------|----------|--------------|
| Phases and Passes of Compiler | ⭐⭐⭐ HIGH | Har saal Section A mein |
| Bootstrapping | ⭐⭐ MEDIUM | 2-3 baar aaya |
| Finite State Machines (FSM) | ⭐⭐⭐ HIGH | Har saal — numerical bhi |
| Regular Expressions + NFA/DFA | ⭐⭐⭐ HIGH | Numerical form mein aata hai |
| Lexical Analysis + LEX | ⭐⭐ MEDIUM | Section B mein aata hai |
| Formal Grammars + CFG | ⭐⭐⭐ HIGH | Har saal |
| BNF Notation + Ambiguity | ⭐⭐⭐ HIGH | Ambiguous grammar numerical |
| YACC | ⭐ LOW | Sirf define karna |

---

## 🔥 Is Unit Se Exam Mein Kya Aata Hai

### Section A (2 marks) — Jo ZAROOR aayega:
- "Define bootstrapping in the context of compilers" ← 2022, 2023, 2024 mein aaya
- "Which phase of compiler is optional and why?"
- "What is FSM?"
- "What is ambiguous grammar?"
- "What are capabilities of CFG?"

### Section B/C (7-10 marks) — Jo baar baar aaya:
- FSM aur Regular Expression ka relationship + NFA construct karo
- NFA/DFA construct karo given regex se (NUMERICAL)
- Ambiguous grammar check karo + unambiguous mein convert karo
- Lexical analysis + syntax analysis explain karo with example

---

# 📘 Topic 1: Phases and Passes of Compiler

## 📌 Quick Keywords
> `Compiler` · `Phases` · `Passes` · `Bootstrapping` · `Symbol Table` · `Error Handler`

## 1️⃣ Compiler Kya Hai?
**Compiler** ek program hai jo **high-level language** (C, Java) ko **low-level machine code** mein translate karta hai.

```
Source Program (C code)
        ↓
    [ COMPILER ]
        ↓
Target Program (Machine Code / Assembly)
```

> **Language Translator:** Koi bhi program jo ek language ko doosri language mein convert kare.
> **Compiler** ek specific type ka language translator hai.

---

## 2️⃣ Phases of Compiler
> **Phase = Compiler ka ek logical step** — har phase ek specific kaam karta hai.

```
Source Code
    │
    ▼
┌─────────────────────┐
│  1. Lexical Analysis │  ← Characters → Tokens
└─────────┬───────────┘
          │
    ▼
┌─────────────────────┐
│  2. Syntax Analysis  │  ← Tokens → Parse Tree
│     (Parser)         │
└─────────┬───────────┘
          │
    ▼
┌─────────────────────┐
│  3. Semantic Analysis│  ← Meaning check (type checking)
└─────────┬───────────┘
          │
    ▼
┌──────────────────────────┐
│  4. Intermediate Code    │  ← Platform-independent code
│     Generation           │
└─────────┬────────────────┘
          │
    ▼
┌──────────────────────────┐
│  5. Code Optimization    │  ← Faster / Smaller code (OPTIONAL)
└─────────┬────────────────┘
          │
    ▼
┌──────────────────────────┐
│  6. Code Generation      │  ← Target machine code
└──────────────────────────┘

    (Throughout all phases)
┌──────────────────────────┐
│  Symbol Table Manager    │  ← Variables, functions store karta hai
│  Error Handler           │  ← Errors detect & report karta hai
└──────────────────────────┘
```

#### Phase 1: Lexical Analysis (Scanning)
- **Kaam:** Source code ko characters mein padhta hai → **Tokens** banata hai
- **Token** = Meaningful unit (keyword, identifier, operator, literal)
- Example: `a = b + 2` → `[id:a]` `[=]` `[id:b]` `[+]` `[num:2]`
- **Tool:** LEX

#### Phase 2: Syntax Analysis (Parsing)
- **Kaam:** Tokens ko lekar **Parse Tree** banata hai
- Grammar rules check karta hai
- **Tool:** YACC

#### Phase 3: Semantic Analysis
- **Kaam:** **Meaning** check karta hai — type checking, scope checking
- Example: `int a = "hello"` — type mismatch error detect karta hai

#### Phase 4: Intermediate Code Generation
- **Kaam:** **Three Address Code (3AC)** ya other intermediate form generate karta hai
- Platform-independent hota hai
- Example: `t1 = b + 2`, `a = t1`

#### Phase 5: Code Optimization ⚠️ OPTIONAL PHASE
- **Kaam:** Intermediate code ko **faster/smaller** banata hai
- **Yeh OPTIONAL hai kyunki:** Bina optimization ke bhi compiler kaam kar sakta hai.

#### Phase 6: Code Generation
- **Kaam:** Intermediate code → **Target Machine Code** (Assembly/Object code)
- Register allocation, memory assignment

---

## 3️⃣ Passes of Compiler
> **Pass = Compiler source code ko kitni baar scan karta hai**

| Type | Description |
|------|-------------|
| **Single Pass** | Source code ek baar scan hota hai — fast but limited |
| **Multi Pass** | Source code kai baar scan hota hai — flexible, better optimization |

> **Phases vs Passes:**
> - **Phase** = Logical division of work
> - **Pass** = Physical scan of source code
> - Ek pass mein multiple phases ho sakti hain

---

## 4️⃣ Bootstrapping
> **Bootstrapping** = Compiler ko **usi language mein likhna** jo woh compile karta hai.

```
Step 1: C compiler ko pehle kisi aur language (Assembly) mein likho → C_v1
Step 2: C_v1 use karke C compiler ko C mein likho → C_v2
Step 3: C_v2 use karke C compiler ko dobara compile karo → Final C Compiler ✅
```

## 5️⃣ Symbol Table & Error Handler
- **Symbol Table:** Har phase use karta hai — Variables, functions, types ka data store karta hai
- **Error Handler:** Lexical (`@a = b`), Syntax (`a = + b`), Semantic (`int x = "abc"`)

---

## ✍️ Exam Answers — Phases & Passes

### 📝 2 Marks — "Define Bootstrapping"
> **Bootstrapping** is the process of writing a compiler for a language using that same language itself. It involves a three-step process: first writing a minimal compiler in another language, then using it to compile the new compiler written in the target language, and finally using the resulting compiler to recompile itself.

### 📝 2 Marks — "Which phase of compiler is optional and why?"
> **Code Optimization** is the optional phase. It is optional because a compiler can produce correct target code even without optimization. This phase only improves the efficiency of the generated code but is not required for correctness.

### 📝 7 Marks — "Explain phases of compiler with example"
**Introduction:**
A compiler translates source code into machine code through a series of **phases**.

**The Six Phases:**
1. **Lexical Analysis:** Reads characters → produces Tokens. Example: `a = b + 2` → `id(a), =, id(b), +, num(2)`
2. **Syntax Analysis:** Tokens → Parse Tree (grammar rules check)
3. **Semantic Analysis:** Meaning check — type checking, scope rules
4. **Intermediate Code Generation:** Platform-independent 3AC. Example: `t1 = b + 2`, `a = t1`
5. **Code Optimization** (Optional): Makes code faster/smaller
6. **Code Generation:** Intermediate code → Target Machine Code

**Supporting:** Symbol Table + Error Handler used throughout.

```
Source Code → [Lexical] → Tokens → [Syntax] → Parse Tree
→ [Semantic] → Annotated Tree → [ICG] → 3AC → [Optimization] → [Code Gen] → Target Code
```

---

# 📘 Topic 2: Finite State Machines & Regular Expressions

## 📌 Quick Keywords
> `FSM` · `NFA` · `DFA` · `Regular Expression` · `Transition Table` · `Subset Construction` · `ε-closure`

## 1️⃣ FSM Kya Hai?
> **FSM (Finite Automaton)** = Ek mathematical model jo strings accept ya reject karta hai.

```
M = (Q, Σ, δ, q0, F)
Q = States ka finite set, Σ = Input alphabet, δ = Transition function, q0 = Start state, F = Accept states
```

## 2️⃣ NFA vs DFA

| Feature | NFA | DFA |
|---------|-----|-----|
| Transitions | Multiple next states possible | Exactly ek next state |
| ε-transitions | Allowed | Not allowed |
| Easier to | Construct from Regex | Implement in software |

## 3️⃣ RE → NFA (Thompson's Construction)
```
Regex: a     →  NFA: →(q0)--a--→((q1))
Regex: a|b   →  ε-transitions se dono paths
Regex: ab    →  (a ke NFA ka end) --ε--> (b ke NFA ka start)
Regex: a*    →  ε-loop back to start, ε to end also
```

## 4️⃣ NFA to DFA — Subset Construction
1. Start state of DFA = ε-closure(start state of NFA)
2. Har new DFA state ke liye, compute transitions
3. New state = ε-closure(set of NFA states reachable)
4. Repeat until no new states

### Example: (a|b)*abb ka DFA Table:
| DFA State | NFA States | On 'a' | On 'b' |
|-----------|-----------|--------|--------|
| A (start) | {0,1,2,7} | B | C |
| B | {1,2,3,4,6,7,8} | B | D |
| C | {1,2,5,6,7} | B | C |
| D | {1,2,5,6,7,9} | B | E |
| E (final) | {1,2,5,6,7,10} | B | C |

## ✍️ Exam Answer — "What is FSM?" (2 Marks)
> A **FSM** is a mathematical model with finite states, input alphabet, transition function, start state, and accepting states. It reads input symbols, changes states, and accepts if it ends in an accepting state. Used in lexical analysis for token recognition.

---

# 📘 Topic 3: Lexical Analysis & LEX

## 1️⃣ Lexical Analysis Kya Hai?
> Compiler ki **pehli phase** jo source code ko characters se padhkar **tokens** banati hai.

```
Input:   position = initial + rate * 60
Tokens:  id(position), op(=), id(initial), op(+), id(rate), op(*), num(60)
```

## 2️⃣ Token, Lexeme, Pattern
| Term | Definition | Example |
|------|-----------|---------| 
| **Token** | Category of symbol | `identifier`, `keyword` |
| **Lexeme** | Actual string | `position`, `if`, `+` |
| **Pattern** | Rule (regex) | `[a-z][a-z0-9]*` |

## 3️⃣ LEX Program Structure
```
%{ /* C declarations */ %}
%%
/* Rules: Pattern {Action} */
[a-zA-Z][a-zA-Z0-9]*   { printf("IDENTIFIER: %s\n", yytext); }
[0-9]+                  { printf("NUMBER: %s\n", yytext); }
%%
/* User code: main() */
```

LEX Flow: `.l file → LEX → lex.yy.c → C Compiler → Lexical Analyzer`

---

# 📘 Topic 4: Formal Grammars — CFG, BNF, Ambiguity

## 1️⃣ Grammar
```
G = (V, T, P, S)  — V=Non-terminals, T=Terminals, P=Productions, S=Start symbol
```

## 2️⃣ CFG: `A → α` (left side = single non-terminal)
```
E → E + T | T
T → T * F | F
F → ( E ) | id
```

## 3️⃣ BNF Notation
```
<expr> ::= <expr> + <term> | <term>
```

## 4️⃣ Derivation
- **Leftmost:** Hamesha leftmost non-terminal replace karo
- **Rightmost:** Hamesha rightmost non-terminal replace karo

## 5️⃣ Ambiguous Grammar 🔥
> Ek hi string ke liye **2+ parse trees** → Grammar AMBIGUOUS hai!

Example: `E → E+E | E*E | id` — string `id + id * id` ke 2 parse trees bante hain.

**Fix:** Precedence aur associativity encode karo:
```
E → E + T | T      (+ lower precedence)
T → T * F | F      (* higher precedence)
F → id | ( E )
```

## 6️⃣ Left Recursion Removal
```
Before: E → E + T | T
After:  E → TE', E' → +TE' | ε
```

## 7️⃣ Left Factoring
```
Before: stmt → if expr then stmt | if expr then stmt else stmt
After:  stmt → if expr then stmt rest, rest → else stmt | ε
```

## ✍️ Exam Answer — Ambiguous Grammar (7 Marks)
Show 2 parse trees for `id + id * id`, then convert:
```
E → E + T | T,  T → T * F | F,  F → id | ( E )
```
Verify only ONE parse tree possible now. ✅

---

## 🔑 Important Keywords — Unit 1
- Token, Lexeme, FSM, NFA, DFA, ε-closure, Thompson's Construction, Subset Construction
- CFG, BNF, Ambiguous Grammar, Left Recursion, Left Factoring
- Bootstrapping, Symbol Table, Error Handler, Single/Multi Pass
