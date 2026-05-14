# 📗 UNIT 4 — Symbol Tables, Runtime & Error Handling (Hinglish Notes)
### Topics: Symbol Table, Data Structures, Activation Record, Stack/Heap, Scope, Errors

---

## 📋 Priority Table

| Topic | Priority | PYQ |
|-------|----------|-----|
| Symbol Table + Data Structures | ⭐⭐⭐ | Har saal Section C |
| Activation Record | ⭐⭐⭐ | Har saal Section B/C |
| Stack vs Heap Allocation | ⭐⭐⭐ | Har saal |
| Scope Information (Static/Dynamic) | ⭐⭐⭐ | 7 marks mein |
| Error Types (Lexical, Syntactic, Semantic) | ⭐⭐⭐ | Har saal |
| Error Recovery Methods | ⭐⭐ | Section B |

---

# 📘 Topic 1: Symbol Table & Data Structures

## 📌 Quick Keywords
> `Symbol Table` · `Hash Table` · `Scope` · `Identifier` · `Attribute` · `Hashing`

---

## 1️⃣ Symbol Table Kya Hai?
> **Symbol Table** = Data structure jo compiler maintain karta hai — program ke **identifiers** (variables, functions, types) ki information store karne ke liye.

```
┌───────────────────────────────────────────────────────┐
│                   SYMBOL TABLE                        │
├─────────────┬────────┬────────┬────────┬──────────────┤
│    Name     │  Type  │  Scope │  Size  │   Address    │
├─────────────┼────────┼────────┼────────┼──────────────┤
│ count       │  int   │ local  │  4     │  offset: 0   │
│ total       │ float  │ global │  8     │  0x1000      │
│ getSum      │  func  │ global │   —    │  0x2000      │
│ x           │  int   │ local  │  4     │  offset: 4   │
└─────────────┴────────┴────────┴────────┴──────────────┘
```

---

## 2️⃣ Symbol Table Operations
```
insert(name, type, scope, ...)  ← Naya identifier add karo
lookup(name)                    ← Identifier dhundho
delete(name)                    ← Scope end hone pe remove
update(name, attribute, value)  ← Attribute update karo
```

---

## 3️⃣ Data Structures for Symbol Table

### 1. Linear List (Array)
```
[id1 | info] [id2 | info] [id3 | info] ...
Pros:  Simple to implement
Cons:  O(n) search — slow for large programs
```

### 2. Hash Table ⭐ (Most Used)
```
hash(name) → bucket → chain of entries

"count"  → hash → bucket 3 → [count | int | local]
"total"  → hash → bucket 7 → [total | float | global]
"count2" → hash → bucket 3 → [count | int | local] → [count2 | int | local]  (chaining)

Pros:  O(1) average search
Cons:  Hash collisions
```

**Hash Function example:**
```
h(name) = (sum of ASCII values) mod table_size
h("count") = (99+111+117+110+116) mod 11 = 553 mod 11 = 3
```

### 3. Binary Search Tree
```
Pros:  O(log n) search if balanced
Cons:  Can become unbalanced
```

### 4. Stack of Hash Tables (Scoped Symbol Table)
```
┌─────────────────┐ ← Current scope (innermost)
│ x: int          │
│ y: float        │
├─────────────────┤
│ count: int      │ ← Outer scope
│ total: float    │
├─────────────────┤
│ main: func      │ ← Global scope
│ printf: func    │
└─────────────────┘
```

---

## 4️⃣ Scope Information
> **Scope** = Region where identifier is visible.

**Method 1: Scope Number** — Each identifier tagged with scope level.
```
int x;          → (x, int, scope=0)  [global]
void f() {
    int x;      → (x, int, scope=1)  [local]
    {
        int y;  → (y, int, scope=2)  [nested]
    }
}
```

**Method 2: Stack of Symbol Tables**
- New scope starts → Push new table
- Scope ends → Pop table
- Lookup → Search from top to bottom

---

## 5️⃣ Role of Symbol Table in Different Phases
```
Lexical Analysis:  Identifiers add karo (name only)
Syntax Analysis:   Type information add karo
Semantic Analysis: Type check karo — lookup se verify
Code Generation:   Address aur offset access karo
```

---

## ✍️ Exam Answers — Symbol Table

### 📝 7 Marks — "Define Symbol Table. Explain data structures used."
**Definition:**
A **Symbol Table** is a data structure maintained by the compiler to store information about identifiers. Every identifier is entered with attributes: name, type, scope, size, and memory address.

**Data Structures:**
1. **Linear List:** Simple array — O(n) search. Suitable for small programs.
2. **Hash Table (Most Efficient):** Hash function maps name to bucket. Chaining for collisions. O(1) average search.
3. **Binary Search Tree:** O(log n) search if balanced.
4. **Stack of Hash Tables:** Each scope has its own table. Push on entry, pop on exit. Correctly implements lexical scoping.

### 📝 2 Marks — "Explain role of hash table in symbol table"
> A **hash table** achieves O(1) average-case lookup. A hash function converts identifier name into bucket index. Collisions handled through **chaining**. This makes insertion, lookup, and deletion efficient.

---

# 📘 Topic 2: Runtime Storage & Activation Record

## 📌 Quick Keywords
> `Activation Record` · `Stack Allocation` · `Heap Allocation` · `Static Link` · `Dynamic Link`

---

## 1️⃣ Runtime Storage Organization
```
┌─────────────────────┐  High Address
│       STACK         │  ← Function calls, local variables
│   (grows down ↓)    │
├─────────────────────┤
│   (free space)      │
├─────────────────────┤
│       HEAP          │  ← Dynamic memory (malloc, new)
│   (grows up ↑)      │
├─────────────────────┤
│   Static/Global     │  ← Global variables
├─────────────────────┤
│   Code/Text         │  ← Program instructions
└─────────────────────┘  Low Address
```

---

## 2️⃣ Activation Record (Stack Frame)
> **Activation Record (AR)** = Ek function call ke liye memory block. Har function call ke liye ek AR stack pe push hoti hai.

```
┌───────────────────────────┐
│   Return Value            │  ← Caller ko return karne ki value
├───────────────────────────┤
│   Actual Parameters       │  ← Arguments passed by caller
├───────────────────────────┤
│   Optional Control Link   │  ← Static/Dynamic link
│   (Access Link)           │
├───────────────────────────┤
│   Saved Machine Status    │  ← Registers, PC backup
├───────────────────────────┤
│   Local Data              │  ← Local variables
├───────────────────────────┤
│   Temporaries             │  ← Compiler-generated temps
└───────────────────────────┘
```

| Field | Purpose |
|-------|---------|
| **Return Value** | Function ka result yahan store hota hai |
| **Actual Parameters** | Caller ke arguments |
| **Dynamic Link** | Previous AR ka pointer (caller's AR) |
| **Static Link** | Enclosing scope ka AR pointer (for nested functions) |
| **Saved Status** | Registers save karo, caller return ke baad restore ke liye |
| **Local Data** | Local variables of this function |
| **Temporaries** | Intermediate calculations |

---

## 3️⃣ Stack Allocation — How It Works
```
void main() {        ← AR1 push
    int a = 5;
    foo(a);
}
void foo(int x) {    ← AR2 push
    int b = x + 1;
    bar(b);
}
void bar(int y) {    ← AR3 push
    int c = y * 2;
}                    ← AR3 pop

Stack at bar():
┌────────────────┐  ← Stack Top
│   AR: bar      │  c=10, y=6, ret to foo
├────────────────┤
│   AR: foo      │  b=6, x=5, ret to main
├────────────────┤
│   AR: main     │  a=5
└────────────────┘  ← Stack Bottom
```

---

## 4️⃣ Stack vs Heap Allocation

| Feature | Stack Allocation | Heap Allocation |
|---------|-----------------|-----------------| 
| When | Compile time | Runtime |
| Size | Fixed at compile time | Variable (dynamic) |
| Speed | Fast (just pointer move) | Slower (allocation overhead) |
| Management | Automatic (LIFO) | Manual (malloc/free) or GC |
| Lifetime | Function call duration | Until explicitly freed |
| Examples | Local variables, params | `malloc()`, `new`, objects |
| Overflow | Stack Overflow | Heap Overflow / Fragmentation |

---

## 5️⃣ Static vs Dynamic Scope

| Scope | Definition | Determined |
|-------|-----------|-----------| 
| **Static (Lexical)** | Scope determined by program TEXT structure | At compile time |
| **Dynamic** | Scope determined by calling sequence at RUNTIME | At runtime |

```
int x = 1;
void f() { print(x); }   ← Which x?
void g() {
    int x = 2;
    f();
}

Static Scope:  f() prints 1  (x from global — where f is defined)
Dynamic Scope: f() prints 2  (x from g — most recent call)
```

---

## ✍️ Exam Answers — Runtime & Activation Record

### 📝 2 Marks — "What is Activation Record?"
> An **Activation Record** (stack frame) is a block of memory allocated on the runtime stack when a function is called. It stores: actual parameters, local variables, saved registers, dynamic and static links, and return value. When the function returns, its AR is popped from the stack.

### 📝 7 Marks — "Describe simple stack allocation scheme"
**Introduction:**
Stack allocation uses a runtime stack for function activations in LIFO manner.

**AR Structure:** Return value, actual parameters, dynamic link, static link, saved status, local variables, temporaries.

**How it works:**
1. **Call:** Caller pushes params → New AR created → Dynamic link set → Control transfers
2. **During execution:** Local variables allocated in current AR
3. **Return:** Return value stored → AR popped → Control returns to caller

**Advantages:** Automatic memory management, fast allocation, supports recursion.
**Limitation:** Cannot handle dynamic-size data (use heap for that).

### 📝 2 Marks — "How is scope information represented in symbol table?"
> Scope is represented by **scope level numbers** or a **stack of symbol tables**. Each scope has its own table, pushed on entry and popped on exit. Lookup searches from innermost to outermost scope, implementing lexical scoping where inner declarations shadow outer ones.

---

# 📘 Topic 3: Error Detection & Recovery

## 📌 Quick Keywords
> `Lexical Error` · `Syntactic Error` · `Semantic Error` · `Panic Mode` · `Phrase Level` · `Error Productions`

---

## 1️⃣ Types of Errors in Compilation
```
┌─────────────────────────────────────────────────────┐
│              COMPILATION ERRORS                      │
├───────────────┬─────────────────┬───────────────────┤
│ LEXICAL       │ SYNTACTIC       │ SEMANTIC           │
│ (Phase 1)     │ (Phase 2)       │ (Phase 3)          │
├───────────────┼─────────────────┼───────────────────┤
│ Illegal chars │ Grammar rules   │ Meaning errors     │
│ Bad tokens    │ violated        │ Type mismatches    │
└───────────────┴─────────────────┴───────────────────┘
```

---

## 2️⃣ Lexical Phase Errors
```
@x = 5          ← '@' is illegal character
123abc          ← malformed number token
"hello          ← unterminated string literal
```

**Recovery:** Delete illegal char, Insert missing char, Replace, Skip

---

## 3️⃣ Syntactic Phase Errors
```
int x = 5         ← missing semicolon
if x > 0          ← missing parentheses
((a + b)          ← unmatched parenthesis
```

### Recovery Methods:
1. **Panic Mode** ← Most Common: Skip tokens until synchronizing token (`;`, `}`) found
2. **Phrase Level:** Local corrections — insert/delete/replace token
3. **Error Productions:** Add common error rules to grammar
4. **Global Correction:** Minimum edit distance (theoretical — too expensive)

---

## 4️⃣ Semantic Errors
```
1. TYPE ERRORS:        int x = "hello"
2. UNDECLARED:         y = a + b;  (a never declared)
3. SCOPE ERRORS:       { int x = 5; }  y = x + 1;  (x out of scope)
4. MULTIPLE DECL:      int x; float x;
5. WRONG ARG COUNT:    foo(1, 2, 3) when foo takes 2
6. WRONG RETURN:       int bar() { return "hi"; }
```

**Detection:** Via Symbol Table lookups, type checking, scope checking

---

## 5️⃣ Error Recovery Comparison

| Error Type | Phase | Example | Recovery |
|-----------|-------|---------|----------|
| Lexical | Scanner | `@x = 5` | Delete illegal char |
| Syntactic | Parser | missing `;` | Panic mode / phrase level |
| Semantic | Semantic Analyzer | type mismatch | Report + continue |
| Logical | None | Wrong algorithm | Not by compiler |
| Runtime | Execution | Division by zero | At runtime |

---

## ✍️ Exam Answers — Errors

### 📝 7 Marks — "Explain semantic errors and challenges in detecting them"
**Types:** Type errors, undeclared identifiers, multiple declarations, wrong argument count, return type mismatch, scope errors.

**Challenges:**
1. **Context Dependency:** Cannot detect by pattern matching — need full program state
2. **Type Coercion Ambiguity:** `int x; float y; y = x + 1.5;` — valid coercion vs error?
3. **Overloaded Operators:** `a + b` meaning depends on types
4. **Dynamic vs Static Types:** In Python/JS, many errors only at runtime

**Handling:** Report and continue (assume correct type), Symbol Table verification, Type inference.

### 📝 7 Marks — "Explain different types of errors with examples"
1. **Lexical:** `@x = 5` — illegal char. Recovery: Skip
2. **Syntax:** `int x = 5` (no `;`). Recovery: Panic mode
3. **Semantic:** `int x = "hello"` — type mismatch. Recovery: Assume type, continue
4. **Logical:** `if (x = 5)` instead of `==`. Not detected by compiler
5. **Runtime:** Division by zero. Detected at runtime only

---

## 🔑 Important Keywords — Unit 4
- Symbol Table, Hash Table, Hash Function, Chaining
- Scope, Scope Number, Stack of Tables
- Activation Record, Stack Frame, Dynamic/Static Link
- Stack Allocation, Heap Allocation, LIFO
- Lexical/Syntactic/Semantic Error
- Panic Mode, Phrase Level, Error Productions

---

## ✅ Unit 4 Complete! ✅
