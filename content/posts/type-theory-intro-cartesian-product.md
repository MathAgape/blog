+++
date = '2025-02-28T13:45:53+08:00'
title = '初识类型论：以笛卡尔积为例 [WIP]'
categories = ["Mathematics"]
tags = ["Type Theory", "Category Theory"]
+++

非常感谢一位朋友推荐的类型论入门课程[MAT-FORMATH by *Andrej Bauer*](https://www.youtube.com/playlist?list=PL-47DDuiZOMDBfb5t8Hd30utd_TopoQLE)，第一集([2024-10-04 Type theory](https://www.youtube.com/watch?v=OEGXNEPddYw&t=2305s))以**笛卡尔积**为例子，让我首次触摸到了类型论、集合论、范畴论的微妙关联。

也非常感谢这位朋友之前推荐的范畴论入门书籍 **The Joy of Abstraction** by *Eugenia Cheng* 以及网站 [nLab](https://ncatlab.org/nlab/show/HomePage)。

如果没有这些无私分享，我是绝无可能了解到这么新颖有趣的数学。感谢他们没有故意建起知识的护城河，而是为初学者打破了第一道信息壁垒。

结合以上参考资料，我写下了这篇学习笔记。初学思考，严谨不足，仅供交流。文末附有**后记**，简述了我业余自学数学的奇妙旅程。

<!--more-->

---

我们**通常**这样定义笛卡尔积：

设\(A\)和\(B\)为两个集合。**笛卡尔积（cartesian product）** 被定义为集合[^1]：

\[ A \times B = \{(a, b) \mid a \in A, \, b \in B\} \]

其中的元素 \((a, b)\) 称为**有序对（ordered pair）**。

注意到，当笛卡尔积的定义写下，就已经暗示了集合 \( P \) 配有两个自然**投影（projections）** \(\pi_1\) 和 \(\pi_2\) ：\( \pi_1: A \times B \to A\)，\(\pi_2: A \times B \to B \)；其中， \(\pi_1(a, b) = a\)，\(\pi_2(a, b) = b\)。

---

以上是笛卡尔积的通常定义，如果要写成纯粹的**一阶逻辑(First-Order Logic)** 形式，则是：

\[ \forall x \, ( x \in A \times B \leftrightarrow \exists a \, \exists b \, (a \in A \land b \in B \land x = \{\{a\}, \{a, b\}\})) \]

其中，用嵌套的集合表示有序对，是基于库拉托夫斯基（Kuratowski）定义[^2]。

---

以上是经典**集合论**和**数理逻辑**思维下的定义。
但这样定义还不够自然——尤其是在使用计算机实现辅助证明时。因此，我们需要引入**类型论**，作为一种新的数学形式化（formalism）方法。

笛卡尔积可用**类型论**的语言表示为如下五步[^3]：

1. **初始化（Initialization）**

\[ \frac{\vdash A : Type \quad \vdash B : Type}{\vdash P : Type}\]

2. **配对（Pairing）**

\[ \frac{\vdash a : A \quad \vdash b : B}{\vdash u : P} \]

3. **投影（Projections）**

\[ \frac{\vdash u : P}{\vdash \pi_1(u) : A} \]
\[ \frac{\vdash u : P}{\vdash \pi_2(u) : B} \]

4. **等式约束1（Equations 1）** 

\[ \pi_1 (u)=a \]
\[ \pi_2 (u)=b \]

5. **等式约束2（Equations 2）**

\[ (\pi_1 (u),\pi_2 (u))=u \]

其中，\( P \) 就是笛卡尔积 \( A \times B \)，\( u \) 就是有序对 \( (a,b) \)[^4]。

---

## 构造（constructions）

前三条叙述了定义笛卡尔积所涉及的三种**构造（constructions）**。[^5]

---

### 构造：初始化（Initialization）
\[ \frac{\vdash A : Type \quad \vdash B : Type}{\vdash P : Type}\]

- 这串公式用自然语言说，就是：如果 \(A\) 和 \(B\) 是类型，那么它们的笛卡尔积 \(P\) 也是一个类型，它是基于类型 \(A\) 和 \(B\) 生成的新类型。
- 在编程中，\(P\) 即 \(A \times B\) 可被写作形如`prod(A,B)`的前缀形式。
- 在集合论，就是：\(A\) 和 \(B\) 是集合，它们的笛卡尔积 \( A \times B \) 也是一个集合。
- 在范畴论，就是：\( A, B \in \mathcal{C} \Rightarrow P \in \mathcal{C} \)。

---

### 构造：配对（Pairing）
\[ \frac{\vdash a : A \quad \vdash b : B}{\vdash u : P} \]

- 这串公式用自然语言说，就是：如果 \(a\) 的类型是 \(A\)，\(b\) 的类型是 \(B\) ，那么 \(u\) 的类型是 \(P\)。
- 在编程中，\(u\) 即 \((a, b)\) 可被写作形如`pair(a,b)`的前缀形式。
- 在集合论，就是：如果 \(a\) 是 \(A\) 的元素，\(b\) 是 \(B\) 的元素，那么 \(u\) 是 \((a, b)\) 的元素。
- 在范畴论，就是：

![](https://mathagape.github.io/blog/images/cartesian-product-category-1-pairing.png)

```
\documentclass{article}
\usepackage{amsmath, amssymb}
\usepackage{tikz-cd}

\begin{document}

\[
\begin{tikzcd}
  & Q \arrow[dl, "a"'] \arrow[d, "{u}"] \arrow[dr, "b"] & \\ 
  A & P & B
\end{tikzcd}
\]

\end{document}
```

---

### 构造：投影（Projections）
\[ \frac{\vdash u : P}{\vdash \pi_1(u) : A} \]
\[ \frac{\vdash u : P}{\vdash \pi_2(u) : B} \]

- 这串公式用自然语言说，就是：如果 \(u\) 的类型是 \(P\)，那么 \(\pi_1(u)\) 的类型是 \(A\)。
- 在编程中， \(\pi_1(u)\) 、\(\pi_2(u)\) 可被写作形如`proj_1(u)`、`proj_2(u)`的前缀形式。
- 在集合论，就是：对于有序对 \((a, b)\) 总能通过投影\(\pi_1\) 、\(\pi_2\) 提取出它的第一个分量 \(\pi_1(a, b)\) 和第二个分量 \(\pi_2(a, b)\)，分别属于 \(A\) 和 \(B\) 。
- 在范畴论，就是：

![](https://mathagape.github.io/blog/images/cartesian-product-category-2-projections.png)

```
\documentclass{article}
\usepackage{amsmath, amssymb}
\usepackage{tikz-cd}

\begin{document}

\[
\begin{tikzcd}
  & Q \arrow[d, "{u}"] & \\ 
  A & \ P \ \arrow[l, "\pi_1"] \arrow[r, "\pi_2"'] & B
\end{tikzcd}
\]

\end{document}
```

---

## 公理（Axioms）

但若仅仅只有构造，一切还都只是初步的框架，一些空壳符号的连接，意义还尚未成形。ho，就好像一具人的躯体（Körper），而非一个活生生的身体（Leib）[^6]，它缺少灵魂！

而等式（equations）是代数（algebra）思维的重要观念。

因此，需要再加上**等式**将构造加以**约束**，这就是**公理（axioms）**，灵魂注入，ho！

---

### 等式约束1（Equations 1）

\[ \pi_1 (u) = a \]
\[ \pi_2 (u) = b \]

这两个等式为结构赋予意义：对任意有序对总是**存在（exist）** 投影。
对应范畴论**泛性质（universal properties）**中的**存在性（existence）**。

![](https://mathagape.github.io/blog/images/cartesian-product-category-axioms.png)

直观可见，这两个等式形成了两个**交换三角形（triangle commutes）**。
```
\documentclass{article}
\usepackage{amsmath, amssymb}
\usepackage{tikz-cd}

\begin{document}

\[
\begin{tikzcd}
  & Q \arrow[dl, "a"'] \arrow[d, "{u}"] \arrow[dr, "b"] & \\
  A & \ P \ \arrow[l, "\pi_1"] \arrow[r, "\pi_2"'] & B
\end{tikzcd}
\]

\end{document}
```
---

### 等式约束2（Equations 2）

\[(\pi_1(u),\pi_2(u))=u\]

这两个等式为结构赋予意义：有序对的分解总是**唯一（unique）**。
对应范畴论**泛性质（universal properties）**中的**唯一性（uniqueness）**。

---

通过这样一番探索，我们可以发现，我们似乎总用不同的方式来诉说同一个数学本质，类型论、计算机代码、集合论、范畴论，就好像各种语言变体，我愿称之为方言（dialects）[^7]，每一种方言都在显示本质的某一侧。

类型论与计算机代码有一种更自然的相通，是一种非常便于计算（compute）的形式化方法，这为后面 LEAN 的辅助证明打下了基础。

关于笛卡尔积在范畴论与类型论的对应，参考了[nLab 词条 cartesian product](https://ncatlab.org/nlab/show/cartesian+product)：
![](https://mathagape.github.io/blog/images/cartesian-product-nlab.png)

---

### 后记

我几乎快要遗忘，我第一次对数学产生强烈好奇的那个下午，当我看到第三次数学危机这一概念，就冒出一个念头：除了集合论，还有别的**什么论**吗？我用自己的办法搜遍全网，也没有找到答案。那会我对数学一无所知，只是好奇，想要看看更大的世界。

后来我开始艰难地自学数学，这对于一个本科是美术的人来说，着实困难。书都是靠着搜索随缘找的，一开始是看微积分，看不懂，然后看数学分析，还是看不懂，然后再看点集拓扑，终于有点头绪。那会我尝试在 Bilibili 做讲解数学的视频，因为我实在没办法，我周围没有一个能交流数学的人，只能这样自己当自己的老师，自娱自乐。（这些视频目前已经不在了，我删除了它们）

但我还是惦记着那个起初的问题：除了集合论，还有别的**什么论**吗？

看我视频的人多起来，和我交流的人也多起来。直到一位朋友推荐我阅读一本新书， **The Joy of Abstraction** ，这是一本**范畴论**的入门书，彻底刷新了我对数学的认知。

后来，我看数学实在累了，就转而学习计算机了。LISP 的方言 Scheme 是我人生所学的第一门计算机语言，参考书是经典的SICP，即《计算机程序的构造和解释》（Structure and Interpretation of Computer Programs）。在这本书中，我接触到 Lambda 演算。然后，我又冒出一个念头：我能不能自己写出一个程序，可以验证我的习题答案是否正确，甚至能为我搜寻证明所需要的条件？

我尝试过很多办法，甚至有一瞬间又想到那个起初的问题：除了集合论，还有别的**什么论**吗？

直到那位朋友又为我推荐了视频教程 [MAT-FORMATH by *Andrej Bauer*](https://www.youtube.com/playlist?list=PL-47DDuiZOMDBfb5t8Hd30utd_TopoQLE)，让我试试了解**类型论**与 LEAN。当时我对这些还一无所直，但我有一种极为强烈的直觉，就是这个了！

果然，就是这个。

[^1]: 我写这个笛卡尔积的定义参考了 *The Joy of Abstraction* P.245，原文如下：Let \(A\) and \(B\) be sets. Then the *cartesian product* \(A \times B\) is the set defined by \( A \times B = \{(a, b) \mid a \in A, \, b \in B\} \).
The elements \((a, b)\) are called *ordered pairs*, and there are functions \(p\) and \(q\) as shown on the right called *projections*, sending
an element \((a, b)\) to \(a\) and \(b\) respectively. 
[^2]: 按照库拉托夫斯基（Kuratowski）定义，有序对 \((a, b)\) 可以定义为\( (a, b) = \{\{a\}, \{a, b\}\} \)，用集合嵌套的形式来表示有序对的序。特别地，当 \(a = b\) ，例如有序对 \( (2,2) \) ，则可表示为 \( \{\{2\}\} \)，因为 \( (2,2) = \{\{2\}, \{2,2\}\} = \{\{2\}, \{2\}\}\) ，而 \( \{\{2\}, \{2\}\}\) 又可化简为 \( \{\{2\}\} \)。
[^3]: 我写这五步参考了 *Andrej Bauer* 的视频教程 ([2024-10-04 Type theory](https://www.youtube.com/watch?v=OEGXNEPddYw&t=2305s)) 。；“初始化（initialization）”一词，以及为 equations 分组，是我在整理思路过程中的自创。原教程中并未给这个步骤命名，直接称为 contruction，也并未给 equations 分组。
[^4]: 我作出符号上这样的代替，这是为了强调，我把笛卡尔积**看作一个整体**，即一个类型，或一个范畴对象，暂且不去考虑它内部更多的细节和意义，就像画画的时候，先不去考虑细节和意义，只用最简单的基础形状起形，等到大关系的构造完成，再向内填充细节和意义。
[^5]: 在目前语境下，functions、mappings、operations、constructions 可视为同义词，它们是同一个本质在不同语境下的不同显现。我出于自己的理解和习惯在相应语境下使用它们：如果强调计算规则，用 "function"（函数）；如果强调集合间的关系，用 "mapping"（映射）；如果强调对元素的操作，用 "operation"（运算）；如果强调如何创建新对象，用 "construction"（构造）。而在类型论语境，我会更多使用"construction"。
[^6]: 这里是一个我把数学与现象学结合的脑洞。区分“躯体（Körper）“与”活生生的身体（Leib）”的说法，主要来自胡塞尔（Edmund Husserl）的现象学研究；后被梅洛-庞蒂（Maurice Merleau-Ponty）在《知觉现象学》（Phénoménologie de la perception）中进一步发展。“Leib”不仅是物理上的身体，而且是体验世界的主体，具有感知、意向性和运动能力，而“Körper”只是作为客体存在的身体，与外部世界的物理对象无异。关于这些，我是从扎哈维（Dan Zahavi）的《现象学入门》（Phenomenology: The Basics）首次接触的。
[^7]: 在计算机领域，我们经常谈到方言。计算机方言（computer dialect）通常是指一种编程语言或计算机语言的变种或实现，它可能在语法、语义、特性、功能等方面有所不同，但依然保持语言的核心理念或设计原则。类似于自然语言的方言，计算机方言是在某个基本语言或平台上进行的扩展或修改。计算机方言的存在往往是由于需求的不同、实现方式的不同，或是为了适应特定领域或环境的需求。例如 Scheme 和 Racket 都是 LISP 的方言。