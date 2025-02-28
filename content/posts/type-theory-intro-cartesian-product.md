+++
date = '2025-02-28T13:45:53+08:00'
title = '初识类型论：以笛卡尔积为例 [WIP]'
categories = ["Mathematics"]
tags = ["Set", "Type", "Category"]
+++

非常感谢一位朋友推荐的类型论入门课程[MAT-FORMATH by *Andrej Bauer*](https://www.youtube.com/playlist?list=PL-47DDuiZOMDBfb5t8Hd30utd_TopoQLE)，其中第一集([2024-10-04 Type theory](https://www.youtube.com/watch?v=OEGXNEPddYw&t=2305s))以**笛卡尔积**为例子，通俗易懂地讲解了类型论、集合论、范畴论的微妙关联。

也非常感谢这位朋友之前推荐的范畴论入门书籍**The Joy of Abstraction** by *Eugenia Cheng* 以及网站 [nLab](https://ncatlab.org/nlab/show/HomePage)。

以上就是本文的主要参考来源。感谢这些数学工作者的无私分享，他们没有故意建起知识的护城河，而是为初学者打破了第一道信息壁垒。

本文内容为自己的初学思考笔记，并不足够严谨，仅供参考。

<!--more-->

---

我们**通常**这样定义笛卡尔积[^1]：

设\(A\)和\(B\)为两个集合。**笛卡尔积（cartesian product）** 被定义为集合：

\[ P = \{(a, b) \mid a \in A, \, b \in B\} \]

其中的元素 \((a, b)\) 称为**有序对（ordered pair）**。\( P \) 通常被记作 \( A \times B \)。

此外要注意到，当笛卡尔积的定义写下，就已经暗示了集合 \( P \) 配有两个自然**投影（projections）** \(\pi_1\)和\(\pi_2\)：\( \pi_1: A \times B \to A\)，\(\pi_2: A \times B \to B \)；其中， \(\pi_1(a, b) = a\)，\(\pi_2(a, b) = b\)。

---

以上是笛卡尔积的通常定义，如果要写成纯粹的**一阶逻辑(First-Order Logic)** 形式，则是[^2]：

\[ \forall x \, ( x \in A \times B \leftrightarrow \exists a \, \exists b \, (a \in A \land b \in B \land x = \{\{a\}, \{a, b\}\})) \]

---

以上是经典**集合论**和**数理逻辑**思维下的定义。
但这样定义还不够自然——尤其是在使用计算机实现辅助证明时。因此，我们需要引入**类型论**，作为一种新的数学形式化（formalism）方法。

下面我们先来看看**类型论**怎样表示笛卡尔积的定义：

1. **初始化（Initialization）**[^3]

\[ \frac{\vdash A : Type \quad \vdash B : Type}{\vdash P : Type}\]

2. **配对（Pairing）**

\[ \frac{\vdash a : A \quad \vdash b : B}{\vdash (a,b) : P} \]

3. **投影（Projections）**

\[ \frac{\vdash u : P}{\vdash \pi_1(u) : A} \]
\[ \frac{\vdash u : P}{\vdash \pi_2(u) : B} \]

4. **等式约束（Equations）**
\[ \pi_1(a,b)=a \]
\[ \pi_2(a,b)=b \]
\[ (\pi_1(u),\pi_2(u))=u \]

---

## 构造（constructions）

前三条叙述了定义笛卡尔积所涉及的三种构造（constructions）。[^4] 

### 初始化（Initialization）
\[ \frac{\vdash A : Type \quad \vdash B : Type}{\vdash P : Type}\]

- 这表示：如果 \(A\) 和 \(B\) 是类型（Type），那么它们的笛卡尔积 \(P\) 也是一个类型。也就是说，笛卡尔积类型是一个新的类型，由两个给定的类型**构造**而成。
- 在编程中，\(P\) 即 \(A \times B\) 可被写作形如`prod(A,B)`的前缀形式。
- 在范畴论，相当于：
\[ A, B \in \mathcal{C} \Rightarrow A \times B \in \mathcal{C} \]

### 配对（Pairing）
\[ \frac{\vdash a : A \quad \vdash b : B}{\vdash (a,b) : P} \]

- 这表示：如果 \(a\) 是 \(A\) 的元素，\(b\) 是 \(B\) 的元素，那么我们可以**构造**一个有序对 \((a, b)\)，它属于 \(A \times B\)。
- 在编程中， \((a, b)\) 可被写作形如`pair(a,b)`的前缀形式。
- 在范畴论，相当于图示：![配对](https://mathagape.github.io/blog/images/cartesian-product-category-1-pairing.png)

```
\documentclass{article}
\usepackage{amsmath, amssymb}
\usepackage{tikz-cd}

\begin{document}

\[
\begin{tikzcd}
  & Q \arrow[dl, "a"'] \arrow[d, "{(a,b)}"] \arrow[dr, "b"] & \\ 
  A & P & B
\end{tikzcd}
\]

\end{document}
```

### 投影（Projections）
\[ \frac{\vdash u : P}{\vdash \pi_1(u) : A} \]
\[ \frac{\vdash u : P}{\vdash \pi_2(u) : B} \]

- 这表示：如果 \(u\) 是 \(P\) 的一个元素（即某个 \((a, b)\)），那么我们可以通过投影**构造**（提取）出它的第一个分量 \(\pi_1(u)\) 和第二个分量 \(\pi_2(u)\)，分别属于 \(A\) 和 \(B\) 。
- 在编程中， \(\pi_1(u)\) 、\(\pi_2(u)\) 可被写作形如`proj_1(u)`、`proj_2(u)`的前缀形式。
- 在范畴论，相当于图示：
![](https://mathagape.github.io/blog/images/cartesian-product-category-2-projections.png)
```
\documentclass{article}
\usepackage{amsmath, amssymb}
\usepackage{tikz-cd}

\begin{document}

\[
\begin{tikzcd}
  & Q \arrow[d, "{t}"] & \\ 
  A & \ P \ \arrow[l, "\pi_1"] \arrow[r, "\pi_2"'] & B
\end{tikzcd}
\]

\end{document}
```

---

## 公理（Axioms）

但若仅仅只有构造，一切还都只是初步的框架，一些空壳符号的连接，意义还尚未成形。ho，就好像一具人的躯体（Körper），而非一个活生生的身体（Leib）[^5]，它缺少灵魂！

而等式（equations）是代数（algebra）思维的重要观念。

因此，需要再加上**等式**将构造加以**约束**，这就是**公理（axioms）**，灵魂注入，ho！

---

### 等式约束（Equations）

\[ \pi_1(a,b)=a \]
\[ \pi_2(a,b)=b \]

- 这体现了存在性。

\[(\pi_1(u),\pi_2(u))=u\]

- 这体现了唯一性。

- 在范畴论，相当于图示：![](https://mathagape.github.io/blog/images/cartesian-product-category-axioms.png)
```
\documentclass{article}
\usepackage{amsmath, amssymb}
\usepackage{tikz-cd}

\begin{document}

\[
\begin{tikzcd}
  & Q \arrow[dl, "a"'] \arrow[d, "{(a,b)}"] \arrow[dr, "b"] & \\
  A & \ P \ \arrow[l, "\pi_1"] \arrow[r, "\pi_2"'] & B
\end{tikzcd}
\]

\end{document}
```

[^1]: 笛卡尔积的简明定义可参考 *The Joy of Abstraction* P.245：Let \(A\) and \(B\) be sets. Then the *cartesian product* \(A \times B\) is the set defined by \( A \times B = \{(a, b) \mid a \in A, \, b \in B\} \).
The elements \((a, b)\) are called *ordered pairs*, and there are functions \(p\) and \(q\) as shown on the right called *projections*, sending
an element \((a, b)\) to \(a\) and \(b\) respectively. 为了配合类型论的后文，我在书写上作了自己的改动。例如，我用 \( P \) 代替 \( A \times B \)，以强调把笛卡尔积看作一个整体，即一个类型，或一个范畴的对象，暂且不去考虑它内部更多的细节和意义，就像画人的时候先不去考虑五官的细节和意义，只勾出最简单的外轮廓。
[^2]: 按照库拉托夫斯基（Kuratowski）定义，有序对 $(a, b)$ 可以定义为\( (a, b) = \{\{a\}, \{a, b\}\} \)，用集合嵌套的形式来表示有序对的序。特别地，当 \(a = b\) ，例如有序对 \( (2,2) \) ，则可表示为 \( \{\{2\}\} \)，因为 \( (2,2) = \{\{2\}, \{2,2\}\} = \{\{2\}, \{2\}\}\) ，而 \( \{\{2\}, \{2\}\}\) 又可化简为 \( \{\{2\}\} \)。
[^3]: 这里的“初始化（Initialization）”是我自创的一个词。*Andrej Bauer* 似乎并未给这个步骤命名，直接称为 Contruction。
[^4]: 我没办法严格区分 functions、mappings、operations、constructions，我暂且把它们视为近义词甚至同义词。我出于自己的理解和习惯在相应语境下使用它们：如果强调计算规则，用 "function"（函数）；如果强调集合间的关系，用 "mapping"（映射）；如果强调对元素的操作，用 "operation"（运算）；如果强调如何创建新对象，用 "construction"（构造）。而在类型论语境，我会更多使用"construction"。
[^5]: 这里是一个我把数学与现象学结合的脑洞。区分“躯体（Körper）“与”活生生的身体（Leib）”的说法，主要来自胡塞尔（Edmund Husserl）的现象学研究；后被梅洛-庞蒂（Maurice Merleau-Ponty）在《知觉现象学》（Phénoménologie de la perception）中进一步发展。“Leib”不仅是物理上的身体，而且是体验世界的主体，具有感知、意向性和运动能力，而“Körper”只是作为客体存在的身体，与外部世界的物理对象无异。关于这些，我是从扎哈维（Dan Zahavi）的《现象学入门》（Phenomenology: The Basics）首次接触的。