# 📗 UNIT 3 — Syntax-Directed Translation (Hinglish Notes)
### Topics: SDT, Attributes, Intermediate Code, 3AC, Quadruples, Triples

---

## 📋 Priority Table

| Topic | Priority | PYQ |
|-------|----------|-----|
| Synthesized vs Inherited Attributes | ⭐⭐⭐ | Har saal 2 marks |
| Three Address Code (3AC) | ⭐⭐⭐ | Har saal numerical |
| Quadruples & Triples | ⭐⭐⭐ | Har saal numerical |
| SDT Schemes | ⭐⭐⭐ | 7 marks mein |
| Back Patching | ⭐⭐ | Kabhi kabhi |

---

# 📘 Topic 1: SDT Schemes & Attributes

## 📌 Quick Keywords
> `SDD` · `SDT` · `Synthesized` · `Inherited` · `Attribute Grammar`

## 1️⃣ SDD & SDT Kya Hai?
> **SDD (Syntax-Directed Definition)** = CFG + semantic rules.
> **SDT (Syntax-Directed Translation)** = Actions enclosed in `{ }` embedded within grammar.

## 2️⃣ Synthesized vs Inherited Attributes 🔥
> **PYQ:** "What are two types of attributes?"

| Feature | Synthesized Attributes | Inherited Attributes |
|---------|------------------------|----------------------|
| Direction | **Bottom-Up** (child se parent) | **Top-Down** (parent/sibling se child) |
| Example | `E.val = E1.val + T.val` | Data type passing down tree |
| SDD Type | S-attributed SDD | L-attributed SDD |

---

# 📘 Topic 2: Intermediate Code & 3AC

## 📌 Quick Keywords
> `Three Address Code` · `Quadruple` · `Triple` · `Indirect Triple` · `Postfix`

## 1️⃣ Intermediate Code Kya Hai?
> Machine-independent code (parse tree aur target code ke beech). Optimization easy karta hai.

## 2️⃣ Three Address Code (3AC)
> At most **3 addresses** wala statement.
> Types: `x = y op z`, `x = op y`, `x = y`, `goto L`, `if x < y goto L`

## 3️⃣ Postfix Notation
> Operator operands ke **baad** aata hai. Example: `a + b * c` → `a b c * +`

---

# 📘 Topic 3: Quadruples, Triples & Indirect Triples 🔥

## 1️⃣ Differences
| Feature | Quadruple | Triple | Indirect Triple |
|---------|-----------|--------|-----------------|
| Fields | 4 (op, arg1, arg2, result) | 3 (op, arg1, arg2) | 3 + listing array |
| Temp vars | Explicit (`t1`) | Implicit `(0)` position | Implicit |
| Optimization| Easy | Hard | Easy (modify listing) |

## 2️⃣ Example: `a = b * -c + b * -c`

**3AC:**
```
t1 = -c
t2 = b * t1
t3 = -c
t4 = b * t3
t5 = t2 + t4
a  = t5
```

**Quadruple Table:**
| # | op | arg1 | arg2 | result |
|---|----|----|----|----|
| 0 | uminus | c | — | t1 |
| 1 | * | b | t1 | t2 |
| 2 | uminus | c | — | t3 |
| 3 | * | b | t3 | t4 |
| 4 | + | t2 | t4 | t5 |
| 5 | = | t5 | — | a |

**Triple Table:**
| # | op | arg1 | arg2 |
|---|----|----|---|
| 0 | uminus | c | — |
| 1 | * | b | (0) |
| 2 | uminus | c | — |
| 3 | * | b | (2) |
| 4 | + | (1) | (3) |
| 5 | = | a | (4) |

---

# 📘 Topic 4: Translation of Control Flow & Back Patching

## 1️⃣ Boolean Expressions (Short Circuit Code)
```
if a < b OR c > d

3AC using jumps:
    if a < b goto Ltrue
    if c > d goto Ltrue
    t = 0
    goto Lnext
Ltrue: t = 1
Lnext: ...
```

## 2️⃣ WHILE Statement Translation
```c
while (a < b) { x = x + 1; }

3AC:
L1: if a < b goto L2
    goto L3
L2: t1 = x + 1
    x = t1
    goto L1
L3: (continue)
```

## 3️⃣ Back Patching
> Incomplete jumps ke addresses ko baad mein fill karna.
- `makelist(i)`: List banata hai
- `merge(L1, L2)`: Lists combine karta hai
- `backpatch(L, addr)`: Addresses fill karta hai `truelist` aur `falselist` mein.

---

## ✍️ Exam Answers — Unit 3

### 📝 2 Marks — "Difference between inherited and synthesized attributes"
> **Synthesized Attributes** get values from their children (Bottom-Up flow). Example: evaluating math expressions.
> **Inherited Attributes** get values from their parent or siblings (Top-Down flow). Example: passing variable type to identifiers.

### 📝 2 Marks — "What is postfix notation?"
> **Postfix notation** (Reverse Polish Notation) places the operator after the operands. It doesn't need parentheses to show order of operations. Example: `(a+b)*c` becomes `a b + c *`. It's easily evaluated using a stack.

### 📝 7 Marks — "Explain Quadruples and Triples in SDT"
> They are methods to represent Three Address Code.
> **Quadruples** have 4 fields: `(op, arg1, arg2, result)`. They use explicit temporary variables which makes optimization (like moving statements) very easy.
> **Triples** have 3 fields: `(op, arg1, arg2)`. The result is implicitly referred to by the statement's position number. They save space but are hard to optimize.
> **Indirect Triples** use a separate listing array pointing to triples, allowing easy optimization by just reordering the array.

---
## ✅ Unit 3 Complete! ✅
