本文介绍语意理解的第二个阶段——**与上下文无关的语义**（context-independent meaning），而**逻辑形式**（Logical Form）即是它的表示方式。如自然语言理解（一）中介绍的，我们把将一个句子映射到它的逻辑形式这个过程称为**语义理解**（semantic interpretation）。

为什么我们需要语言的逻辑形式呢？这是因为同样的一句话，在不同的语境（上下文）下可以表达出不同的意思。我们来看一个简单的中文例子：
*“你吃饭了吗？”*
看似非常简单的句子，其实从深层的语义角度可以表达不同的意思。场景一：想象一下你傍晚走在街上碰到了同学，这是一句再也熟悉不过的招呼语。场景二：有一天中午你在宿舍，突然发现自己很饿了，然后向室友问了这么一句。这时你想表达的意思是：“没吃饭就一起去啊！饿死我了！” 场景三：你和女友潇洒的出去美餐了一顿，回到宿舍看到室友还在蹲着打游戏，这时候你问的这一句可能表达的是一种疑问（得瑟？）。。。

由此可见，同样一句话在不同的上下文中会产生不同的意思（这再次说明自然语言理解有多难）。虽然我们的终极目的是想得到句子在上下文中的意思，但想一步登天实属太难。因此我们希望能够定义一种与上下文无关的句子语义，这便是逻辑形式语言了。本文主要从以下三个部分进行介绍：

 1. 基本的逻辑形式语言
 2. 语义角色
 3. TRIPS 逻辑形式

----------
**基本的逻辑形式语言**

顾名思义，逻辑形式语言便是将句子以逻辑的方式表示，也正因为这种表示只涉及逻辑关系，所以是与上下文无关的。逻辑形式语言与一阶逻辑，或称一阶断言计算（FOFC）有着很多相似的地方。由于篇幅原因，不熟悉一阶逻辑的同学请自行谷歌了（其实简单说来就是与或非加上量化表示（存在 ／ 所有））。

逻辑形式语言中，词义（word sense）被视为原子，或者常量，即它们是逻辑形式中的最小单位。根据词义所描述类型的不同，这些常量又可以被分为两类：1、表示对象（object），包括抽象对象的常量叫项（term）。（好奇葩的名字。。我不会翻了。。）2、描述关系和性质的常量叫做断言（predicate）。有了这些要素，一个简单的陈述句便可以用这样的方式表示：断言 ＋ （适量的）项。 其中根据断言后所加项的个数可以将断言分为单元断言和多元断言。我们来看两个例子吧：
Fido is a dog.          -->          (DOG1 FIDO1)
Sue loves Jack.       -->          (LOVES1 SUE1 JACK1)

怎么样，是不是感觉很简单？若想句子复杂一点，我们可以加入一阶逻辑的东西：

Sue does not love Jack.         -->          (NOT (LOVES1 SUE1 JACK1))
Jack loves Sue or loves Mary     -->       (OR1 (LOVES1 JACK1 SUE1) (LOVES1 JACK1 MARY1)

相信这些例子都很好接受，我就不再赘述了，重要的是要会举一反三哦。
接下来我们再加入广义量词（generalized quantifier），这是模式就变成：
（量词：受限的对象 主体）
看几个例子：
Most dogs bark.     -->      (MOST1 d1: (DOG1 d1) (BARKSx d1))
The dogs bark.       -->      (THE x: (PLUR DOG1) x) (BARKS1 x))

其实基本的逻辑形式语言还有很多内容，比如如何处理词语的歧义。在此只希望介绍逻辑形式语言的思想，就不详述了。有兴趣的同学可以阅读参考文献的内容。


----------
**语义角色**

语义角色（semantic role)，翻译起来怪怪的词语，但是是很有意思的内容。十分形象的名字，语义角色就是*句子中各个词组对于动词而言所扮演的角色（作用）*。语义角色分为核心角色（core role）和关系角色（relation role）。核心角色是指那些与动词语义具有内在联系、本质关系的角色，比如动作的发起者、影响者；而关系角色则是其他辅助的角色，如时间、方法、结果等。各个语义角色的定义和例子请看下表（专有名词，就不翻译了）：

核心角色  | 定义 | 例子
---- | ---  | ---
AGENT | 事件的起因或造成者 | *The storm* destroy the house
AFFECTED    | 事件的影响者 | The storm destroy *the house*
AFFECTED-RESULT     | 在事件的最后受到改变 | We baked *a cake*
NEUTRAL | 无因果关系的对象，但有存在性 | I want *a pizza*
FORMAL    | 无因果关系的对象，无时间存在性 | He believes *that he is right*


----------

关系角色  | 定义 | 例子
---- | ---  | ---
RESULT | 有关事件的最终状态 | He swep the clubs *from the table*
LOCATION    | 有关事件的发生地点 | He sang *in the corner*
METHOD     | 标志两个事件的因果关系 | He opened the door *with a hammer*
MANNER | 标志事件的质量和风格 | He walked *slowly*
TIME    | 有关事件的时间位置 | He arrived *at 2 o'clock*
EXTENT | 事件在某个维度上的大小 | I spent *twenty dollars*
FREQUENCY    | 事件发生的频率 | He *rarely* comes to meetings
BENEFICIARY     | 事件发生的受益者 | I open the door *for him*
MOTIVATION | 事件发生的目的 | He unlocked the door *to let him in*


----------
彻底掌握不同语境下细分各个语义角色不是一件简单的事情，也不是光盯着定义看就能明白的事情。它需要你多看例子，从例子中慢慢揣摩出规律，这也是一种乐趣所在呐！（话说复习的时候，一遍一遍的刷例句然后判断新的句子，感觉自己就是一个分类器。。看完训练数据之后再在测试数据上预测结果。。。）


----------
**TRIPS 逻辑形式**

TRIPS 是 The Rochester Interactive Planning System（罗切斯特交互计划系统）的缩写，是由美国罗切斯特大学 [James Allen](https://www.cs.rochester.edu/~james/%20James%20Allen)教授（也就是我这门课的老师啦。。。）团队领导的一个人工智能交互的项目， TRIPS逻辑形式是其中的一部分研究成果。TRIPS逻辑形式结合了第一部分的基本逻辑形式和第二部分的语义角色，（在我看来）是一个比较完备的逻辑形式语言。我们先来看个例子：
But the man wants to eat it.
这句话转化为TRIPS逻辑形式如下：
(SPEECHACT V1 ONT::SA_TELL :CONTENT w1 :MOD b1)     | 这是一个TELL类型的语句，包含内容w1和修饰b1
-------- | ---
(F w1(:* ONT::WANT W::WANT :FORMAL e1 :NEUTRAL m1 :TMA((W::TENSE W::PRES))) | 内容w1时一个关于e1和m1的事件，时态是现在时
(THE m1 (:* ONT::MALE W::MAN))    | m1在内容中是一个指代的man
(PRO i1 (:* ONT::REFERENTIAL-SEM W::IT) :PRoform W::IT)     | i1是一个"it"表示的代词对象
(F e1 (:*ONT::CONSUME W::EAT) :AFFECTED i1 :AGENT m1))    | e1是一个关于i1和m1的吃的关系
(F V12 (:* ONT::CONJUNCT W::BUT) :OF w1))     | w1受到反义关系"but"的修饰


----------
乍一看这个TRIPS逻辑形式还是颇为复杂的，但其实它包含了几个关键的部分，让我们就着上面的例子一一道来。

 1. Ontology
		 逻辑形式中每个原子的类型都由其在逻辑形式Ontology的分层结构给出。比如“eat”这个词，当他表示“吃这个意思是，它就是在ONT::CONSUME（消费）这个类别下的。
		 有了ontology之后，我们就可以将它用于简单描述当中，简单描述有点类似于第一部分介绍的基本逻辑形式语言中的简单断言。比如：the train --> (THE x (:* ONT::VEHICLE W::TRAIN))；trains --> (INDEF-SET x (:* ONT::VEHICLE W::TRAIN))。
 2. SpeechAct
	 描述句子陈述的类型。上例中的SA_TELL表示句子是陈述句， 也是最常用的句式。当然也可以有其他句式，比如说“Wh开头的疑问句”（Where is the knife?）用Wh-Question表示。
 3. F
	F 用来标识简单事件或者关系。模式为：F ＋ 事件编号 ＋ 动词 ＋ 语义角色。
	例子：A man loaded the cargo. 
	--> (F l1 (:* ONT::FILL-CONTAINER W::LOAD) :AGENT m1 :AFFECTED c1)
	（省略之后对m1和c1的补充）
 4. PRO
	PRO 用来描述代词，其中还有一个类别来指代所有类型的对象（如果只带对象类型不明是可以用）：ONT::REFERENTIAL-SEM。并且后面加上一个特殊的角色：PROFORM来表明与句子内容的关联信息。具体看上面的例子。	
 5. :MOD
	 :MOD表示修饰，它用处非常广泛，可以用来添加简单的修饰，也可以用作添加连词（见上例）。
	 用作简单修饰的例子: The red truck
	 --> (THE V1 (:* ONT::LAND-VEHICLE W::TRUCK) :MOD V2)
			 (F V2 (:* ONT::RED W::RED)  :OF V1))

关于TRIPS逻辑形式语言的内容还有很多很多，包括疑问句、量词、比较级和最高级、时间地点数量等等， 在此不能一一而列，有兴趣的同学可以阅读参考文献。

［1］NATURAL LANGUAGE UNDERSTANDING, JAMES ALLEN, SECOND EDITION
［2］The TRIPS Logical Form