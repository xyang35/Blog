﻿这学期修了 Prof. Daniel Gildea 的 Statistical Speech and Language Processing 课程。作为 machine learning 的进阶课程，这门课的确难度不小。本文记录了这两天复习期末考所作的笔记，里面涵盖了本次课程涉及的主要内容（忽略了一些复杂、较为不重要的部分）。另外，列出了课上和复习期间领悟的一些心得，个人以为是上这门课最大的收获。水平有限，理解难免有误，请各位指正。

另外，推荐大家去看Coursera上哥大的[Natural Language Processing课程](https://class.coursera.org/nlangp-001)，讲的非常清楚，例子也很多。

## 总结

 1. NLP 课程主要介绍了以下三类应用：
   * POS Tagging：给句子中的每个词标记相应的词性
   * Parsing：分析得到句子的句法结构（通常得到一个句法树）
   * Machine Translation：将源语言表述的句子翻译为目标语言表述的句子
  其实这三类应用之间互有联系，方法也有相似之处。粗略地讲：Parsing 可以看作 Tagging 的进化版，因为它不仅需要对每个词进行标记，还需要标记出层次性的句法结构。因此虽然 Parsing 的算法同样是动态规划（或前向后向），但比 Tagging 要计算多一个维度的信息。MT 尤其是 Syntax based MT 与 Parsing 也有相似之处，只不过除了 Parse 源句子外还要注意两个平行语料库（parrallel corpus）的关系。
 2. 动态规划（DP）在 NLP 中尤为重要。只要涉及到解码问题（decoding），即给定数据（句子）和 模型 （参数）求最优解（序列），基本都要用到 DP 来求解。这就很自然地延伸到两个问题：
   * 为什么要用DP？
     因为可行解的数量太多（对于长度为$n$、状态个数为$T$的解码问题，可行解有$T^n$个），穷举法效率太低。而DP可以给出多项式时间复杂度的解法。
   * 为什么可以用DP？
     个人认为这涉及到NLP中模型设计。动态规划适用于有重叠子问题和最优子结构性质的问题。而 NLP 中所用的模型（如HMM，PCFG）的性质使其优化问题具有类似 “$max \prod$” 的形式。而 “$max \prod$” 可以转化为 “$\prod max$” 来求解（准确来说，需要每一项$\in\Re^+$，而概率一定$p\geq 0$因此满足该限制），故产生了重叠子问题和最优子结构的性质，从而可以用DP来求解。换句话说，NLP中模型的设计很重要的一点是：可以使用动态规划进行高效求解。
 3.  对于参数求解问题（parameters learning），一般会涉及到以下几种模型和算法，它们之间的优劣值得思考：
   * 隐马尔科夫模型（HMM）和期望最大化算法（EM）：EM常用于学习含有隐变量（hidden variables）的模型。它是一种迭代求解的算法，重复：E步—估计在当前模型参数下的数据分布，以及，M步—通过该分布利用最大似然法更新模型参数。HMM中的E步通过前向后向算法（其实也是动态规划）实现。HMM是最为经典的NLP统计模型，能够表示tags之间的时序依赖关系。个人觉得该算法的一个缺点是比较复杂，计算较慢
   * 感知机算法（Perceptron Algorithm）：由用于二分类的感知机算法延伸而来，若解码序列与真实序列不符，则更新相应参数的权重。感知机算法的优点是简单，并且可以使用任意定义的特征（相对的，HMM中只有两种特征 $f(y_i|y_{i-1})$ 和 $f(x_i|y_i)$）。感知机算法的缺点可能是由于模型简单效果并不是太好，而且是个确定性模型（deterministic）。
   * 条件随机场（CRF）：属于最大熵模型的一种，也可以看作指数线性模型。与生成式模型（generative）的HMM不同，CRF是判别式模型（discriminative），即HMM建模$P(X_1^N,Y_1^N)$ 而CRF建模$P(Y_1^N|X_1^N)$。另外，CRF没有隐变量，而且是无向图。CRF的学习可以用梯度下降算法，但也需要用到前向后向算法来计算当前模型参数下特征的分布。CRF的优点是与感知机一样可以使用任意特征，但相比感知机而言它是一个概率模型。
   * 结构化支持向量机（Structured SVM）：属于最大间隔模型，同样从用于二分类的SVM延伸而来，其模型可以与感知机算法进行类比。SVM其实跟感知机算法没有太大区别，甚至可以看成加入了L2正则化的感知机算法。而Structured SVM还有一项改变是：在限制条件中加入了损失函数（loss function），导致解码时需要把损失函数考虑进来（还是动态规划）。因此Structured SVM具有一个优点：可以结合不同的损失函数。
 4. 大部分模型学习的更新公式都具有相似的形式，即：加上真实的减去预测的（move forwards the y we saw and move away from the $\tilde{y}$ we expect to see)。例子有：Perceptron、CRF、Structured SVM。至于原因嘛，还需要深入思考。
 5. Q：为什么有了维特比解码（Viterbi），还需要后验解码（即HMM中的前向后向，Parsing中的Inside-Outside）？
   A：首先，维特比算法是求解解码问题的正确方法，保证解码后的序列得到的score最大。但我们在评价performance的时候，除了看该序列是否正确（这对于一个序列只是对和错的二分问题），还要看每一位解码的正确与否（如真实序列为ABCD，解码后序列为ABBD，虽然是错的，但有其中三位都是对的）。这样就会导致另一种损失函数： $L=\sum_iI(y_i\neq\tilde{y_i})$。而后验解码正是相对应这种损失函数评价下的解码算法，因此也叫做Minimum Bayes Risk （MBR）算法。由于这种评价方法的存在，后验解码通常会得到比维特比解码更好的结果。那么反过来，为什么我们还需要维特比算法呢？因为它更加简单高效（对比前向后向算法），而且它本来就是对的，只是评价标准不同而已。
 6. HMM可以看作CFG的特列，在笔记中略有介绍。
 7. SCFG部分在笔记里懒得写了，感兴趣可以参考这篇文章：[Introduction to synchronous grammar](http://www3.nd.edu/~dchiang/papers/synchtut.pdf%20Introduction%20to%20synchronous%20grammar) 
 8. 还有其他感悟，等我想起来再继续添加。。
## 笔记
地址：https://github.com/xyang35/course-NLP/blob/master/notes/nlp_note.pdf

目录：
1 Part of Speech (POS) Tagging
<ul>
	<li>Perception Algorithm</li>
	<li> HMM</li>
	<li>Conditional Random Field (CRF)</li>
	<li>Structured SVM</li>
	</ul>
	
2 Parsing
<ul>
	<li>Context Free Grammar (CFG)</li>
	<li>CKY Parsing (Viterbi Decoding)</li>
	<li>Posterior Decoding (Inside-Outside Algorithm)</li>
	</ul>
	
3 Machine Translation
<ul>
	<li>Word Alignment (IBM Models)</li>
	<li>Phrase Based Translation</li>
	<li>Syntax Based Translation (SCFG)</li>
	</ul>


