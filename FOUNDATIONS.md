# Epistemic World — Foundations

**Version:** 0.1
**Status:** Draft

## 1. 目的

Epistemic World 是一個以 TypeScript 建立的研究型框架，用來探索：

> **當一個 Observer 身處於一個世界之中，而且只能取得有限資訊時，它能知道什麼？**

我們希望把原本屬於哲學、邏輯與認知科學的問題，轉化成可以由電腦建立、執行與實驗的模型。

本文件只定義目前最基本的概念。

---

## 2. World

**World（世界）**是我們正在研究的環境。

一個 World 至少包含：

* 當前狀態（State）
* 存在於其中的 Agent
* Agent 可以取得的資訊
* 隨時間發生的變化

例如：

```text
World
├── rain = true
├── door = closed
└── treasure = true
```

World 的真實狀態代表：

> **在這個模型中，事情實際上是什麼樣子。**

---

## 3. Agent

**Agent（代理人）**是存在於 World 中、能夠取得資訊並形成自身認知狀態的實體。

Agent 不必知道 World 的完整狀態。

例如：

```text
World

rain = true
treasure = true

Alice
└── observes: rain

Bob
└── observes: nothing
```

因此：

```text
World 的狀態
    ≠
Agent 所知道的狀態
```

這個區別是本專案的核心。

---

## 4. Observation

**Observation（觀察）**是 Agent 從 World 取得的資訊。

例如：

```text
World:
    rain = true

Alice observes:
    rain = true
```

但：

```text
Observation ≠ Knowledge
```

因為「看到了某個資訊」與「知道某個命題為真」是不同概念。

我們會在後續模型中定義兩者之間的關係。

---

## 5. Proposition

**Proposition（命題）**是一個可以判斷真假的陳述。

例如：

```text
rain
door is closed
treasure exists
```

在最簡單的情況下：

```text
rain = true
```

就是一個命題。

未來可能支援更複雜的邏輯表示：

```text
¬p
p ∧ q
p ∨ q
p → q
```

以及與 Agent Knowledge 有關的命題：

```text
K_A(p)
```

---

## 6. Knowledge

**Knowledge（知識）**表示某個 Agent 知道某個命題。

我們使用：

```text
K_A(p)
```

表示：

> Agent A knows proposition p.

例如：

```text
K_A(rain)
```

表示 Alice 知道正在下雨。

Knowledge 是 **agent-relative** 的：

```text
K_A(p)
```

並不代表：

```text
K_B(p)
```

---

## 7. Knowledge 與 Truth

在本專案目前的基礎模型中：

> Knowledge 必須與 World 的真實狀態一致。

因此：

```text
K_A(p) → p
```

如果 Alice 知道 `rain`，那麼在目前模型中 `rain` 必須是真的。

這使得 Knowledge 與未來的 **Belief（信念）**有所區別。

例如：

```text
World:
    rain = false

Alice believes:
    rain = true
```

這可以成立：

```text
B_A(rain)
```

但不能成立：

```text
K_A(rain)
```

Belief 暫時不屬於 v0.1 的實作範圍。

---

## 8. Possible World

Agent 通常無法直接看到 World 的完整狀態。

因此，從 Agent 的角度來看，可能存在多個它認為「有可能是真實世界」的狀態。

這些稱為 **Possible Worlds（可能世界）**。

例如：

```text
World A
    treasure = true

World B
    treasure = false
```

Alice 看不到箱子裡面。

因此對 Alice 而言：

```text
Possible Worlds(Alice)
=
{ World A, World B }
```

Alice 無法確定 treasure 是否存在。

---

## 9. Indistinguishability

如果 Agent 無法根據目前可取得的資訊區分兩個 World，我們稱這兩個 World 對該 Agent 是 **indistinguishable（不可區分）** 的。

表示為：

```text
A ~ₐ B
```

意思是：

> Agent A 無法區分 World A 與 World B。

例如：

```text
World A                 World B

treasure = true         treasure = false
box = closed            box = closed

        ↓                    ↓
        └──── Alice ─────────┘

Alice 只能看到：
box = closed
```

因此：

```text
World A ~Alice World B
```

Alice 無法從目前的觀察判斷 treasure 的真實狀態。

---

## 10. Knowledge 與 Possible Worlds

在目前的模型中，一個直觀的定義是：

> Agent 知道命題 p，代表 p 在 Agent 所認為可能的所有 World 中都成立。

也就是：

```text
K_A(p)
```

當且僅當：

```text
所有 Alice 認為可能的 World
都滿足 p
```

例如：

```text
Possible Worlds(Alice):

World A:
    treasure = true

World B:
    treasure = false
```

因為存在一個 Alice 認為可能、但 `treasure = false` 的 World：

```text
¬K_A(treasure)
```

如果 Alice 之後觀察到：

```text
treasure = true
```

那麼：

```text
Possible Worlds(Alice):

World A
```

因此：

```text
K_A(treasure)
```

成立。

這建立了本專案第一個重要關係：

```text
Observation
      ↓
Possible Worlds
      ↓
Knowledge
```

---

## 11. 第一個研究問題

Epistemic World 的第一個核心問題是：

> **什麼情況下，兩個不同的 World 對一個 Agent 而言是不可區分的？**

我們會從這個問題開始建立：

```text
Observation
        ↓
Indistinguishability
        ↓
Possible Worlds
        ↓
Knowledge
```

之後再逐步研究：

```text
Knowledge
    ↓
Knowledge about Knowledge
    ↓
Dynamic Information
    ↓
Multi-Agent Reasoning
```

---

## 12. v0.1 的範圍

v0.1 只關注：

```text
World
Agent
State
Proposition
Observation
Knowledge
Possible World
Indistinguishability
```

暫時不實作：

```text
Belief
Memory
Common Knowledge
Dynamic Epistemic Logic
AI / LLM
Machine Learning
Game Theory
Evolution
Physical Simulation
Visualization
```

這些都是未來可能建立在基礎模型之上的方向。

---

## 13. 第一個 Computational Experiment

我們的第一個實驗將建立兩個世界：

```text
World A
    treasure = true
    box = closed

World B
    treasure = false
    box = closed
```

Alice 只能觀察：

```text
box = closed
```

因此：

```text
World A ~Alice World B
```

Alice 不知道：

```text
treasure
```

接著讓 Alice 取得新的 Observation：

```text
treasure = true
```

系統應該更新 Alice 的 Possible Worlds：

```text
Before:

{ World A, World B }


After:

{ World A }
```

因此：

```text
Before:
¬K_A(treasure)

After:
K_A(treasure)
```

這將成為 Epistemic World 第一個正式的驗證實驗。

---

## 14. 核心原則

目前我們只遵守一個最重要的原則：

> **先定義我們要研究的概念，再決定程式應該怎麼寫。**

因此：

```text
Theory
   ↓
Model
   ↓
Implementation
   ↓
Experiment
```

而不是：

```text
Code
   ↓
找一個理由解釋 Code
```

v0.1 的目標不是建立一個完整的 Epistemic Logic framework。

目標只是建立一個：

> **足夠小、足夠清楚，而且可以被電腦驗證的第一個模型。**
