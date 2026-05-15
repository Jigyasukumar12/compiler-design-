# 📗 Unit 1 — Compiler Design Introduction (Complete Hinglish Notes)
### BCS602 | AKTU Semester VI | Dr. A.P.J. Abdul Kalam Technical University

---

# 📋 Unit 1 — Poora Overview

## 🎯 Unit Ka Goal

> Is unit mein **compiler ka poora introduction** hai — woh kya hai, kaise kaam karta hai, aur lexical analysis kaise hoti hai. Source code se machine code tak ka safar samjhenge.

---

## 📊 Topics Coverage

| Topic | Priority | PYQ Frequency |
|-------|----------|--------------|
| Phases aur Passes of Compiler | ⭐⭐⭐ HIGH | Har saal Section A mein |
| Bootstrapping | ⭐⭐ MEDIUM | 2-3 baar aaya |
| Finite State Machines (FSM) | ⭐⭐⭐ HIGH | Har saal — numerical bhi |
| Regular Expressions + NFA/DFA | ⭐⭐⭐ HIGH | Numerical form mein aata hai |
| Lexical Analysis + LEX | ⭐⭐ MEDIUM | Section B mein aata hai |
| Formal Grammars + CFG | ⭐⭐⭐ HIGH | Har saal |
| BNF Notation + Ambiguity | ⭐⭐⭐ HIGH | Ambiguous grammar numerical |
| YACC | ⭐ LOW | Sirf define karna |

---

## 🔥 Exam Mein Kya Aata Hai

### Section A (2 marks) — Jo ZAROOR aayega:
- "Define bootstrapping compiler ke context mein" ← 2022, 2023, 2024 mein aaya
- "Compiler ka kaunsa phase optional hai aur kyun?"
- "FSM kya hai?"
- "Ambiguous grammar kya hai?"
- "CFG ki capabilities kya hain?"
- "Language translator aur compiler mein difference"

### Section B/C (7-10 marks) — Jo baar baar aaya:
- FSM aur Regular Expression ka relationship + NFA construct karo
- NFA/DFA construct karo given regex se (NUMERICAL)
- Ambiguous grammar check karo + unambiguous mein convert karo
- Lexical analysis + syntax analysis explain karo with example

---

## ⏱️ Preparation Time

> **Total: 5-6 din** (1.5 hrs/day)
> - Phases & FSM: 2 din (numerical practice karo)
> - Lexical Analysis + LEX: 1 din
> - CFG + BNF + Ambiguity: 2 din (numerical practice karo)

---

---

# 🔄 Topic 1: Phases aur Passes of Compiler
### Priority: ⭐⭐⭐ HIGH

## 📌 Quick Keywords
> `Compiler` · `Phases` · `Passes` · `Bootstrapping` · `Symbol Table` · `Error Handler`

---

## 1️⃣ Compiler Kya Hai?

**Compiler** ek aisa program hai jo **high-level language** (jaise C, Java) ko **low-level machine code** mein translate karta hai.

```
Source Program (C code)
        ↓
    [ COMPILER ]
        ↓
Target Program (Machine Code / Assembly)
```

> **Language Translator:** Koi bhi program jo ek language ko doosri language mein convert kare, usse language translator kehte hain.
> **Compiler** ek specific type ka language translator hai jo high-level ko low-level mein convert karta hai.

---

## 2️⃣ Compiler Ke Phases

> **Phase = Compiler ka ek logical step** — har phase ek specific kaam karta hai aur output ko next phase ke liye ready karta hai.

```
Source Code
    │
    ▼
┌─────────────────────┐
│  1. Lexical Analysis │  ← Characters ko Tokens mein convert
└─────────┬───────────┘
          │
    ▼
┌─────────────────────┐
│  2. Syntax Analysis  │  ← Tokens se Parse Tree banata
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
│  5. Code Optimization    │  ← Faster/Smaller code (OPTIONAL)
└─────────┬────────────────┘
          │
    ▼
┌──────────────────────────┐
│  6. Code Generation      │  ← Target machine code
└──────────────────────────┘

    (Sabhi phases ke saath)
┌──────────────────────────┐
│  Symbol Table Manager    │  ← Variables, functions store
│  Error Handler           │  ← Errors detect & report
└──────────────────────────┘
```

---

### Phase-wise Detailed Explanation

#### Phase 1: Lexical Analysis (Scanning)
- **Kaam:** Source code ko characters mein padhkar **Tokens** banata hai
- **Token** = Meaningful unit (keyword, identifier, operator, literal)
- **Example:** `a = b + 2` 
  - Output: `[id:a]` `[=]` `[id:b]` `[+]` `[num:2]`
- **Tool:** LEX
- **Hinglish Samajh:** Jaise sentence ko words mein todna

#### Phase 2: Syntax Analysis (Parsing)
- **Kaam:** Tokens ko lekar **Parse Tree** banata hai
- Grammar rules check karta hai
- **Example:** `a = b + 2` ka parse tree structure verify karta hai
- **Tool:** YACC
- **Hinglish Samajh:** Words se sentence ka structure banana aur check karna ki grammatically correct hai ya nahi

#### Phase 3: Semantic Analysis
- **Kaam:** **Meaning** check karta hai — type checking, scope checking
- **Example:** `int a = "hello"` — type mismatch error detect karega
- Annotated parse tree banata hai
- **Hinglish Samajh:** Sentence grammatically toh correct hai, par meaning banti hai ya nahi

#### Phase 4: Intermediate Code Generation
- **Kaam:** **Three Address Code (3AC)** ya other intermediate form generate karta hai
- Platform-independent hota hai (kisi bhi machine pe run ho sakta hai)
- **Example:** `t1 = b + 2`, `a = t1`
- **Hinglish Samajh:** Hindi ko English mein convert karne se pehle ek common language mein lana

#### Phase 5: Code Optimization ⚠️ OPTIONAL PHASE
- **Kaam:** Intermediate code ko **faster ya smaller** banata hai
- Redundant (repeated/unnecessary) code remove karta hai
- > **Yeh OPTIONAL kyun hai:** Bina optimization ke bhi compiler correct code produce kar sakta hai. Optimization sirf performance improve karta hai, correctness ke liye zaruri nahi hai.
- **Hinglish Samajh:** Message ko chota aur clear banana without meaning change kiye

#### Phase 6: Code Generation
- **Kaam:** Intermediate code ko **Target Machine Code** (Assembly/Object code) mein convert karta hai
- Register allocation, memory assignment karta hai
- **Hinglish Samajh:** Final language mein convert karna jo machine samajh sake

---

## 3️⃣ Compiler Ke Passes

> **Pass = Compiler source code ko kitni baar completely scan karta hai**

| Type | Description |
|------|-------------|
| **Single Pass** | Source code ek hi baar scan hota hai — fast but limited optimization |
| **Multi Pass** | Source code kai baar scan hota hai — flexible, better optimization possible |

```
Single Pass Compiler:
Source → [Saare phases ek scan mein] → Target Code

Multi Pass Compiler:
Source → [Pass 1: Lexical+Syntax] → [Pass 2: Semantic] → [Pass 3: Code Gen] → Target
```

> **Phases vs Passes ka Difference:**
> - **Phase** = Kaam ka logical division (conceptual)
> - **Pass** = Source code ka physical scan (practical)
> - Ek pass mein multiple phases ho sakti hain

---

## 4️⃣ Bootstrapping

> **Bootstrapping** = Compiler ko **usi language mein likhna** jo woh compile karta hai.

```
Problem: Maan lo C compiler banana hai C language mein — 
         but C compiler toh already chahiye C code run karne ke liye!

Solution (Bootstrapping):
Step 1: C compiler ko pehle kisi aur language (jaise Assembly) mein likho → C_v1
Step 2: C_v1 use karke C compiler ko C mein likho → C_v2
Step 3: C_v2 use karke apne aap ko dobara compile karo → Final C Compiler ✅
```

**Real-life Example:**
- Jaise apne bootstrap (shoe laces) khud hi kheenchke khade hona
- **"Pulling yourself up by your bootstraps"** — khud apne dum pe khada hona

---

## 5️⃣ Symbol Table

- **Kaam:** Variables, functions, types ka data store karna
- Har phase isse use karta hai
- Scope information rakhta hai (kaunsa variable kahan valid hai)
- Efficient data structures use karta hai (hash table, binary tree)

## 6️⃣ Error Handler

- Compile ke **har phase** mein errors detect hoti hain:
  - **Lexical Error:** `@a = b` — illegal character `@`
  - **Syntax Error:** `a = + b` — wrong grammar
  - **Semantic Error:** `int x = "abc"` — type mismatch
- User ko clear error messages deta hai

---

## ✍️ Exam Answers (Hinglish)

---

### 📝 2 Marks — "Bootstrapping define karo"

> **Bootstrapping** ek aisa process hai jismein compiler ko usi language mein likha jata hai jise woh compile karta hai. Isme teen steps hote hain: pehle ek minimal compiler dusri language mein likha jata hai, phir usse use karke naya compiler target language mein likha jata hai, aur finally woh compiler khud ko compile karta hai. Isse compiler self-hosting ban jata hai.

**English Translation:**
> **Bootstrapping** is the process of writing a compiler for a language using that same language itself. It involves three steps: first writing a minimal compiler in another language, then using it to compile the new compiler written in the target language, and finally using the resulting compiler to recompile itself. This makes the compiler self-hosting.

---

### 📝 2 Marks — "Compiler ka kaunsa phase optional hai aur kyun?"

> **Code Optimization** compiler ka optional phase hai. Yeh isliye optional hai kyunki compiler bina optimization ke bhi correct target code produce kar sakta hai. Yeh phase sirf generated code ki efficiency (speed aur size) improve karta hai lekin correctness ke liye zaruri nahi hai. Compiler iske bina bhi puri tarah se kaam kar sakta hai.

**English Translation:**
> **Code Optimization** is the optional phase of a compiler. It is optional because a compiler can produce correct target code even without optimization. This phase only improves the efficiency (speed and size) of the generated code but is not required for correctness. A compiler can function completely without it.

---

### 📝 7 Marks — "Compiler ke phases ko example ke saath explain karo"

**Introduction (Parichay):**
Compiler ek aisa program hai jo high-level language mein likhe source code ko equivalent machine code mein translate karta hai. Yeh translation ek series of well-defined steps ke through hota hai jinhe **phases** kehte hain. Har phase ek specific transformation perform karta hai.

**Compiler Ke Chhe Phases:**

**1. Lexical Analysis (Scanning)**
Pehla phase source program ko character by character padhta hai aur unhe meaningful units mein group karta hai jinhe **tokens** kehte hain. 

Example: Input `a = b + 2` ke liye:
- Tokens produced: `id(a)`, `=`, `id(b)`, `+`, `num(2)`
- Tool: LEX

**2. Syntax Analysis (Parsing)**
Parser token stream leta hai aur language ke grammar rules ke against check karta hai, aur **Parse Tree** banata hai.
- Input: Token stream
- Output: Parse Tree / Syntax Tree
- Grammar rules verify karta hai

**3. Semantic Analysis**
Yeh phase program ki **meaning** check karta hai — type checking, scope rules, aur declarations.
- Errors detect karta hai jaise: string ko integer variable mein assign karna
- Output: Annotated parse tree

**4. Intermediate Code Generation**
Parse tree ko **platform-independent intermediate representation** mein convert karta hai, typically Three Address Code (3AC).
- Example: `t1 = b + 2`, `a = t1`
- Kisi bhi machine ke liye same hoga

**5. Code Optimization** *(Optional Phase)*
Intermediate code ko improve karta hai taaki woh faster ya smaller ho, without meaning change kiye.
- Redundant computations remove karta hai
- Yeh phase **optional** hai — correctness iss par depend nahi karti

**6. Code Generation**
Optimized intermediate code ko **target machine code** (assembly ya object code) mein translate karta hai.
- Register allocation aur memory assignment handle karta hai
- Final executable code banata hai

**Supporting Components:**
Sabhi phases ke dauran, **Symbol Table** variables aur functions ki information store karta hai, jabki **Error Handler** har stage pe errors detect aur report karta hai.

**Diagram:**
```
Source Code → [Lexical] → Tokens → [Syntax] → Parse Tree
→ [Semantic] → Annotated Tree → [Intermediate Code Gen]
→ 3AC → [Optimization] → Optimized Code → [Code Gen] → Target Code
```

**Conclusion (Nishkarsh):**
Compiler ki phased structure modularity ensure karti hai, error detection ko easier banati hai, aur better code quality provide karti hai. Har phase program ke representation ko step by step transform karta hai jab tak machine-executable code nahi ban jata.

---

## 🔑 Important Keywords Yaad Rakho
- Token, Lexeme, Lexical Analysis
- Parse Tree, Syntax Analysis
- Type Checking, Semantic Analysis
- Three Address Code, Intermediate Representation
- Code Optimization (OPTIONAL)
- Symbol Table, Error Handler
- Single Pass, Multi Pass
- Bootstrapping, Self-hosting

---

---

# 🤖 Topic 2: Finite State Machines & Regular Expressions
### Priority: ⭐⭐⭐ HIGH — Numerical bhi aata hai

## 📌 Quick Keywords
> `FSM` · `NFA` · `DFA` · `Regular Expression` · `Transition Table` · `Subset Construction` · `ε-closure`

---

## 1️⃣ Finite State Machine (FSM) Kya Hai?

> **FSM (Finite Automaton)** = Ek mathematical model jo strings ko accept ya reject karta hai.
> Lexical analysis mein tokens recognize karne ke liye use hota hai.

**Real-life Example:**
- Jaise traffic light — limited states (red, yellow, green) aur transitions
- ATM machine — different states (idle, PIN entry, transaction, etc.)

### FSM Ke 5 Components:
```
M = (Q, Σ, δ, q0, F)

Q   = States ka finite set         Example: {q0, q1, q2}
Σ   = Input alphabet (symbols)     Example: {a, b, 0, 1}
δ   = Transition function          δ(state, input) = next state
q0  = Start state (initial state)
F   = Accept states (final states)
```

---

## 2️⃣ NFA vs DFA

| Feature | NFA (Non-deterministic) | DFA (Deterministic) |
|---------|------------------------|---------------------|
| Full Form | Non-deterministic Finite Automaton | Deterministic Finite Automaton |
| Transitions | Ek state se multiple next states possible | Ek state se exactly ek next state |
| ε-transitions | Allowed (empty string pe transition) | Not allowed |
| Design | Easy to construct from Regex | Complex to construct |
| Implementation | Complex (multiple paths check) | Easy (single path follow) |
| Conversion | NFA → DFA possible | — |
| Acceptance | Agar **koi bhi ek** path accept kare | Exactly **ek hi** path hai |

**Hinglish Samajh:**
- **NFA:** Jaise multiple raaste se destination tak pahunchna — koi bhi ek raasta sahi ho to accept
- **DFA:** Sirf ek fixed raasta — deterministic (pakka pata hai next kahan jaana hai)

---

## 3️⃣ Regular Expression → NFA (Thompson's Construction)

Regular Expression ke basic operations:

| Operation | Symbol | Meaning |
|-----------|--------|---------|
| Concatenation | ab | 'a' ke baad 'b' |
| Alternation | a \| b | Ya to 'a' ya 'b' |
| Kleene Star | a* | Zero ya more 'a's |
| Plus | a+ | Ek ya more 'a's |
| Optional | a? | Zero ya ek 'a' |

### Basic Construction Rules:

```
Regex: a          
NFA: →(q0)--a--→((q1))

Regex: a|b        
NFA: ε-transitions se dono paths
        →(q0)--ε--→(q1)--a--→(q2)--ε--→((q4))
             └--ε--→(q3)--b--→(q2)--ε--↗

Regex: ab         
(a ke NFA ka end) --ε--> (b ke NFA ka start)

Regex: a*         
ε-loop back to start, ε to end bhi
```

---

## 4️⃣ Example — NFA for a(b|c)*

> **PYQ 2023-24 Section C:** "Given regex a(b|c)*, construct NFA"

**Step 1:** Break down karo: `a` ke baad `(b|c)*`

**Step 2:** NFA diagram:
```
        a           b
→(q0)------→(q1)──────→(q2)
              │  ε ↗↙ ε  │
              └──(q3)─────┘
                   │ c
                   ↓
                 (q4)──ε──→ back to q1 loop
```

**Simplified representation:**
```
States: q0, q1, q2
Start: q0
Final: q2

Transitions:
q0 --a--> q1
q1 --b--> q2   (loop back: q2 --ε--> q1)
q1 --c--> q2   (loop back: q2 --ε--> q1)
q1 --ε--> q2   (for zero occurrences)
```

**Meaning:** String "a" ke baad zero ya more b's ya c's (koi bhi combination).
**Accepts:** `a`, `ab`, `ac`, `abc`, `abbc`, `acbc`, `abcbcb`, etc.

---

## 5️⃣ NFA to DFA — Subset Construction

> **PYQ 2022-23:** "Construct NFA and DFA for (0+1)*(00+11)(0+1)*"

### Algorithm Steps (Hinglish):
1. DFA ka start state = ε-closure(NFA ka start state)
2. Har new DFA state ke liye, har input symbol pe transitions compute karo
3. New state = ε-closure(set of NFA states jo reachable hain)
4. Repeat karo jab tak koi new states nahi banti

**ε-closure kya hai:**
> Ek state se sirf ε-transitions (empty string) pe jo saari states reach ho sakti hain

### Example: NFA for (a|b)*abb

**NFA States:**
```
States: {0,1,2,3,4,5,6,7,8,9,10}
q0 pe (a|b)* loop
Final part "abb" sequence detect karta hai
```

**DFA Transition Table (Subset Construction):**

| DFA State | NFA States (ε-closure) | On 'a' | On 'b' |
|-----------|----------------------|--------|--------|
| A (start) | {0,1,2,7} | B | C |
| B | {1,2,3,4,6,7,8} | B | D |
| C | {1,2,5,6,7} | B | C |
| D | {1,2,5,6,7,9} | B | E |
| E (final) | {1,2,5,6,7,10} | B | C |

**Hinglish Explanation:**
- Har DFA state ek **set of NFA states** represent karta hai
- Agar set mein koi bhi final state hai, to woh DFA state bhi final hoga
- Transitions naye sets create karti hain

---

## 6️⃣ DFA Optimization — Minimization

> Equivalent states ko remove kar do, combine kar do.

**Steps:**
1. Accepting aur non-accepting states ko separate karo
2. States jinka identical transitions ho, unhe identify karo
3. Equivalent states ko merge karo
4. Final minimized DFA milega

**Benefit:** Kam states = kam memory, faster execution

---

## ✍️ Exam Answers (Hinglish)

---

### 📝 2 Marks — "FSM kya hai?"

> **Finite State Machine (FSM)** ek mathematical model hai jismein finite number of states hote hain, ek input alphabet, ek transition function, ek start state, aur final/accepting states ka set. Yeh input symbols ko ek ek karke padhta hai, transition function ke according states change karta hai, aur agar accepting state mein end hota hai to input ko accept karta hai. FSMs lexical analysis mein tokens (identifiers, keywords, operators) ko recognize karne ke liye use hote hain.

**English Translation:**
> A **Finite State Machine (FSM)** is a mathematical model consisting of a finite set of states, an input alphabet, a transition function, a start state, and a set of final/accepting states. It reads input symbols one by one, changes states according to the transition function, and accepts the input if it ends in an accepting state. FSMs are used in lexical analysis to recognize tokens (identifiers, keywords, operators) in source code.

---

### 📝 7 Marks — "FSM aur Regular Expressions ke beech relationship explain karo. Lexical analysis mein regular expressions kaise use hote hain discuss karo."

**Introduction (Parichay):**
**Regular Expression (RE)** ek notation hai jo string patterns ko describe karne ke liye use hota hai. **Finite State Machine (FSM)** ek computational model hai jo strings ko accept ya reject karta hai. Yeh dono closely related hain — har regular expression ko equivalent FSM (NFA ya DFA) mein convert kiya ja sakta hai, aur vice versa.

**Regular Expressions — Basic Operations:**

| Operation | Symbol | Meaning (Hindi) |
|-----------|--------|---------|
| Concatenation | ab | 'a' ke baad 'b' aaye |
| Alternation | a \| b | Ya to 'a' ya 'b' |
| Kleene Star | a* | Zero ya zyada 'a' |
| Plus | a+ | Ek ya zyada 'a' |
| Optional | a? | Zero ya ek 'a' |

**Relationship: RE → NFA → DFA**

Standard conversion path yeh hai:
```
Regular Expression
       ↓ (Thompson's Construction)
      NFA
       ↓ (Subset Construction)
      DFA
       ↓ (Minimization)
  Minimal DFA
```

**1. RE to NFA (Thompson's Construction):**
Har RE operator ek specific NFA fragment mein map hota hai jo ε-transitions se connect hote hain. 

Example: `a(b|c)*` ke liye NFA:
- `a` simple transition hai
- `(b|c)` ε-branching use karta hai (do raaste)
- `*` loop create karta hai ε-transitions se

**2. NFA to DFA (Subset Construction):**
Kyunki NFA mein multiple possible next states hain, hum ise DFA mein convert karte hain jahan har DFA state ek **set of NFA states** ko represent karta hai. Yeh **ε-closure** ka concept use karta hai (saari states jo ε-transitions se reachable hain).

**Lexical Analysis Mein Use:**
Compiler mein, lexical analyzer (scanner) tokens recognize karne ke liye FSMs use karta hai:

```
Source Code: a = b + 2

Lexical Analyzer (DFA patterns use karke):
- Identifier pattern: [a-z][a-z0-9]*  → Regex → DFA
- Number pattern: [0-9]+              → Regex → DFA
- Operator pattern: [=+\-*/]          → Regex → DFA

Output Tokens: id(a), op(=), id(b), op(+), num(2)
```

Har token type ek regular expression se define hota hai, jo DFA mein convert hota hai. Lexical analyzer input ko in DFAs ke through run karta hai taaki longest matching token identify ho.

**Pattern Matching:**
FSMs **greedy matching** enable karte hain — hamesha longest possible token match karo. Yeh important hai kyunki `>=` ek token hona chahiye, do nahi (`>` aur `=`).

**Diagram Example:**
```
Regular Expression: [a-z][a-z0-9]*

DFA for Identifier:
→ (q0) --[a-z]--> ((q1)) --[a-z0-9]--> ((q1))
                                    ↑___________|
(q1 accepting hai aur alphanumeric pe loop karta hai)
```

**Conclusion (Nishkarsh):**
Regular expressions token patterns describe karne ke liye convenient notation provide karte hain, jabki FSMs (specially DFAs) implementation mechanism provide karte hain. LEX tool yeh conversion automate karta hai — programmer regex patterns likhta hai, aur LEX automatically corresponding DFA-based scanner generate karta hai.

---

## 🔑 Important Keywords Yaad Rakho
- FSM, NFA, DFA, ε-closure
- Thompson's Construction (RE → NFA)
- Subset Construction (NFA → DFA)
- Token, Lexeme, Pattern
- Greedy Matching, Longest Match
- Transition Table, Transition Function
- Deterministic, Non-deterministic

---

---

# 🔍 Topic 3: Lexical Analysis & LEX Compiler
### Priority: ⭐⭐ MEDIUM

## 📌 Quick Keywords
> `Lexical Analysis` · `Token` · `Lexeme` · `Pattern` · `Scanner` · `LEX` · `Symbol Table`

---

## 1️⃣ Lexical Analysis Kya Hai?

> **Lexical Analysis** = Compiler ki **pehli phase** jo source code ko characters se padhkar **tokens** banati hai.
> Lexical Analyzer ko **Scanner** ya **Lexer** bhi kehte hain.

**Hinglish Samajh:** 
- Jaise paragraph ko words mein todna
- Har meaningful word ek token ban jata hai

```
Input:   position = initial + rate * 60

Tokens Generate Hote Hain:
┌─────────────┬───────────────┬──────────┐
│   Lexeme    │     Token     │  Value   │
├─────────────┼───────────────┼──────────┤
│ position    │ identifier    │ id(1)    │
│ =           │ assignment op │ op(=)    │
│ initial     │ identifier    │ id(2)    │
│ +           │ addition op   │ op(+)    │
│ rate        │ identifier    │ id(3)    │
│ *           │ multiply op   │ op(*)    │
│ 60          │ integer lit   │ num(60)  │
└─────────────┴───────────────┴──────────┘
```

---

## 2️⃣ Token, Lexeme, Pattern

| Term | Definition (Hindi) | Example |
|------|-----------|---------|
| **Token** | Symbol ki category/type | `identifier`, `keyword`, `operator` |
| **Lexeme** | Source mein actual string | `position`, `if`, `+` |
| **Pattern** | Token ko describe karne wala rule | `[a-z][a-z0-9]*` identifier ke liye |

**Detailed Example:**
```
Code: if (x >= 100)

Token:    | keyword | punct | id | relop | num | punct |
Lexeme:   | if      | (     | x  | >=    | 100 | )     |
Pattern:  | "if"    | "("   | [a-z]+ | >=  | [0-9]+ | ")" |
```

---

## 3️⃣ Token Ke Types

```
1. Keywords:     if, else, while, for, int, float, return
                 (Reserved words — identifier nahi ban sakte)

2. Identifiers:  variable names (count, sum_total)
                 function names (calculateTotal)
                 
3. Operators:    Arithmetic: +, -, *, /, %
                 Relational: ==, !=, >, <, >=, <=
                 Logical: &&, ||, !
                 Assignment: =, +=, -=
                 
4. Literals:     Integer: 42, 0, -15
                 Float: 3.14, 2.5e-3
                 String: "hello"
                 Character: 'a'
                 
5. Punctuation:  ; , ( ) { } [ ]

6. Whitespace:   spaces, tabs, newlines
                 (Usually ignore — just separate tokens)
                 
7. Comments:     /* ... */ , // ...
                 (Usually ignore)
```

---

## 4️⃣ Lexical Errors

Lexical phase mein detect hone wale common errors:

1. **Illegal character:** `@x = 5` — `@` valid nahi hai
2. **Unterminated string:** `"hello` — string close nahi hui
3. **Invalid number format:** `3.14.15` — do decimal points
4. **Too long identifier:** (kuch languages mein limit hoti hai)

**Error Recovery:**
- Illegal character ko skip karo
- Error report karo with line number
- Scanning continue karo

---

## 5️⃣ LEX — Lexical Analyzer Generator

> **LEX** = Ek tool jo automatically **lexical analyzer** (scanner) generate karta hai, given regex patterns.

### LEX Program Ki Structure:

```
%{
/* C declarations section */
#include <stdio.h>
int line_count = 0;
%}

%%
/* Rules section: Pattern  {Action} */

[a-zA-Z][a-zA-Z0-9]*   { printf("IDENTIFIER: %s\n", yytext); }
[0-9]+                  { printf("NUMBER: %s\n", yytext); }
"+"                     { printf("PLUS\n"); }
"-"                     { printf("MINUS\n"); }
"="                     { printf("ASSIGN\n"); }
\n                      { line_count++; }
[ \t]                   { /* ignore whitespace */ }
.                       { printf("ERROR: %s\n", yytext); }

%%
/* User code section */
int main() {
    yylex();  // Scanning start
    printf("Total lines: %d\n", line_count);
    return 0;
}
```

### LEX Program Ke 3 Parts:
1. **Declarations** (`%{ %}`) 
   - C code, variables, includes
   - Global declarations
   
2. **Rules** (`%%` ke beech)
   - Pattern `{Action}` pairs
   - Regex pattern + corresponding C code
   
3. **User Code** (last `%%` ke baad)
   - main() function
   - Helper functions

### Important LEX Variables:
- **yytext:** Current lexeme (matched string)
- **yyleng:** Length of yytext
- **yylineno:** Current line number

### LEX Processing Flow:
```
LEX source file (.l file)
        ↓
    [ LEX Tool ]
        ↓
  lex.yy.c (Generated C program)
        ↓
    [ C Compiler ]
        ↓
  Lexical Analyzer (Executable)
        ↓
    [ Run on Input ]
        ↓
  Tokens Output
```

**Example Compilation:**
```bash
$ lex mylex.l          # Generates lex.yy.c
$ gcc lex.yy.c -ll     # Compile with LEX library
$ ./a.out < input.txt  # Run on input file
```

---

## 6️⃣ Lexical vs Syntactic Structure

| Aspect | Lexical Structure | Syntactic Structure |
|--------|------------------|---------------------|
| **Kaam** | Tokens banata | Grammar rules check |
| **Basis** | Regular expressions | Context-free grammars |
| **Model** | FSM/DFA based | Parser (LL, LR) based |
| **Output** | Token stream | Parse tree |
| **Phase** | First (pehla) | Second (doosra) |
| **Tool** | LEX | YACC |

**Hinglish Samajh:**
- **Lexical:** Words identify karna (dictionary check)
- **Syntactic:** Sentence structure check karna (grammar check)

---

## ✍️ Exam Answers (Hinglish)

---

### 📝 2 Marks — "LEX program ke various parts kya hain?"

> LEX program ko teen parts mein divide kiya jata hai jo `%%` se separate hote hain:
> 1. **Declarations Section:** C declarations, macro definitions, aur `%{ %}` blocks jismein header files include hoti hain.
> 2. **Rules Section:** Pattern-action pairs `pattern { action }` format mein. Har pattern ek regular expression hai aur corresponding action C code hai jo tab execute hota hai jab pattern match hota hai.
> 3. **User Code Section:** Auxiliary C functions jaise `main()` aur `yywrap()` is section mein hote hain.

**English Translation:**
> A LEX program is divided into three parts separated by `%%`:
> 1. **Declarations Section:** Contains C declarations, macro definitions, and `%{ %}` blocks for including header files.
> 2. **Rules Section:** Contains pattern-action pairs in the format `pattern { action }`. Each pattern is a regular expression; the corresponding action is C code executed when the pattern matches.
> 3. **User Code Section:** Contains auxiliary C functions like `main()` and `yywrap()`.

---

### 📝 7 Marks — "Lexical Analysis aur Syntax Analysis phases ko example ke saath explain karo. Error reporting bhi explain karo."

**Introduction (Parichay):**
Compiler ke pehle do phases — **Lexical Analysis** aur **Syntax Analysis** — source code ko padhne aur uski structure verify karne ke liye responsible hote hain. Yeh dono milkar ensure karte hain ki input program syntactically valid hai aage processing se pehle.

---

**Phase 1: Lexical Analysis**

Lexical analyzer (scanner) source program ko character by character padhta hai aur unhe **tokens** mein group karta hai — program ke smallest meaningful units.

*Key Terms (Mukhya Shabd):*
- **Token:** Logical category (identifier, keyword, operator)
- **Lexeme:** Actual matched string (jaise `count`, `if`, `+`)
- **Pattern:** Regular expression jo valid lexemes define karta hai

*Example input ke liye:* `a = (b + c) * 2`

```
Token Stream (Generated):
id(a), op(=), punct((), id(b), op(+), id(c), punct()), op(*), num(2)
```

*Lexical Phase Mein Errors:*
- Illegal character (jaise `@`, `$`)
- Unterminated string literal (`"hello` — quote close nahi)
- **Recovery Strategy:** Illegal character ko skip karo, error report karo, scanning continue karo

---

**Phase 2: Syntax Analysis (Parsing)**

Parser lexical analysis se token stream leta hai aur check karta hai ki woh programming language ke **grammar rules** ko follow kar raha hai ya nahi. Yeh **Parse Tree** produce karta hai.

*Example Grammar:*
```
E → E + T | T
T → T * F | F
F → ( E ) | id | num
```

*Parse Tree for* `a = (b + c) * 2`:
```
        Assignment
       /          \
    id(a)         Expr(*)
                 /      \
             Expr(+)    num(2)
             /    \
          id(b)  id(c)
```

*Syntax Phase Mein Errors:*
- Missing semicolon: `int x = 5`  ← `;` nahi hai
- Unmatched parenthesis: `(a + b`  ← closing bracket missing
- Wrong grammar: `a ++ b` (invalid expression)

**Error Recovery Methods:**
1. **Panic Mode:** Skip tokens jab tak synchronizing token nahi mil jata (jaise semicolon)
2. **Phrase Level:** Local correction — tokens insert/delete karo
3. **Error Productions:** Grammar mein common errors ke liye special rules add karo

---

**Interaction (Ek Doosre Se Kaise Connect Hain):**
```
Source Code
    → [Lexical Analyzer] → Token Stream
    → [Syntax Analyzer]  → Parse Tree
    → [Semantic Analyzer] → ...
```

Syntax analyzer lexical analyzer ko call karta hai jab bhi use next token chahiye (`getNextToken()` function ke through).

**Example Flow:**
```
Input: if (x > 0) y = 1;

Lexical: keyword(if), punct((), id(x), relop(>), num(0), punct()), id(y), op(=), num(1), punct(;)

Syntax: Parse tree banata hai jo if-statement ki structure verify karta hai
```

**Conclusion (Nishkarsh):**
Lexical aur Syntax Analysis milkar compiler ka **front-end** form karte hain. Lexical analysis tokens ki linear structure handle karta hai, jabki syntax analysis program ki hierarchical (tree-like) structure handle karta hai. Dono phases errors efficiently detect karte hain aur clear error messages provide karte hain.

---

## 🔑 Important Keywords Yaad Rakho
- Token, Lexeme, Pattern
- Scanner, Lexer, Lexical Analyzer
- LEX tool, yytext, yylex()
- Lexical error, Syntactic error
- Parse Tree, Token Stream
- Error Recovery, Panic Mode

---

---

# 📐 Topic 4: Formal Grammars, CFG, BNF & Ambiguity
### Priority: ⭐⭐⭐ HIGH — Ambiguity numerical zaroor aata hai

## 📌 Quick Keywords
> `CFG` · `BNF` · `Parse Tree` · `Derivation` · `Ambiguous Grammar` · `Left Recursion` · `Left Factoring`

---

## 1️⃣ Grammar Kya Hai?

> **Grammar** = Rules ka ek set jo define karta hai ki kaunsi strings ek language mein valid hain.

**Hinglish Samajh:**
- Jaise Hindi/English grammar sentences ki structure define karta hai
- Similarly, programming language grammar code ki structure define karta hai

### Formal Grammar Ke 4 Components:
```
G = (V, T, P, S)

V = Non-terminals (variables)        Example: {E, T, F}
T = Terminals (actual symbols)       Example: {id, +, *, (, )}
P = Production rules                 Example: E → E + T
S = Start symbol                     Example: E
```

**Example:**
```
Grammar for simple expressions:
V = {E, T, F}
T = {id, +, *, (, )}
S = E
P = {
    E → E + T
    E → T
    T → T * F
    T → F
    F → ( E )
    F → id
}
```

---

## 2️⃣ Context Free Grammar (CFG)

> **CFG** = Ek aisa grammar jisme har production rule ka left side ek **single non-terminal** hota hai.

```
Form: A → α
where A ∈ V (single non-terminal — ek hi variable)
      α ∈ (V ∪ T)*  (terminals aur non-terminals ka koi bhi string)
```

**CFG Kyun Important:**
- Programming languages ko define karne mein use hota hai
- Parser design ke liye base hai
- Regular languages se zyada powerful

**CFG vs Regular Grammar:**
- Regular: Sirf right-linear ya left-linear
- CFG: Koi bhi structure (nested bhi)

### Example CFG for Arithmetic Expressions:
```
E → E + T    (Expression plus Term)
E → T
T → T * F    (Term times Factor)
T → F
F → ( E )    (Parenthesized expression)
F → id       (Identifier)
```

**Yeh Grammar Kya Accept Karta Hai:**
- `id`
- `id + id`
- `id * id + id`
- `(id + id) * id`
- Nested expressions

---

## 3️⃣ BNF Notation

> **BNF (Backus-Naur Form)** = CFG likhne ka standard, readable notation.

```
BNF Format:
<symbol> ::= expression | expression | ...

Example:
<expr>   ::= <expr> + <term> | <term>
<term>   ::= <term> * <factor> | <factor>
<factor> ::= ( <expr> ) | id
```

**BNF vs Regular CFG Notation:**
- BNF mein `<>` angle brackets use hote hain non-terminals ke liye
- `::=` use hota hai `→` (arrow) ki jagah
- `|` (pipe) alternatives ke liye (same hai dono mein)
- Meaning same, notation different — zyada readable

**Extended BNF (EBNF):**
```
{...}  = Zero or more repetitions
[...]  = Optional (zero or one)
(...)  = Grouping

Example:
stmt ::= if ( expr ) stmt [ else stmt ]
        | while ( expr ) { stmt }
```

---

## 4️⃣ Derivation (String Kaise Derive Kare)

> **Derivation** = Start symbol se production rules apply karke string banana.

### Two Types:

**1. Leftmost Derivation (Baayen Se Shuru):**
Hamesha leftmost (sabse left wala) non-terminal ko replace karo

```
Example: Derive "id + id" from E

E ⟹ E + T       (E ko replace kiya)
  ⟹ T + T       (left E ko T se replace)
  ⟹ F + T       (left T ko F se replace)
  ⟹ id + T      (F ko id se replace)
  ⟹ id + F      (T ko F se replace)
  ⟹ id + id     (F ko id se replace)
```

**2. Rightmost Derivation (Daayen Se Shuru):**
Hamesha rightmost (sabse right wala) non-terminal ko replace karo

```
Example: Same string "id + id"

E ⟹ E + T       (E ko expand kiya)
  ⟹ E + F       (rightmost T ko replace)
  ⟹ E + id      (F ko id se replace)
  ⟹ T + id      (E ko T se replace)
  ⟹ F + id      (T ko F se replace)
  ⟹ id + id     (F ko id se replace)
```

**Important Note:**
- Dono derivations same string produce karenge
- Order alag hai, result same
- Parse tree bhi same hoga (for unambiguous grammar)

---

## 5️⃣ Parse Tree

> **Parse Tree** = Derivation ka tree (graphical) representation.

### Parse Tree for `id + id * id` using proper grammar:
```
              E
           /  |  \
          E   +   T
          |      / | \
          T     T  *  F
          |     |     |
          F     F     id
          |     |
          id    id
```

**Parse Tree Ki Properties:**
- Root = Start symbol (E)
- Internal nodes = Non-terminals (E, T, F)
- Leaf nodes = Terminals (id, +, *)
- Left to right leaves = final string

---

## 6️⃣ Ambiguous Grammar ⚠️ HIGH PRIORITY PYQ TOPIC

> **Ambiguous Grammar** = Ek hi string ke liye **2 ya zyada** different parse trees exist karte hain.

**Hinglish Samajh:**
- Jaise sentence "I saw a man with a telescope" — do meanings
  - Maine telescope se dekha
  - Maine ek aadmi dekha jiske paas telescope tha
- Similarly programming mein ambiguity problem hai

### 🔥 PYQ Example: `E → E+E | E*E | id`

**Question:** Check if `id + id * id` is ambiguous?

**Parse Tree 1** (+ pehle apply):
```
        E
      / | \
     E  +  E
     |    / | \
     id  E  *  E
         |     |
         id    id
```
**Meaning:** `id + (id * id)` — * pehle evaluate

**Parse Tree 2** (* pehle apply, structure different):
```
        E
      / | \
     E  *  E
    /|\     |
   E + E    id
   |   |
   id  id
```
**Meaning:** `(id + id) * id` — + pehle evaluate

**Conclusion:** Dono **alag parse trees** hain → Grammar **AMBIGUOUS** hai!

---

### Ambiguity Ko Kaise Fix Karein?

**Solution:** Operator **precedence** aur **associativity** explicitly define karo grammar mein.

**Unambiguous Grammar:**
```
E → E + T | T        (+ lowest precedence, left associative)
T → T * F | F        (* higher precedence, left associative)
F → id | ( E )       (Highest precedence — parentheses)
```

**Ab `id + id * id` ka sirf ek parse tree:**
```
          E
        / | \
       E  +  T
       |    / | \
       T   T  *  F
       |   |     |
       F   F     id
       |   |
       id  id
```

**Yeh Enforce Karta Hai:**
- `*` ka precedence `+` se higher
- Dono left-to-right associate
- Result: `(id + (id * id))` — correct!

---

## 7️⃣ Left Recursion

> **Left Recursion** = Production rule jisme left side ka symbol right side mein bhi **sabse pehle** aata hai.
> Form: `A → Aα | β`

**Problem Kyun Hai:**
- Top-down parsers (LL parsers) infinite loop mein fas jaate hain
- Left-most non-terminal ko hamesha A milta rahega

**Example:**
```
E → E + T | T    ← Left recursive (E right side mein pehle)
```

### Left Recursion Removal Algorithm:

```
Before: A → Aα | β
After:  A → βA'
        A'→ αA' | ε

Hinglish: Recursive part ko alag kar do with new non-terminal
```

**Detailed Example:**
```
Before (Left Recursive):
E → E + T | T

After (Removed):
E → TE'
E'→ +TE' | ε
```

**Verify:**
- `T` directly generate kar sakte
- `T + T` bhi: `E → TE' → T+TE' → T+Tε → T+T`
- No infinite loop!

---

## 8️⃣ Left Factoring

> **Left Factoring** = Jab do productions same prefix share karein, common part ko factor out karo.

**Problem:**
```
A → αβ | αγ    (Dono α se start — LL parser confused)
```

**Solution:**
```
A → αA'
A'→ β | γ
```

**Real Example:**
```
Before (Common Prefix):
stmt → if expr then stmt 
     | if expr then stmt else stmt

After (Left Factored):
stmt → if expr then stmt rest
rest → else stmt | ε
```

**Benefit:**
- Predictive parsing easier
- LL(1) grammar ban sakta hai
- Parser ko pata lag jata next kya expect karna hai

---

## 9️⃣ CFG Ki Capabilities

**CFG Kya Describe Kar Sakta Hai:**
1. **Nested structures** — matching parentheses `{a^n b^n}`
2. **Arithmetic expressions** with proper precedence
3. **Programming constructs** — if-else, loops, function calls
4. **Palindromes:** `w = reverse(w)`

**CFG Kya Describe NAHI Kar Sakta:**
1. `{a^n b^n c^n}` — yeh context-sensitive hai
2. **Semantic constraints** — type checking
3. **Declarations before use** — scope rules

**Hinglish Example:**
```
Can express: "Open { matched with close }"
Cannot express: "Variable 'x' must be declared before use"
```

---

## ✍️ Exam Answers (Hinglish)

---

### 📝 2 Marks — "Ambiguous grammar kya hai? Example do."

> Ek grammar ko **ambiguous** kaha jata hai agar language mein koi aisi string exist karti hai jiski **ek se zyada parse trees** (ya ek se zyada leftmost/rightmost derivations) hain. Ambiguous grammars compilers ke liye problematic hote hain kyunki woh program ki meaning ko unclear bana dete hain.
>
> **Example:** `E → E + E | E * E | id`
> String `id + id * id` ke liye do different parse trees ban sakti hain — ek jismein `+` pehle evaluate hota hai aur doosri jismein `*` pehle. Isliye yeh grammar ambiguous hai.

**English Translation:**
> A grammar is said to be **ambiguous** if there exists at least one string in the language that has **more than one parse tree** (or more than one leftmost/rightmost derivation). Ambiguous grammars are problematic for compilers because they make the meaning of a program unclear.
>
> **Example:** `E → E + E | E * E | id`
> For string `id + id * id`, two different parse trees exist — one where `+` is evaluated first and another where `*` is evaluated first. Hence this grammar is ambiguous.

---

### 📝 7 Marks — "Check karo ki grammar ambiguous hai ya nahi. Agar ambiguous hai to unambiguous mein convert karo: E → E+E | E*E | id"

**Step 1: Ambiguity Check Karo**

String consider karo: `id + id * id`

**Parse Tree 1** (+ pehle apply):
```
         E
       / | \
      E  +  E
      |    / | \
      id  E  *  E
          |     |
          id    id
```
**Meaning:** `id + (id * id)` — multiplication pehle ✓ (normally correct)

**Parse Tree 2** (* pehle apply, structure different):
```
         E
       / | \
      E  *  E
     /|\     |
    E + E    id
    |   |
    id  id
```
**Meaning:** `(id + id) * id` — addition pehle ✗ (wrong precedence)

**Kyunki two different parse trees exist karte hain** same string ke liye, grammar **AMBIGUOUS** hai.

---

**Step 2: Unambiguous Grammar Mein Convert Karo**

Ambiguity remove karne ke liye, hume enforce karna padega:
- `*` ka **precedence higher** hai `+` se
- Dono operators **left associative** hain

```
Unambiguous Grammar:

E → E + T | T        ← + ka rule (lower precedence)
T → T * F | F        ← * ka rule (higher precedence)
F → id | ( E )       ← Base cases
```

**Verification:** Parse karo `id + id * id` with new grammar:

```
Leftmost Derivation:

E ⟹ E + T
  ⟹ T + T
  ⟹ F + T
  ⟹ id + T
  ⟹ id + T * F
  ⟹ id + F * F
  ⟹ id + id * F
  ⟹ id + id * id
```

**Ab sirf EK parse tree possible hai:**
```
          E
        / | \
       E  +  T
       |    / | \
       T   T  *  F
       |   |     |
       F   F     id
       |   |
       id  id
```

**Yeh Tree Kya Represent Karta:**
- Pehle `id * id` evaluate hoga (subtree T * F)
- Phir result ko `id +` hoga
- Correct precedence: `id + (id * id)`

---

**Conclusion (Nishkarsh):**
Original grammar `E → E+E | E*E | id` ambiguous hai kyunki yeh operator precedence ya associativity specify nahi karta. Unambiguous version explicitly in rules ko grammar structure ke through encode karta hai, jo ensure karta hai ki har valid expression ke liye unique parse tree mile.

**Summary Table:**

| Aspect | Ambiguous | Unambiguous |
|--------|-----------|-------------|
| Productions | `E → E+E \| E*E \| id` | `E → E+T \| T`, `T → T*F \| F`, `F → id` |
| Parse Trees | Multiple for same string | Unique for each string |
| Precedence | Not defined | `*` > `+` |
| Usability | ❌ Not usable | ✅ Parser-ready |

---

## 🔑 Important Keywords Yaad Rakho
- CFG, BNF, EBNF, Grammar
- Derivation (Leftmost, Rightmost), Parse Tree
- Ambiguous Grammar, Multiple Parse Trees
- Left Recursion, Removal Algorithm
- Left Factoring, Common Prefix
- Operator Precedence, Associativity
- Non-terminal, Terminal, Production Rule
- Start Symbol, Sentential Form

---

---

# ✅ Unit 1 Complete Checklist

## Yaad Karne Wale Main Points:

### Phases (Topic 1):
- [ ] 6 phases yaad — Lexical, Syntax, Semantic, Intermediate, Optimization, Code Gen
- [ ] Code Optimization OPTIONAL kyun hai
- [ ] Bootstrapping ka concept
- [ ] Symbol Table aur Error Handler ka role

### FSM & Regex (Topic 2):
- [ ] NFA vs DFA difference
- [ ] Thompson's Construction (RE → NFA)
- [ ] Subset Construction (NFA → DFA)
- [ ] ε-closure concept
- [ ] Numerical practice: Given regex → NFA banao

### Lexical Analysis (Topic 3):
- [ ] Token, Lexeme, Pattern difference
- [ ] LEX program ki 3 parts
- [ ] Lexical vs Syntax errors

### CFG & Ambiguity (Topic 4):
- [ ] CFG ke 4 components
- [ ] BNF notation
- [ ] Ambiguous grammar detect karna
- [ ] Ambiguity fix karna (precedence/associativity)
- [ ] Left Recursion removal
- [ ] Left Factoring

---

## Practice Numericals (Zaroor Karo):

1. **NFA Construction:** `(a|b)*abb` ke liye NFA banao
2. **NFA to DFA:** Upar wale NFA ko DFA mein convert karo
3. **Ambiguity Check:** `E → E^E | E*E | E+E | id` check karo
4. **Ambiguity Fix:** Upar wale ko unambiguous banao
5. **Left Recursion:** `A → Aα | Aβ | γ` se remove karo

---

## 7 Marks Answers Practice Karo:

1. Compiler ke phases detail se
2. FSM aur Regular Expression ka relationship
3. Lexical + Syntax analysis with errors
4. Ambiguous grammar detect + fix

---

## PYQ Pattern (Last 3 Years):

- **2024:** Bootstrapping (2M), FSM (7M), Ambiguous grammar (7M)
- **2023:** Phases (2M), NFA to DFA (7M), LEX program (7M)
- **2022:** Optional phase (2M), Regex to NFA (7M), Grammar (7M)

---

> ✅ **Unit 1 Complete!**
> 🎯 **Confidence Check:** Agar upar ki checklist mein 80% tick kar sako, tum ready ho!
> 
> ➡️ **Next Unit:** Unit 2 — Parsing (SLR, CLR, LALR)

---

# 📚 Quick Revision Tables

## Compiler Phases Quick Reference

| Phase | Input | Output | Tool |
|-------|-------|--------|------|
| Lexical | Characters | Tokens | LEX |
| Syntax | Tokens | Parse Tree | YACC |
| Semantic | Parse Tree | Annotated Tree | — |
| Intermediate | Annotated Tree | 3AC | — |
| Optimization | 3AC | Optimized 3AC | — |
| Code Gen | 3AC | Machine Code | — |

## Token Types Quick Reference

| Type | Example | Pattern |
|------|---------|---------|
| Keyword | `if`, `while` | Exact match |
| Identifier | `count`, `sum` | `[a-z][a-z0-9]*` |
| Number | `42`, `3.14` | `[0-9]+(\.[0-9]+)?` |
| Operator | `+`, `==` | Fixed symbols |
| String | `"hello"` | `"[^"]*"` |

## Grammar Conversions Quick Reference

| From | To | Method |
|------|-----|--------|
| Regex | NFA | Thompson's Construction |
| NFA | DFA | Subset Construction |
| DFA | Minimal DFA | State Minimization |
| Left Recursive | Non-recursive | Elimination Algorithm |
| Common Prefix | Factored | Left Factoring |

---

**End of Unit 1 Complete Hinglish Notes**

**Best of Luck for Exam! 🎯**
**Mehnat Karo, Marks Aayenge! 💪**
