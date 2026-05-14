# 📗 UNIT 5 — Code Generation & Optimization (Hinglish Notes)
### Topics: Code Generator, Basic Blocks, Flow Graphs, DAG, Optimization, Data-Flow Analysis

---

## 📋 Priority Table

| Topic | Priority | PYQ |
|-------|----------|-----|
| Code Generator Design Issues | ⭐⭐⭐ | Har saal 2 marks + 7 marks |
| Basic Blocks + Flow Graphs | ⭐⭐⭐ | Har saal |
| DAG Representation | ⭐⭐⭐ | Har saal numerical |
| Common Sub-expression Elimination | ⭐⭐⭐ | Har saal |
| Loop Optimization | ⭐⭐⭐ | 7 marks mein |
| Machine-Independent Optimization | ⭐⭐ | Section A mein |
| Global Data-Flow Analysis | ⭐⭐⭐ | Section A + B |

---

# 📘 Topic 1: Code Generation Basics

## 📌 Quick Keywords
> `Code Generator` · `Basic Block` · `Flow Graph` · `DAG` · `Common Sub-expression` · `Loop Optimization` · `Data-Flow Analysis`

---

## 1️⃣ Code Generator Kya Karta Hai?
> **Code Generator** = Intermediate code (3AC) ko **target machine code** mein translate karta hai.

```
Intermediate Code (3AC)
        ↓
   [ CODE GENERATOR ]
        ↓
Target Code (Assembly / Machine Code)
```

---

## 2️⃣ Code Generator — Design Issues ⭐
> **PYQ:** "Discuss design issues in code generation" — Har saal aata hai!

**1. Input to Code Generator:**
- Intermediate representation (3AC, DAG)
- Symbol table (variable types, addresses)
- Must be error-free

**2. Target Program:**
- Absolute machine code — directly executable
- Relocatable machine code — linked later
- Assembly code — human readable

**3. Memory Management:**
- Mapping names to addresses
- Register vs memory allocation decisions

**4. Instruction Selection:**
- Multiple target instructions may implement same operation
- Choose efficient instruction
- Example: `x = x + 1` → use `INC x` instead of `MOV R, x; ADD R, 1; MOV x, R`

**5. Register Allocation ← Most Important Issue**
- Limited registers available
- Decide which values to keep in registers, which to spill to memory
- Sub-problem: **Register Assignment** — which specific register for which variable

**6. Evaluation Order:**
- Order of evaluation affects efficiency
- Some orders require fewer registers

---

# 📘 Topic 2: Basic Blocks & Flow Graphs

## 1️⃣ Basic Block — Definition
> **Basic Block** = Maximum sequence of 3AC instructions jisme:
> - **Ek hi entry point** hai (sirf pehli instruction pe jump ho sakta hai bahar se)
> - **Ek hi exit point** hai (sirf aakhri instruction jump/branch kar sakti hai)

### How to Find Basic Blocks:
**Step 1 — Find Leaders:**
1. Pehli instruction hamesha leader hoti hai
2. Kisi bhi jump ki **target** instruction leader hai
3. Jump ke **immediately baad** wali instruction leader hai

**Step 2:** Har leader se agle leader tak = ek basic block

### Example:
```
3AC:
(1)  i = 1               ← Leader → B1
(2)  j = 1
(3)  t1 = 10 * i         ← Leader (target of 11) → B2
(4)  t2 = t1 + j         ← Leader (target of 9) → B3
(5)  t3 = 8 * t2
(6)  t4 = t3 - 88
(7)  a[t4] = 0.0
(8)  j = j + 1
(9)  if j <= 10 goto (4)
(10) i = i + 1            ← Leader (after jump 9) → B4
(11) if i <= 10 goto (3)
(12) i = 1                ← Leader (after jump 11) → B5
...
```

**Basic Blocks:** B1: (1)-(2), B2: (3), B3: (4)-(9), B4: (10)-(11), B5: (12)...

---

## 2️⃣ Flow Graph
> **Flow Graph** = Basic blocks ko nodes maano, control flow ko edges.

```
        B1
        │
        ▼
        B2 ◄────────┐
        │           │
        ▼           │
        B3 ◄──┐    │
        │     │    │
        ├─────┘    │  (j loop back edge)
        ▼          │
        B4 ────────┘  (i loop back edge)
        │
        ▼
        B5
```

**Back Edge** = Edge from descendant to ancestor → indicates a **LOOP**

---

# 📘 Topic 3: DAG (Directed Acyclic Graph)

## 1️⃣ DAG Kya Hai?
> **DAG** = Basic block ka graph representation.
> - **Leaves** = Variables, constants
> - **Interior nodes** = Operators
> - **Shared nodes** = Common sub-expressions

## 2️⃣ DAG Construction Algorithm
For each statement `x = y op z`:
1. If node for `y` doesn't exist → create leaf node
2. If node for `z` doesn't exist → create leaf node
3. Check if node for `(op, y, z)` exists → if yes, reuse it (CSE!)
4. If no → create new interior node
5. Attach label `x` to this node

### Example — Simple DAG:
```
Basic Block:
1. x = a + b
2. y = c - d
3. z = x * y

DAG:
        *       ← z
       / \
      +   -     ← x, y
     / \ / \
    a  b c  d
```

### Example — Common Sub-expression Elimination with DAG:
```
1. t1 = a + b
2. t2 = a + b    ← SAME as t1!
3. t3 = t1 * t2

DAG Construction:
- t1 = a+b → create (+, a, b), label = t1
- t2 = a+b → node (+,a,b) already EXISTS → label t2 to SAME node
- t3 = t1*t2 → t1 = t2 = same node → t3 = t1 * t1

     *     ← t3
    / \
   +   +   ← SAME NODE (t1 and t2 both)
  / \
 a   b

Optimized Code:
t1 = a + b       ← computed once
t3 = t1 * t1     ← reuse t1, no need for t2!
```

## 3️⃣ Value Numbers
> Each unique expression gets a unique number. Same value number → same result → reuse!

```
val(a) = 1, val(b) = 2
t1 = a + b → val(t1) = hash(+, 1, 2) = 5
t2 = a + b → val(t2) = hash(+, 1, 2) = 5  ← SAME! → t2 = t1
```

---

# 📘 Topic 4: Code Optimization Techniques

## 1️⃣ Classification
```
CODE OPTIMIZATIONS
├── Machine-Independent
│   ├── Local (within basic block)
│   │   ├── Common Sub-expression Elimination
│   │   ├── Dead Code Elimination
│   │   ├── Constant Folding
│   │   └── Copy Propagation
│   └── Global (across basic blocks)
│       ├── Loop Optimization
│       └── Global Data-Flow Analysis
└── Machine-Dependent
    ├── Register Allocation
    ├── Peephole Optimization
    └── Instruction Scheduling
```

---

## 2️⃣ Local Optimizations

### Common Sub-expression Elimination (CSE)
```
Before: t1 = a + b          After: t1 = a + b
        t2 = a + b    →            t2 = t1       ← reuse
```

### Copy Propagation
```
Before: x = t1              After: y = t1 + 1   ← replace x
        y = x + 1
```

### Dead Code Elimination
```
Before: x = 5               After: x = 10       ← first removed
        x = 10                     use(x)
        use(x)
```

### Constant Folding
```
Before: x = 3 * 4           After: x = 12       ← compile time
```

### Algebraic Simplification
```
x = y + 0  →  x = y
x = y * 1  →  x = y
x = y * 0  →  x = 0
```

---

## 3️⃣ Loop Optimizations (Important!)

### Code Motion (Loop Invariant Removal)
> Move computations that **don't change** outside the loop.
```
Before:                          After:
for (i=0; i<n; i++) {            x = a * b;  ← moved outside
    x = a * b;                   for (i=0; i<n; i++) {
    arr[i] = x + i;                 arr[i] = x + i;
}                                }
```

### Strength Reduction
> Replace expensive operation with cheaper equivalent.
```
Before: t = i * 4        After: t = t + 4  (+ instead of *)
```

| Expensive | Cheaper |
|-----------|---------|
| `x * 2` | `x + x` or `x << 1` |
| `x ** 2` | `x * x` |
| `i * constant` | accumulated addition |

### Induction Variable Elimination
```
for (i=0; i<10; i++) {     →    t = 0;
    t = 4 * i;                   while (t < 40) {
    a[t] = 0;                        a[t] = 0;
}                                    t = t + 4;
                                 }
                                 // i completely eliminated!
```

### Loop Unrolling
```
Before: for (i=0; i<4; i++) { a[i] = 0; }
After:  a[0]=0; a[1]=0; a[2]=0; a[3]=0;  ← no loop overhead
```

---

## 4️⃣ Peephole Optimization (Machine-Dependent)
> Small window of target instructions examine karo, better sequence se replace karo.

```
MOV R0, x    →   MOV R0, x      (second MOV removed — redundant)
MOV x, R0

ADD R0, 0    →   (removed — no-op)

MOV R0, R0   →   (removed — self-move)
```

---

# 📘 Topic 5: Global Data-Flow Analysis

## 1️⃣ Kya Hai?
> **Data-Flow Analysis** = Analyze how data (values) flow through the **entire program** across basic blocks.

## 2️⃣ Reaching Definitions
> Definition d of variable x **reaches** point p if there's a path from d to p with no other definition of x.

**Data-Flow Equations:**
```
For basic block B:
gen[B]  = {definitions in B that reach end of B}
kill[B] = {all other definitions of same variables}

IN[B]  = ∪ OUT[predecessors of B]
OUT[B] = gen[B] ∪ (IN[B] - kill[B])
```

**Algorithm:** Iterative fixed-point computation
```
Initialize: OUT[B] = ∅ for all B
Repeat until no change:
    For each block B:
        IN[B]  = ∪ OUT[P] for all predecessors P
        OUT[B] = gen[B] ∪ (IN[B] - kill[B])
```

## 3️⃣ Live Variable Analysis
> Variable x is **live** at point p if its value may be used before it is redefined.

**Use:** Register allocation (keep live vars in registers), dead code detection

**Equations (backward analysis):**
```
use[B]  = {variables used in B before any redefinition}
def[B]  = {variables defined in B before any use}

OUT[B] = ∪ IN[S] for all successors S
IN[B]  = use[B] ∪ (OUT[B] - def[B])
```

---

## ✍️ Exam Answers — Unit 5

### 📝 2 Marks — "Discuss two design issues in code generation"
> 1. **Register Allocation:** Decide which values to keep in limited registers. Variables in registers are faster than memory. Register assignment = which specific register.
> 2. **Instruction Selection:** Multiple instructions may implement same operation. Choose most efficient — e.g., `INC x` instead of `MOV R,x; ADD R,1; MOV x,R`.

### 📝 2 Marks — "What is machine-independent code optimization?"
> Transformations applied to intermediate code that improve efficiency regardless of target machine. Examples: common sub-expression elimination, constant folding, dead code elimination, copy propagation, and loop optimizations (code motion, strength reduction, induction variable elimination).

### 📝 2 Marks — "Explain concept of global data-flow analysis"
> **Global data-flow analysis** examines how data values flow across ALL basic blocks using iterative equations (gen/kill or use/def sets). Two common analyses: **Reaching Definitions** (which assignments reach a point) and **Live Variables** (which variables may be used before redefinition). Enables global optimizations and register allocation. Converges to fixed point.

### 📝 7 Marks — "Explain optimization techniques + DAG for CSE"
**Principle Sources:**
1. **CSE:** Compute same expression once, reuse. `t1 = a+b; t2 = a+b → t2 = t1`
2. **Copy Propagation:** `x = t1; y = x+1 → y = t1+1`
3. **Dead Code Elimination:** Remove unused assignments
4. **Constant Folding:** `x = 3*4+2 → x = 14`
5. **Loop Optimizations:** Code motion, induction variable, strength reduction, unrolling

**DAG for CSE:**
```
Basic Block:                    DAG:
t1 = a + b                              -       ← t4
t2 = c * d                             / \
t3 = a + b  ← same as t1!             *   +     ← t2, t3/t1 (SHARED!)
t4 = t2 - t3                         / \ / \
                                     c  d a  b

Optimized: t1 = a+b; t2 = c*d; t4 = t2-t1  (t3 eliminated!)
```

### 📝 7 Marks — "Discuss optimization techniques at basic block level"
1. **CSE:** `t1 = a+b; t2 = a+b → t2 = t1`
2. **Dead Code:** `x=5; x=10; use(x) → x=10; use(x)`
3. **Constant Folding:** `x = 2*3+4 → x = 10`
4. **Copy Propagation:** `x=t1; y=x+1 → y=t1+1`
5. **Algebraic Simplification:** `x = y*1 → x = y`

**DAG automatically applies all these:** Shared nodes = CSE, No parent = Dead code, Constant leaves = Constant folding.

---

## 🔑 Important Keywords — Unit 5
- Code Generator, Target Code, Register Allocation, Instruction Selection
- Basic Block, Leader, Flow Graph, Back Edge, Loop
- DAG, Value Numbers, Shared Node
- Common Sub-expression Elimination, Dead Code, Copy Propagation
- Loop Optimization: Code Motion, Induction Variable, Strength Reduction
- Global Data-Flow Analysis, Reaching Definitions, Live Variables
- gen[], kill[], IN[], OUT[], Fixed-Point Iteration
- Peephole Optimization

---

## 📋 Quick Revision — ALL PYQ Answers Brief

| Question Type | Key Points |
|--------------|-----------| 
| Phases of Compiler | 6 phases + Symbol Table + Error Handler |
| Bootstrapping | 3-step self-hosting process |
| FSM vs RE | Thompson's + Subset Construction |
| Ambiguous Grammar | 2 parse trees → fix with precedence grammar |
| Symbol Table | Hash table — O(1) lookup |
| Activation Record | Stack frame with params, locals, links |
| Stack vs Heap | Automatic vs manual, fixed vs dynamic |
| Basic Block | Max straight-line code, find leaders |
| DAG | Shared nodes = CSE |
| Loop Optimization | Code motion, induction var, unrolling |

---

## ✅ Unit 5 Complete! Poora Compiler Design Subject Complete! 🎉
